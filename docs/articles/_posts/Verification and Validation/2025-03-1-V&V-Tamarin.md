---
layout: post
title: Tamarin
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - verification-techniques
categories: article
---
<!--more-->

# Sintassi

## Sorts
Sono i tipi di termini che si possono avere.
Esiste un tipo principale che è il tipo `msg`, il quale può essere di tipo
- `pub` : usato per indicare termini pubblici (ad esempio identificatori noti a tutti)
- `fresh` : usato per indicare termini generati sempre nuovi (ad esempio i nonces)

## Signatures
Fondamentalmente rappresentano la definizione astratta di una certa funzione.

Le principali signature sono:

| Simbolo	| Arità	| Significato |
| :--: | :--: | :--:|
| enc/2	| 2 | Cifratura: enc(m, k) significa che il messaggio m è cifrato con la chiave k. |
| dec/2	| 2	| Decifratura: dec(c, k) restituisce il messaggio originale m se c = enc(m, k). |
| h/1	| 1	| Funzione hash: h(m) rappresenta l’hash di m. |
| <_, _> |	| 2	Coppia: <m1, m2> è un messaggio formato da due parti. |
| fst/1	| 1	| Prima componente: fst(<m1, m2>) = m1. |
| snd/1	| 1	| Seconda componente: snd(<m1, m2>) = m2. | 
| _ ∧ _	| 2	| Operatore logico AND. | 
| _^{-1}| 1 | Inversione (ad esempio chiave privata corrispondente a una chiave pubblica). | 
| _ * _	| 2 | Operazione bilineare (utile per Diffie-Hellman e altri schemi di crittografia). | 
| 1 | 0 | Identità moltiplicativa nell'operazione bilineare. |

A questo punto ci si potrebbe chiedere: okay, ma queste sono le signature delle funzioni, ma come faccio a imporre che `Enc(_, k)` utilizzi una chiave `k` di tipo `fresh`?

## Terms


## Facts

## State
insieme di fatti

## Transition
- state:


#### Linear Facts

#### Persistent Facts
Definiti con !