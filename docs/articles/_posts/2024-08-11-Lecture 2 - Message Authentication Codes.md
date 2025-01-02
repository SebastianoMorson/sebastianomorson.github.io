---
layout: post
title: Lecture 2 Network Security - MAC
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
Quando parliamo di comunicazione attraverso una rete, potremmo incappare nei seguenti attacchi:
- disclosure
- traffic analysis 
- masquerade
- content modification
- sequence modification
- timing modification
- source repudation
- destination repudation

La **message authentication** è la procedura per verificare che il messaggio ricevuto arriva da una presunta sorgente non è stato alterato durante il percorso (volontariamente o involontariamente).

# How MAC works
![[1715177156_grim.png]]

A MAC function is similar to encryption. One difference is that the MAC algorithm need not be reversible, as it must be for decryption. In general, the MAC function is a many-to-one function. The domain of the function consists of messages of some arbitrary length, whereas the range consists of all possible MACs and all possible keys

Un MAC dovrebbe:
- impedire di poter creare un MAC uguale a quello di un altro messaggio o di scovare una chiave che genera lo stesso MAC 
- permettere solo di svolgere attacchi a forza bruta su un certo plaintext 
- l'algoritmo di autenticazione non dovrebbe essere più debole su un sottoinsieme di bit del messaggio

## HMAC 
Un HMAC è un MAC derivato da una funzione di hash.
L'idea è sfruttare la velocità delle funzioni di hash rispetto a quelle di encryption simmetrico

![[1715180084_grim.png]]

Efficient implementation of HMAC:
![[1715180125_grim.png]]

È stato dimostrato che la sicurezza di HMAC dipende dalla forza della funzione di hash utilizzata. 

## Data Authentication Algorithm (DAA)
![[1715180667_grim.png]]

## ChiperBased Message Authentication Code (CMAC)
![[1715180716_grim.png]]

# Authenticated Encryption (AE)
AE è un termine per indicare sistemi di crittografia che proteggono sia la confidenzialità che l'autenticità di una comunicazione.

Si può ottenere un sistema di questo tipo in varie maniere:
- digest + encrypted version 
	  h = H (M ) 
	  E(K , (M ||h ))
- authentication + encryption 
	  T = MAC(K1 , M ) 
	  E(K2 , [M || T ]) 
- encryption + authentication
	 C = E(K2 , M ) 
	 T = MAC(K1 , C )
	 (C , T )
- encryption || authentication
	  C = E(K2 , M )
	  T = MAC(K1 , M )
	  (C , T )

Il problema è che ciascuno di questi approccia ha delle vulnerabilità.


## Counter with Chiper Block Chaining Message Authentication Code (CCM)
È una variante dell'approccio "cifra e poi fai il MAC" per la cifratura autenticata.

CCM si basa sull'utilizzo di AES, CTR mode e CMAC come algoritmo di cifratura.

La chiave simmetrica usata per la cifratura è la stessa usata per calcolare il MAC.

![[1715328300_grim.png]]
In questo schema:
- nonce serve per impedire i replay attacks,
- Ass. Data sono dati non cifrati associati al plaintext, come ad esempio l' header di un pacchetto, etc.

## Galois/Counter Mode (GCM)
È un algoritmo pensato per essere parallelizzabile così da fornire un alto throughput, ma con minor costo e latenza

![[1715328672_grim.png]]

GCM usa le due funzioni schematizzate nell'immagine sopra:
- GHASH, ossia una funzione di hash che usa una chiave
- GCTR, ossia una modalità CTR che prevede dei contatori per contare le operazioni

La funzione $GHASH_{H}(X)$ può essere vista come 
$$
GHASH_{H}(X) = (X_{1}\cdot H^m)\; XOR\; (X_{2}\cdot H^{m-1})\;XOR\; \dots\;XOR\; (X_{1}\cdot H^2)\; XOR\; (X_{m}\cdot H)
$$
dove:
- il simbolo $\cdot$ indica la moltiplicazione in $GF(2^{128})$
- H è la chiave di hash  

Se la stessa chiave di hash viene usata per l'autenticazione di messaggi multipli, allora $H^{2},H^{3}$ può essere precalcolata ona volta per l'utilizzo di ogni messaggio che deve essere autenticato.


$GCTR_{K}(ICB, X)$ prende in input un segreto K e una stringa di lunghezza arbitraria X e ritorna un testo cifrato Y di lunghezza len(X).


Lo schema generale di GCM è il seguente:
![[1715329372_grim.png]]

## Key Wrap (KW)
L'idea è quella di scambiare una chiave simmetrica in modo sicuro usando una chiave simmetrica che è già nota a entrambi le parti. Questa chiave già conosciuta prende il nome di **Key Encryption Key (KEK)**

La comodità di questo algoritmo è che se la segretezza della chiave pre-shared è mantenuta, le due parti possono modificare la chiave segreta utilizzata per la comunicazione rapidamente, scambiandosi la chiave attraverso key wrapping. 


Il nuovo modo di cifratura, chiamato Key Wrapping (KW), è stato creato per gestire chiavi di lunghezza superiore alla dimensione dei blocchi dell'algoritmo di crittografia, come AES. Poiché le chiavi sono cruciali per la sicurezza e vengono utilizzate ripetutamente, la loro protezione è prioritaria. KW offre una robusta crittografia in cui ogni bit dell'output dipende in modo significativo da ogni bit dell'input, a differenza di altre modalità. Tuttavia, KW ha un throughput inferiore rispetto ad altre modalità, ma è adatto per applicazioni di gestione delle chiavi. È utilizzato principalmente per piccole quantità di testo in chiaro anziché per grandi quantità come messaggi o file.

![[1715330937_grim.png]]

![[1715333316_grim.png]]

## Pseudorandom Number Generation Using Hash Functions and MACs
L'idea è di usare una hash function o MAC come PRNG.
![[1715333424_grim.png]]
![[1715333590_grim.png]]
Nello standard di sicurezza LAN wireless IEEE 802.11i, viene utilizzata una combinazione di seme e contatore come input dati, incrementando il contatore per ogni blocco di output. Questo metodo sembra offrire una sicurezza migliorata rispetto all'approccio SP 800-90, dove l'input dati per ciascun blocco di output dipende solo dall'output del blocco precedente di HMAC. Tuttavia, anche se un avversario può osservare l'output pseudocasuale, conoscendo sia l'input che l'output di HMAC, la presunta sicurezza di HMAC dovrebbe impedire di recuperare la chiave K e quindi di prevedere i futuri bit pseudocasuali. Al contrario, protocolli come TLS e WTLS utilizzano HMAC due volte per ogni blocco di output, senza fornire informazioni dirette sull'input. Questa doppia applicazione di HMAC aumenta il carico computazionale ma sembra eccessiva in termini di sicurezza.

