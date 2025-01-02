---
layout: post
title: Information Theory - Lecture 3
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
## Theorem Direct
Se $l_{1},l_{2},\dots,l_{k}$ sono tali che $\sum_{i=1}^k D^{-l_{i}} \le 1$, allora esiste un prefix code dove le lunghezze degli encoding sono $l_{1},l_{2},\dots,l_{k}$

***Dimostrazione***
La dimostrazione si basa sullo stesso concetto della dimostrazione del [[2024-12-26-Information Theory Lecture 2#Proof for prefix codes|teorema di Kraft MacMillan per i prefix code ]]
![[Pasted image 20240307095131.png]]
## Remark
Ricordiamo la definizione di [[2024-12-26-Information Theory Lecture 2#Uniquely Decodable with delay 1|Uniquely Decodable]].
I prefix code ***hanno lo stesso potere dei U.D. codes***.
Possiamo raggiungere gli stessi livelli di compressione e non abbiamo delay nel decoding usando i prefix codes.


## Definition of EL
Assumiamo 

$$ 
\begin{aligned}
&A\text{ primary alphabet}\\
&B\text{ secondary alphabet} \\
&\varphi:A^* \to B^*
\end{aligned}
$$

Allora

$$
\begin{aligned}
EL(\varphi) &= \sum_{i=1}^k p_{i}\cdot l(\varphi(a_{i})) \\
&= \sum_{i=1}^k p_{i \cdot l_{i}}
\end{aligned}
$$
Ossia, $EL(\varphi)$ rappresenta la lunghezza media delle stringhe del codice
# Code Rate

## Compression Rate

### 1° Shannon Theorem (Source Code Theorem)
Se $\varphi$ è Uniquely Decodable allora 

$$
EL(\varphi) \ge H_{D}(P)
$$

dove $H_D(P)$ è l'entropia di $P$ usando $log_D$ 

L'idea è che non si può esprimere più informazione dell'entropia che abbiamo.
Perciò il teorema di Shannon dimostra che l'entropia $H_D(P)$ è una misura teorica del numero di bit medio necessari a rappresentare l'informazione di una sorgente attraverso una codifica ottimale.

***Dimostrazione***

*Tesi*: $EL(\varphi) - H_{D}(P) \ge 0$

$$
\begin{aligned}
EL(\varphi) - H_{D}(P) &= \sum_{i=1}^k p_{i} \cdot l_{i} + \sum_{i=1}^k p_{i} \log_{D} p_{i} \\
&= \sum_{i=1}^k p_{i} (l_{i}+\log_{D}p_{i}) \\
&= \sum_{i=1}^k p_{i} \left(\log_{D}D^{l_{i}} + \log_{D} p_{i}\right) \\
&= \frac{1}{\ln D } \cdot \sum_{i=1}^k p_{i} \ln(D^{l_{i}}\cdot p_{i})\\
&= -\frac{1}{\ln D} \cdot \sum_{i=1}^k p_{i} \ln\left( \frac{1}{D^{l_{i}} \cdot p_{i}} \right) \\
&\ge -\frac{1}{\ln D} \sum_{i=1}^k p_{i} \ln\left( \frac{1}{D^{l_{i}} \cdot p_{i}} - 1 \right) \\
&\ge -\frac{1}{\ln D} \left( \sum_{i=1}^k p_{i} \cdot \frac{1}{D^{l_{i}}\cdot p_{i}} - \sum_{i=1}^k p_{i} \cdot 1 \right) \\
&\ge -\frac{1}{\ln D}\left( \sum_{i=1}^k D^{-l_{i}} -1 \right) \\ \\
&\text{Ma per il teorema di Kraft MacMillen}\\
&\text{quello che sta dentro le parentesi è } \le 1\\ &\text{Perciò:}
\\ \\
&\ge 0

\end{aligned}
$$
