---
layout: post
title: V&V-Fair Transition System
truncated_preview: true
excerpt_separator: <!--more-->
categories: article
tags:
  - miscellaneous
---
<!--more-->
Una volta mi faceva schifo la matematica e ora invece trovo che sia fondamentale.

I Fair Transition Systems sono un modello utile a rappresentare sistemi/programmi reattivi, ossia che variano in base alle condizioni del contesto in cui si trovano e che non ci si aspetta abbiano una terminazione. 

Un sistema/programma tradizionale è un sistema che dato un input termina producendo un output, mentre un sistema reattivo è un programma che dato un contesto iniziale, esegue certe operazioni che possono variare in base agli stati in cui si troverà. 

Un esempio di sistema reattivo è un sistema operativo, il sistema SCADA di un automobile o cose di questo genere.

### Come definisco un FTS?
Un FTS è definito da una tupla così composta
$$
<\cal{V},\Theta,\cal{T}, \cal{J}, \cal{C}>
$$
Vi chiederete ovviamente cosa significa ciascuna di queste lettere. Allora:
- $\cal{V}$ è l'insieme *finito* delle variabili 
	- ***control variable***: ce n'è solo una 
	- ***data variable***: possono essere più di una e rappresentano caratteristiche (ad esempio il colore di un semaforo, etc.)
- $\Theta$ è una formula di stato ***soddisfacibile*** che rappresenta la condizione iniziale. Se uno stato soddisfa $\Theta$ è detto ***initial state***
- $\cal{T}$ è l'insieme *finito* delle transizioni
	- $\tau \in \cal{T}$ è una funzione del tipo $\tau: \Sigma \to 2^\Sigma$ (in pratica mappa da uno stato a un insieme di stati. Se mi trovo nello stato "semaforo verde" posso finire nello stato "mi butto sulle strisce", "attraverso la strada", "rimango fermo", ...) 
	- bisogna vedere $\tau$ come una funzione che specifica i successori di uno stato s
	- se $\tau(s)\ne \emptyset$ allora la transizione è abilitata
	- se $\tau(s)=\emptyset$ allora la transizione è disabilitata
- $\cal{J}$ tale che $\cal{J}\subseteq \cal{T}$, è l'insieme delle transizioni per cui è richiesta la proprietà di ***justice***
- $\cal{C}$ tale che $\cal{C}\subseteq \cal{T}$,  è l'insieme delle transizioni per cui è richiesta la proprietà di ***compassion***

#### Proprietà di Justice
La proprietà di justice dice questo: se arrivati a un certo stato posso fare qualcosa X (ad esempio arrivo in università e posso aprire la porta dell'aula) allora ***prima o poi*** eseguirò la transizione X (prima o poi aprirò la porta). "Non so quando, non so come, ma prima o poi vi troverò".
#### Proprietà di Compassion
La proprietà di compassion invece fa un'assunzione più forte. Dice che se ho una transizione X che è attiva un numero infinito di volte, allora la transizione verrà presa un numero infinito di volte. Cioè, in altri termini: ***escludo*** la possibilità che le transizioni attive un numero ***infinito*** di volte vengano prese solo in un numero ***finito*** di occasioni.

La differenza sostanziale tra justice e compassione è che:
1. se richiedo la compassion automaticamente richiedo la justice (perchè la compassion è una proprietà più forte della justice)
2. se richiedo la justice non è detto che ottenga anche la compassion (perchè prima o poi la transizione potrebbe essere presa, ma non per un numero infinito di volte)


#### Idling transition
Esistono delle transizione "idling" che in pratica non fanno niente, perchè non cambiano nè i valori delle data variables, nè quello della control variable. 

Possiamo quindi vedere le idling transition come transizioni neutre.
$$τ(sk​)={sk​}$$
Il senso pratico è quello di mantenersi in una specifica condizione senza fare nulla (pensiamo al condizionatore. Può rimanere acceso senza fare nulla finchè le condizioni di calore o umidità non cambiano abbastanza da dover far intervenire il condizionatore)

#### P-computation
Una p-computation non è altro che una sequenza di computazione $\sigma$ di un FTS che soddisfa le seguenti proprietà:
- **initiliaty**: $s_0$ soddisfa $\Theta$ 
- **consequentiality**: per ogni stato di indice j=1,2,... è vero che $s_{j+1}$ è un $\tau$-successore di $s_j$ per qualche $\tau$
- **justice**: non esistono transizioni $\tau \in \cal{J}$ che sono abilitate e non vengono mai prese 
- **compassion**: non esistono transizioni $\tau \in \cal{C}$ che sono sempre abilitate e vengono prese un numero finito di volte

**state P-accessible**: stato contenuto in una P-computazione
**RUN computation**: sequenza di computazione $\sigma$ che soddisfa solo le proprietà di initiality e consequentiality
