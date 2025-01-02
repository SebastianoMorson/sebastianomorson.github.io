---
layout: post
title: Myhill-Nerode Theorem
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Per 3 anni sono riuscito a scampare la dimostrazione del teorema di Mayhill-Nerode, ma è arrivata l'ora di impararlo definitivamente.

Il teorema afferma che i seguenti enunciati sono equivalenti:
1. $L \subseteq \Sigma^*$ è regolare
2. $L$ è l'unione di classi di equivalenza di $\Sigma^*$ indotte da una relazione di invariante a destra di indice finito
3. $R_{L}$ è di indice finito

Parafrasando:
1. L è un linguaggio regolare
2. L è l'unione di sottoinsiemi di parole che appartengono alla stessa classe di equivalenza, ossia parole equivalenti tra loro rispetto a una relazione $R$. In particolare in questo caso la relazione $R$ è una relazione invariante a destra di indice finito, ossia una relazione del tipo
   
   $$
   xR_{L}​y⟺(∀z∈Σ^∗)(xz∈L⟺yz∈L).
   $$
   
   dove il numero di classi di equivalenza indotte da R è finito
3. il numero di classi di equivalenza indotte da $R_L$ è finito

Perciò in poche parole dobbiamo dimostrare che
1. 1 -> 2 : Se $$L\subseteq\Sigma^*$$ è regolare allora $L$ è l'unione di classi di equivalenza di $\Sigma^*$ indotte da una relazione di invariante a destra di indice finito
2. 2 -> 3: Se $L$ è l'unione di classi di equivalenza di $\Sigma^*$ indotte da una relazione di invariante a destra di indice finito allora $R_{L}$ è di indice finito
3. 3 -> 1: Se $R_{L}$ è di indice finito allora  $L \subseteq \Sigma^*$ è regolare

#### Dimostrazione (1)
Essendo L un linguaggio regolare, esiste un DFA $\cal{M}$ che lo accetta.
Per definizione di linguaggio riconosciuto da un automa:

$$
L = \bigcup_{q \in F} \;\{\;x\in\Sigma^* \;| \;\hat{\delta}(q_{0}, x) = q \;\}
$$
definiamo quindi la relazione ~$_{\cal{A}}$ su un DFA $\cal{A}$ come la relazione per cui se $x,y\in\cal{A}^*$, allora $x$ ~$\cal{_A} \;y$ se $\delta(q_0, x) = \delta(q_0,y)$

Ci accorgiamo quindi che L è l'unione delle classi di equivalenza di ~$\cal{_A}$ che contengono una parola $x$ per la quale $\delta(q_0,x) \in F$ 


#### Dimostrazione (2)

#### Dimostrazione (3)
