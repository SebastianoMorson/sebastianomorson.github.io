---
layout: post
title: Introduction to Recommender Systems
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
categories: article

---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_5716a8004da94ff89a3816430b5fe81b~mv2.png/v1/fill/w_258,h_390,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5716a8004da94ff89a3816430b5fe81b~mv2.png)
Immaginiamo che domani io trovi di fronte a casa una scatolina. Questa scatolina contiene una tecnologia futuristica che memorizza un numero immenso di informazioni sulle ragazze (o ragazzi, o cani, gatti, auto, quello che vuoi). Otterrei un potere immenso. Mi garantirei un matrimonio perfetto, ma perchè? Perchè potrei scoprire dei pattern di comportamento della mia compagna, per esempio cosa succede prima che si incazzi, in modo da adottare le adeguate contromisure prima che accada. Oppure potrei scoprire cosa le manca e cosa desidera, in modo da sapere sempre qual è il miglior regalo per ogni festività. Potrei personalizzare il regalo in base ai suoi interessi più profondi o capire attraverso le relazioni andate male cosa fare per far sì che non ricapiti. Chiaramente l'esempio è sciocco, ma è facile estendere l'esempio a ogni campo della nostra vita privata o lavorativa.

Il problema di avere una scatolina del genere è però se la perdiamo o ci viene sottratta. Da regalo divino diventerebbe il peggior incubo della persona/cosa interessata, perchè se da bravo fidanzato ho pensato a sfruttare i dati per migliorare la relazione, un utente malintenzionato potrebbe usarli per sconvolgere la sua vita o rovinare il nostro rapporto. Ecco quindi che il problema della privacy e dell'utilizzo improprio dei dati diventa un problema critico. Come i dati vengono memorizzati e chi li gestisce diventa fondamentale. Inoltre se anche volessi usare questi dati a fin di bene, mi sono posto la domanda di chi ha prodotto questi dati e se veramente rispecchiano la realtà? Cambiando scenario, immaginiamo che questa scatolina contenesse i dati dei crimini violenti perpetuati da persone categorizzate in "vecchie" o "giovani". I vecchi sono quelli che vanno dagli 80 anni in su, i giovani quelli che vanno dagli 0 agli 80 anni. Immaginiamo quindi di usare questi dati per decidere se un uomo debba andare in prigione o meno, in base alla percentuale di anziani e giovani che commettono crimini violenti. Sarebbe un metodo discriminante perchè ovviamente risulterebbe che la percentuale di giovani che commettono crimini è molto più bassa degli anziani, perchè quest'ultima categoria è minoritaria rispetto a quella degli under 80. Ecco quindi che emergono grossi problemi etici e di sicurezza nel momento in cui parliamo di questi dati. Ed ecco perchè ora più che mai bisogna porre sempre più il focus su tutto ciò che direttamente o indirettamente manipola e sfrutte l'enorme mole di informazioni prodotte quotidianamente tramite social, sistemi di videosorveglianza, e sistemi smart.

Questo preambolo serve a dare quindi un senso al primo articolo sui sistemi di raccomandazione, strumenti dove questi Big Data ne sono fondamentalmente la linfa vitale. Proprio per questo motivo, bisogna far chiarezza e capire in che modo li sfruttano. Bisogna capirlo per la nostra sicurezza e anche perchè avere un buon sistema di raccomandazione può far sì di risparmiarci inutili litigate su quale film vedere. Un buon sistema di raccomandazione infatti è in grado di soddisfare le esigenze della coppia, risparmiandoci mal di testa e nottate a dormire sul divano. Not bad.

## Cos'è un sistema di raccomandazione?

Un sistema di raccomandazione è un sistema che cerca elementi attinenti a quelli che possono essere le preferenze dell'utente o i suoi comportamenti. That's it.

Un algoritmo di raccomandazione fatto come si deve, deve avere 4 caratteristiche:

1. accurato: non voglio che mi consigli dei ristoranti in cui si mangia il pescecane solo perchè mi piacciono i pesci e mi piacciono i cani
    
2. efficiente: non voglio che per darmi un consiglio debba aspettare una settimana
    
3. scalabile: se il numero di elementi da poter consigliare aumenta, voglio che comunque sia in grado di suggerirmi in tempi ragionevoli e usando un numero di risorse ragionevole
	
4. adattabile: se le preferenze e i feedback cambiano, voglio che anche l'algoritmo cambi i consigli

Inoltre oltre a queste caratteristiche è necessario che il sistema offra suggerimenti
- recenti: non voglio che il sistema mi consigli di acquistare un cellulare dell'anteguerra quando cerco "cellulare". È meglio se mi suggerisce l'ultimo modello di Iphone (non sono pagato per dire che Apple è meglio di Android, per questo non dirò che Apple è meglio di Android)
    
- basati sul contesto: non voglio ricevere annunci sulle prossime feste del KKK se sono un afroamericano
    
- ripetuti: non voglio che si dimentichi cosa mi piaceva e a cosa sono interessato, indipendentemente se gli elementi che può consigliarmi vengono creati ogni secondo o ogni morte di Papa. Ad esempio nel caso dei film: magari mi piacciono i film di Russel Crowe, ma ne esce uno ogni 5 anni. Non è che dopo 5 anni l'algoritmo si deve dimenticare che a me piacciono un sacco quei film e non consigliarmi di vedere l'ultimo uscito!
    
- autonomamente: non voglio dover ogni volta cliccare "mi piace" per fargli capire cosa mi piace e cosa no

I sistemi di raccomandazione hanno però dei casi in cui si trovano in difficoltà, si incazzano, sbuffano e iniziando a svarionare:
- quando all'inizio non hanno sufficienti dati sull'utente o sull'elemento da cercare ( COLD START)
    
- quando l'utente cambia preferenze e loro devono farsi un mazzo tanto per riuscire a capire che diavolo gli passa per la testa
    
- quando si affidano troppo alle caratteristiche dell'utente, in particolare nei modelli collaborative
    
- quando l'utente ha poche preferenze, quindi il suggerimento finisce per essere troppo generico
    
- quando continuano a offrire raccomandazioni pescate dallo stesso insieme di elementi, quindi si creano delle "bolle" da cui non riescono a uscire

## Quanti tipi di sistemi di raccomandazione esistono?

La prima grande distinzione da fare è nel tipo di sistema di raccomandazione, esistono difatti 3 tipi di sistemi:

- collaborative filtering:
    
- content-based filtering
    
- hybrid

L'idea del collaborative filtering è quella di utilizzare i feedback impliciti ed espliciti degli utenti per offrire suggerimenti ad altri utenti, basandosi sull'idea che se un individuo ha determinate caratteristiche in comunque con un gruppo di altre persone e queste altre persone hanno certe preferenze, anche quell'individuo avrà gusti simili.

L'idea della content-based filtering è invece quella di studiare le caratteristiche delle preferenze dell'utente per individuare items che possiedano le medesime caratteristiche. Ad esempio se a un individuo piacciono gli oggetti gialli e tondi, il sistema cercherà degli elementi gialli e tondi e glieli consiglierà.

La differenza tra i due metodi sta quindi nel fatto che nel collaborative filtering l'analisi è condotta principalmente sulle caratteristiche dell'utente per cercare somiglianze con altri utenti, mentre nel content-based filtering c'è un'attenzione maggiore a quelle che sono le caratteristiche degli item che l'utente predilige, ma senza bisogno di analizzare le preferenze di altri soggetti.

Il metodo hybrid è invece il metodo che fonde collaborative e content-based filtering.