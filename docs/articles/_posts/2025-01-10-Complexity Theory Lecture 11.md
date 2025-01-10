---
layout: post
title: Appartenenza a NP per certificato
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Prima di affrontare le altre dimostrazioni di NP-completezza voglio spiegare come funziona l'appartenenza a NP per certificato.

Iniziamo però con il dare un paio di definizioni:

### Relazione decidibile polinomialmente
Una relazione si dice polinomialmente decidibile se calcolare R(x) è computabile da una macchina di Turing usando tempo polinomiale. 

### Relazione bilanciata polinomialmente
Una relazione si dice bilanciata polinomialmente se $\forall(x,y)\in R$ si ha che $\exists k$ tale che $\mid y \mid \le \mid x\mid^k$. 
Ossia, è possibile verificare che $R(x) =y$ in tempo polinomiale. Una relazione non bilanciata polinomialmente non assicura la possibilità di verificare tutte le trasformazioni della relazione in tempo polinomiale.

Supponiamo che $R(x, y)$ sia una relazione tale che $\mid y\mid \leq p(\mid x \mid)$ (quindi è bilanciata polinomialmente), ma verificare se $(x, y) \in R$ richiede la risoluzione di un problema NP-completo. In tal caso, R non è decidibile in tempo polinomiale, nonostante sia bilanciata polinomialmente.

# APPARTENENZA A NP PER CERTIFICATO
Un linguaggio $\mathcal{L} \in NP \iff \exists R$ decidibile polinomialmente e bilanciata polinomialmente tale che 

$$
\mathcal{L} = \{x \mid \exists (x,y) \in R\}
$$
Quindi fondamentalmente la definizione dice che un linguaggio $\mathcal{L}$ appartiene a NP se e solo se per ogni elemento del linguaggio esiste una relazione che prevede una contro-immagine $y$ per ogni immagine $x$ in $\mathcal{L}$ e tale per cui ogni trasformazione è verificabile in tempo polinomiale (decidibilità polinomiale) e la lunghezza della contro immagine sia sempre inferiore a un polinomio sulla lunghezza di $x$ (bilanciamento polinomiale).

## Significato
Il significato intrinseco è che non è necessario dimostrare l'appartenenza a NP per poter dire che un linguaggio appartiene a NP. È sufficiente che si trovi una sua istanza che sia verificabile in tempo polinomiale.

### Dimostrazione $\text{se } \mathcal{L} \in NP \text{ allora }\exists R \text{ polinomialmente decidibile e bilanciata}$ 
Se $\mathcal{L}$ appartiene a NP significa he esiste una macchina $\mathcal{N}$ con I/O che decide $\mathcal{L}$ in tempo polinomiale.
Consideriamo una relazione $R(x, c_{1},c_{2}, \dots, c_{n}) \mid x\in\mathcal{L} \land \mathcal{N}(c_{1},c_{2},\dots,c_{n})=yes$ .
Eseguiamo quindi la macchina $\mathcal{N}$ sull'input $x$ e a ogni passo non deterministico come scelta non deterministica considero uno degli elementi della tupla $(c_{1},c_{2},\dots,c_{n})$.
$(c_{1}, c_{2}, \dots, c_{n})$ sarà quindi il nostro certificato per l'input $x$. Sappiamo che $(c_{1}, c_{2}, \dots, c_{n}) \leq \mid x \mid^k$ perciò 
la relazione è polinomialmente decidibile e bilanciata. 

### Dimostrazione $\text{se }\exists R \text{ allora } \mathcal{L} \in NP$
Consideriamo il linguaggio composto dagli elementi per cui esiste una controimmagine in R.

$$
\mathcal{L} = \{x \mid \exists y.(x,y)\in R\}
$$

A questo punto è possibile quindi creare una macchina $\mathcal{N}$ che decide il linguaggio $\mathcal{L}$ semplicemente generando non deterministicamente ogni possibile controimmagine $y$ di x con $\mid y \mid \leq \mid x \mid ^k$ per poi verificare che la coppia $(x,y)\in R$. Queste sono tutte operazioni svolte in tempo polinomiale, quindi $\mathcal{L} \in NP$.

$\square$

