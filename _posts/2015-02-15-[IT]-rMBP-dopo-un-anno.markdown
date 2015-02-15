---
layout: post
title:  "[IT] Retina MacBook Pro dopo un anno"
date:   2015-02-15 17:30:00
categories: apple macbook pro retina rmbp italian
---

Ho comprato il mio rMBP come regalo di Natale 2014, sperando in una nuova macchina pronta a soddisfare la mia necessità di hardware potente, versatilità e facilità di trasporto.

Ora, dopo un anno, sono pronto a dare il mio giudizio riguardo la macchina in sé ed il software che includono con essa, OS X 10.{9, 10}.

Tralasciando (l'eccellente) design, alcune ottime scelte di progettazione e i materiali con cui è costruito, posso facilmente definire questo portatile come una enorme presa in giro nei confronti di chi cerca una macchina per **lavorare**, non fare l'hipster nello Starbucks di turno.

Apple definisce la linea *"Pro"* come le perfette macchine per produrre contenuti e in generale eseguire tutte quelle operazioni che un utente avanzato necessita di fare in mobilità.

Il problema è che **qualunque** revisione ora esistente di questa serie di portatili non riesce nell'intento.

Visto che stiamo parlando di computer, mi sembra giusto tirare fuori qualche dato tecnico, in particolare il mio rMBP ha le seguenti specifiche:

* schermo 13" *retina* 2560x1600
* Intel Core i5 4258U
* 8GB di RAM DDR3 1600MHz
* scheda video Intel Iris
* SSD 256GB PCI-E

Come potete notare non è una macchina estremamente potente, ma *dovrebbe* essere in grado di reggere con facilità operazioni complesse come molteplici VM in esecuzione, rendering video senza molte pretese e sviluppo software.

Il problema cruciale è la scelta dello schermo, ed il sistema operativo.

Tralasciando per un attimo la mia propensione a OS come Linux e ultimamente BSD, OS X nelle sue versioni 10.9 e 10.01 risulta essere il peggior sistema operativo che io abbia mai utilizzato da una decina d'anni a questa parte.

Sento già le vocine in lontananza che mi contraddicono dicendo "a me va così bene, non capisco perché a te vada così male".

Ecco, probabilmente questa persona non ha mai usato un rMBP per più di 10 minuti fuori da uno Store.

Apple quando ha progettato questa serie di portatili ha completamente dimenticato che anche l'OS deve essere adattato per far funzionare tutto al meglio.

Lasciate che vi spieghi come funziona il sistema HiDPI che Apple ha studiato per rendere al meglio su questi monitor.

Abbiamo una risoluzione decisamente alta, su uno schermo decisamente piccolo; come fare per rendere utilizzabile questo *coso*?

Moltiplichiamo tutti i componenti grafici per una costante e dividiamo la risoluzione nativa x2!

Di conseguenza un rMBP appena uscito dalla scatola è un portatile da 13" con risoluzione ((2560x1600)/2), quindi 1280x800.

Poco male, OS X permette all'utente di "trasformare" quella risoluzione (decisamente bassa in termini di spazio in cui lavorare) in qualcosa di più abbordabile per l'utente *pro*.

Ora che sappiamo come il sistema operativo gestisce questo losco affare, bisogna introdurre il concetto di "risoluzione retina" e "non-retina".

Una "risoluzione retina" fa quel giochetto di moltiplicazioni e divisioni che ho illustrato prima, una "non-retina" invece no, quindi c'è il classico effetto blur che si ha quando si imposta una risoluzione supportata ma non ottimale per un monitor.

Nelle impostazioni dello schermo infatti possiamo decidere di settare

* 1280x600
* 1280x800
* 1440x900
* 1680x1050

tutte retina.

"Il problema non sussiste, alla fine se scala lo farà solo all'atto della selezione della risoluzione" mi sono detto, prima di comprarlo.

#einvece.

OS X, dall'alto della sua intelligenza **scala in tempo reale componenti grafici e non**.

In pratica ogni volta che c'è da visualizzare qualcosa sullo schermo l'OS fa questo:

    Oggetto da visualizzare -> render alla risoluzione settata come non-retina -> render dell'asset precedente alla risoluzione retina

Quindi è come se su ogni componente venisse effettuato un rendering in due passaggi facendo calare di molto gli FPS delle animazioni ad esempio, e visto che OS X è *pieno* di animazioni il sistema rallenterà.

Non perché la CPU "non è un i7", non perché "8GB di RAM sono pochi per gestire un moderno sistema operativo", non perché "la GPU è lenta", ma perché l'OS è stato programmato **col culo** sotto questo punto di vista.

Questo problema non affligge solo i MacBook Pro ma anche i nuovi iMac con schermo retina 5K, la quale risoluzione non è altro che ((2560x1600) \*2), 5120x3200.

Ad avvalorare la mia tesi, c'è Windows 8.1.

Windows 8.1 al contrario di OS X gestisce in modo diverso gli schermi HiDPI, ed infatti le animazioni e l'interazione col sistema in generale sono fulminee, inattaccabili.

Infatti almeno secondo la mia **modesta** opinione, questo rMBP è una macchina perfetta per far girare Windows.

OS X è meglio lasciarlo a risoluzioni non-retina per ora, almeno finché Apple non si deciderà a smettere di produrre idiozie piene di bug e feature incomplete in favore di un massivo bugfix sul suo sistema operativo, che osano ancora definire "Un nuovo orizzonte di potenza".
