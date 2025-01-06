---
layout: post
title: Complexity Theory - Lecture 2
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->

## K-tape Turing Machine

## K-tape Turing Machine with Input/Output

## Complessità temporale

## Complessità spaziale

## Differenza tra Complessità temporale su k-tape e 1-tape 
Se ho una k-MdT $\mathcal{M}$ che accetta un linguaggio $\mathcal{L}$ in tempo $TIME(f(n))$ allora esiste una 1-MdT $\mathcal{M}'$ che accetta il medesimo linguaggio in tempo $TIME(f(n)^2)$


## Classi di complessità temporale


## Classi di complessità spaziale



## Speed-up theorem sul tempo

$$
TIME(f(n)) = TIME(\epsilon \cdot f(n)+n+2)
$$
La cosa bella dello speed-up theorem è che ci permette di evitare di avere mille classi di complessità diverse per ciascun problema. Essenzialmente infatti ci dice che fintanto che per misurare la complessità di tempo usiamo k-MdT,  le costanti moltiplicative sono ininfluenti, perchè in base a come costruiamo la macchina di Turing possiamo completamente annullarle. 


L'idea della dimostrazione è quella di considerare una MdT $\mathcal{{M}}$ che computa un certo problema e simularne l'esecuzione su una macchina k-tape $\mathcal{M}'$ che esegue $m$ simboli di $\mathcal{M}$ alla volta.

Un volta compresso 

## Speed-up theorem su spazio

