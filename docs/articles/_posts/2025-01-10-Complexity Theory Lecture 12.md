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


## CLIQUE


## INTEGER PROGRAMMING


## KNAPSACK

