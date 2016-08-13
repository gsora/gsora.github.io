---
layout: post
title:  "Dissecting a fake breadwallet iOS app"
date:   2016-08-13 21:16
categories: ios security bitcoin breadwallet fake
---

```
Disclaimer: I do not work for breadwallet, anything you'll read in this article is not endorsed by them; I am fairly new to mobile app security, so expect errors.
```

---

Bitcoin is a nice way of thinking about money: you're your bank, anyone knows anything about you and your assets and vice versa, it's decentralized.

But since you're the only one responsible for your money, a single error can - and *will* - lead to bad consequences.[^1]

[^1]: Mt.Gox anyone?

Today I'll talk about this last part because since the beginning of august 2016, someone is trying to scam iOS Bitcoin holders - often newcomers - faking one of the most used wallet on this platform, **breadwallet**.

iOS is a fairly secure platform, but this fake entity found a method to circumvent the AppStore app review process, uploading many fake version of breadwallet.

These fake versions tend to have the same behaviour: they are almost identical to official apps in both look and functionality, but they steal coins instead.

![apps]

[apps]: /assets/images/breadywallet-apps.png
{: style="margin: 0 auto; display: block; width:36% "}

## iOS application reverse engineering 101

Reversing a binary built for iOS isn't a straightforward process, because Apple encrypts every binary they push on the AppStore.

To be able to reverse anything, the first thing to do is proceed to jailbreak a physical iOS device - in this case I used an iPhone 6s.

This way it's far more easier decrypt a packaged AppStore binary, and it's possible to take a look to what the application does on the device itself: since I'm looking for anomalies, better look everywhere.

Then, I needed something to analyze.

A quick search on the AppStore for "breadwallet" gave me this interesting clone called "breadywallet" (notice the extra 'y'), that appears as "Hangman" on the SpringBoard.

![appstore]

[appstore]: /assets/images/breadywallet-appstore.png
{: style="margin: 0 auto; display: block; width:36% "}

Of course, I already downloaded (and used with much success) the official application on my device.

To decrypt an AppStore binary I used the excellent [Clutch](https://github.com/KJCracks/Clutch), as suggested by the [iPhoneDevWiki](http://iphonedevwiki.net/index.php/Reverse_Engineering_Tools#Clutch).

AppStore binaries are placed in `/var/containers/Bundle/Application`, but the folder containing actual binaries are renamed after an UUID - a quick bash oneliner can help us find the wallets:

```
iDjentleman:~ root# find /private/var/containers/Bundle/Application -maxdepth 2 -type d -name "*wallet.app" -exec echo {} \;
```

Running this command will reveal something interesting:

```
/private/var/containers/Bundle/Application/4F76E2BF-3A60-4349-B8CA-52B808108981/breadwallet.app

/private/var/containers/Bundle/Application/BB79C9DE-BBA9-4B39-B959-E085EEC67682/breadwallet.app
```

Which one is the official app's folder?

More chaos for the end-user is advisable in this situation, I guess.

How can we distinguish between the fake app and the official one?

Every iOS application embeds a metadata file - `iTunesMetadata.plist`; using `plutil` we can check for the software Bundle ID (which should be unique for every application on the AppStore):

```
# cd /private/var/containers/Bundle/Application/4F76E2BF-3A60-4349-B8CA-52B808108981/
# plutil iTunesMetadata.plist | grep softwareVersionBundleId
    softwareVersionBundleId = breadywallet;
```

Oh! Looks like we found the impostor!


After installing Clutch on the device, decrypting the binary is a matter of seconds:

```
iDjentleman:~ root# ./clutch  -b breadywallet
ASLR slide: 0x1000b8000
Dumping <breadwallet> (arm64)
Patched cryptid (64bit segment)
Writing new checksum
Finished dumping breadywallet to /var/tmp/clutch/4B395DF4-9644-4A5D-8F03-4F2494FF6FB2
Finished dumping breadywallet in 0.9 seconds
```

Clutch only managed to decrypt the arm64 build because my iPhone is on iOS 9.3.2 and this major release introduced Bitcode.

Bitcode permits building IPAs specifically tailored for each iOS device Apple currently supports, including in the binary only the correct architecture for each of them - since I'm on an arm64 device, the only segment Clutch found was the 64-bit one.

I decrypted the real breadwallet too, for the sake of comparison.

All these operations have been executed via SSH directly on the device, now we can take the reverse engineering process on a PC.

## A rough idea lead to correct conclusions

My first thought was:

    this rogue app steals Bitcoin but at the same time it's a totally legit wallet too, so it's working using the breadwallet's methods and functions to send coins to a wallet controlled by the offender.

this means the faker could have "engraved" this address in the binary.

`strings` to the rescue!

A Bitcoin wallet address is simple[^2]:

[^2]: [ref](https://en.bitcoin.it/wiki/Address); the method I describe here doesn't 100% validate an address

 - its length goes from a minimum of 25 characters, to a max of 35
 - it starts with a "1" or a "3"

Again, bash will help us achieve what we need:

```
#!/bin/bash
for line in $(cat $1); do 
	if [[ ${#line} -ge 26 && ${#line} -le 35 ]]; then
		if [[ $line == 1* || $line == 3*  ]]; then
			echo $line
		fi
	fi
done
```

Running this script against the strings we took earlier, somethings pops out:

```
12HzbRzNumLxnRxNY72eosNBC98bWHwE85
```

A Bitcoin address!

## Sneaky-beaky like

There's a major difference between the legit binary and the fake one: a method.

All the interesting action happends inside the `BRSendViewController` class.

Inside the fake breadwallet there's a method called `makeTest`, which have an almost identical flow compared to the legit `payToClipboard`; this last method basically send a fixed amount of coins to the address the OS clipboard contains.

`makeTest` does something similar: it creates a payment request for the fraud address instead of the one contained into payToClipboard, then displays a fake error message:

```
Wallet decryption error, check phone for unwanted programs"
```

tricking the user into thinking his/her phone has been hacked.

When the payment view will be presented, `makeTest` will be called.


## Conclusions

Using a common blockchain explorer we can clearly see that this address was not used until the 1st of August 2016; during the period between its first transaction and the 13 of August, this address accumulated *~47BTC*, and widthrawn everything to multiple addresses the 12 of August.

At the current exchange value, 47BTC can be converted to about *27k$*.

breadwallet is [open source](https://github.com/voisine/breadwallet/), this means the faker blindly cloned the repository and didn't even bothered changing the UI at all, but just the bundle ID and the SpringBoard app name.

This means Apple doesn't put a lot of efforts into the app review process, just a single letter changed in the name and boom, goes directly on the AppStore!

The review process for this kind of app (and all the Finance ones, too), needs to be completely renewed, putting more effort into finding dupes and fakes.

Yesterday (12 August), Apple removed the application from their stores worldwide, but I think a new fake will come up soon.
