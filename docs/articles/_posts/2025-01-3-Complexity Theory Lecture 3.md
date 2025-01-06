---
layout: post
title: Complexity Theory - Lecture 3
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->

Se credevate esistessero solo le macchine di Turing deterministiche, ebbene surprise! Esistono anche quelle non deterministiche.

Quello che cambia non sono altro che le transizioni. Se prima per uno stato potevamo avere una singola transizione attivata dal simbolo "a" (è un esempio), ora ne possiamo avere più di una che si attiva sul medesimo simbolo e il medesimo stato. 

Oltretutto la cosa figa è che se abbiamo più scelte, il non determinismo ci permette di eseguire contemporaneamente tutti i percorsi (perchè potenzialmente al primo colpo potrei individuare proprio il path che ci va meglio).



## Simulazione di una MdT non deterministica su una MdT deterministica
$$
NTIME(f(n)) \subseteq \bigcup_{c \in \mathbb{N}} TIME(c^{f(n)})
$$
**Dimostrazione**

Bisogna ricordare una cosa fondamentale: in una MdT non deterministica le transizioni possono essere tante. Significa che in $\Delta$ avremo più transizioni del tipo
$$
\begin{aligned}
\delta(q_0​,1)={(q_{accept}​,1,R),(q_{0}​,1,R)}
\end{aligned}
$$

