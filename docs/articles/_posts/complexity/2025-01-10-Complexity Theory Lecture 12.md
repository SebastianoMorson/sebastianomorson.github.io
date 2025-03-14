---
layout: post
title: Problemi NP-completi
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Dopo aver dimostrato la NP-completezza di due problemi molto interessanti ($CIRCUIT$-$VAL$ e $CIRCUIT$-$SAT$), ora vedremo un altra serie di NP-completezze che in qualche modo derivano proprio dal teorema di Cook-Levin.

Nell'ordine, i problemi di cui disquisiremo saranno:
- $Satisfiability Problem$ (per gli amici SAT)
- $Indipendent Set Problem$ 
- $Clique Problem$
- $Integer Programming (IP)$
- $KnapSack$

Dopo aver affrontato questi problemi passeremo a vedere i cosiddetti problemi $coNP$-$completi$ come $Validity$ e in seguito i problemi $N\mathbb{L}$ come $Reachability$ e $2$-$SAT$ 

## 3-SAT
Il problema SAT è il problema di stabilire la soddisfacibilità di una formula logica in forma normale congiuntiva. In particolare il problema 3-SAT corrisponde allo stabilire la soddisfacibilità di una formula in CNF dove ciascuna clausola è composta da al più 3 termini.

Una formula in forma normale congiuntiva è una formula costruita come 

$$
\varphi  = (x_{1} \lor x_{2}\lor x_{3}) \land(x_{4} \lor x_{5} \lor x_{6})\land \dots
$$

ossia da clausole in congiunzione tra loro.

#### Dimostrazione di appartenenza a NP
Per dimostrare che 3-SAT appartiene a NP si può agire per certificato mostrando come, data una formula $\varphi$ e un suo assegnamento $(x_{1},x_{2},\dots,x_{n})$, sia possibile verificare se l'assegnamento porta allo stato yes in tempo polinomiale. 

#### Dimostrazione di completezza $SAT \preceq 3-SAT$

È da notare che a partire da una formula logica costruita come 

$$
\varphi = x_{1} \lor x_{2} \lor x_{3}\lor x_{4}
$$  

è sempre possibile costruire una formula logica equivalente, ma in CNF con clausole a 3 variabili, tramite riscritture del tipo 


$$
\begin{aligned}
\varphi &= x_{1} \lor x_{2} \lor x_{3}\lor x_{4}\\
z &= x_{1}\lor x_{2} \\
\varphi &= (z \lor x_{3}\lor x_{4}) \land (z \lor x_{1} \lor x_{2})

\end{aligned}
$$

Perciò possiamo concludere che esiste una relazione R polinomialmente bilanciata e decidibile tale che per ogni x in SAT genera una y in 3-SAT. 
3-SAT è quindi NP-completo.

## INDIPENDENT SET
Il problema INDIPENDENT-SET è il problema di stabilire se dato, un grafo G = (V,E) e un intero k, nel grafo esiste un insieme di nodi indipendente di cardinalità k. 
Per insieme indipendente intendiamo un insieme $I$ tale che 

$$
G=(V,E), I\subseteq V, \forall u,v\in I, (u,v) \not\in E
$$

#### Dimostrazione appartenenza a NP
Per dimostrare l'appartenenza è sufficiente portare un certificato $(G,k; I)$
#### Dimostrazione NP-completezza $3\text{-SAT} \preceq \text{IND-SET}$
Consideriamo una formula logica in CNF con clausole da 3 letterali. 

$$
\varphi = (l_{1.1} \lor l_{1,2} \lor l_{1,3}) \land\dots \land(l_{k,1} \lor l_{k,2} \lor l_{k,3})
$$

A questo punto costruiamo un grafo in cui ciascun letterale corrisponde a un nodo:
- connettiamo tutti i nodi che si trovano all'interno di una stessa clausola. 
- connettiamo tra loro i nodi corrispondenti a letterali opposti (ad esempio $x$ e $\lnot x$)

Fatto ciò consideriamo positivi soltanto un letterale per ciascuna clausola (e quindi un nodo per ciascun triangolo). Se abbiamo $m$ clausole consideriamo $m$ letterali positivi e gli altri negativi, non ché $k=m$.

Il grafo ottenuto presenta un insieme indipendente se e solo se la formula $\varphi$ è soddisfacibile. 

Per ogni problema in 3-SAT possiamo costruire un corrispettivo problema in IND-SET usando un algoritmo che opera in tempo polinomiale. Abbiamo dimostrato la completezza. 
## CLIQUE
Il problema CLIQUE è stabilire se, dato un grafo $G = (V,E)$, esiste un insieme di nodi di cardinalità $k$ che sia fortemente connesso.
Perciò 
$$
G = (V,E), I\subseteq V, \forall u,v\in I, (u,v)\in E
$$

#### Dimostrazione di appartenenza a NP
Basta fornire un certificato.

#### Dimostrazione di NP-completezza $IND-SET \preceq CLIQUE$
In questo caso è sufficiente ragionare al contrario rispetto al problema di IND-SET. 
Dato un grafo di IND-SET costruiamo il suo complementare e stabiliamo 
Essendo IND-SET il problema complementare di CLIQUE, se esiste una IND-SET esiste anche CLIQUE. 
## INTEGER PROGRAMMING
In questo caso l'appartenenza a NP è dimostrata attraverso un certificato. La completezza si ottiene riducendo 3-SAT a INT-PROG. Si considera una qualsiasi formula in 3-SAT e si costruisce un integer program come un sistema di disequazioni dove i vincoli sulle variabili sono formalizzate come (ad esempio):

$$
\begin{aligned}
x_{p}+x_{\lnot p} = 1 \\
x_{l_{i,1}} +x_{l_{i,2}} +x_{l_{i,3}} \geq 1 \\
0\leq l_{i,j} \leq 1


\end{aligned}
$$


