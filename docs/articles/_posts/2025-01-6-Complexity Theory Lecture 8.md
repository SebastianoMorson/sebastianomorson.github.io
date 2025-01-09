---
layout: post
title: Relazione tra Classi di Complessità 5 - Savitch Theorem
excerpt_separator: <!--more-->
truncated_preview: true
tags:
  - miscellaneous
categories: article
---
<!--more-->
Premetto che la dimostrazione del teorema di Savitch non è complicata.
Partiamo con l'enunciato.

## -- SAVITCH THEOREM --
Il teorema di Savitch afferma che per 

$$
Reachability \in SPACE((\log n)^2)
$$

**DIMOSTRAZIONE**

Per cominciare ricordiamo qual è il problema $Reachability$.
Ebbene Reachability è il problema di stabilire se dato una grafo $G=(V,E)$ e due sue nodi $u$ e $v$, esiste un path che congiunge i due nodi.

Dato un grafo $G = (V,E)$ e due sue nodi in $V$ cerchiamo quindi di costruire una macchina di Turing deterministica che riconosce il linguaggio.

Questa macchina sarà costruita come segue 

$$
\begin{aligned}
\text{ Input tape} &\mid \triangleright \mid V \mid E \mid u \mid v \mid\ \sqcup  \mid \dots \\
\text{ Working tape 1 } &\mid \triangleright \mid \sqcup \mid \sqcup \mid \sqcup \mid \sqcup \mid\ \sqcup  \mid \dots \\
\text{ Working tape 2 } &\mid \triangleright \mid \sqcup \mid \sqcup \mid \sqcup \mid \sqcup \mid\ \sqcup  \mid \dots \\
\dots \\
\text{ Working tape k } &\mid \triangleright \mid \sqcup \mid \sqcup \mid \sqcup \mid \sqcup \mid\ \sqcup  \mid \dots \\
\text{ Output tape } &\mid \triangleright \mid \sqcup \mid \sqcup \mid \sqcup \mid \sqcup \mid\ \sqcup  \mid \dots 
\end{aligned}

$$
Ragioniamo sul problema:
dato un grafo e due sue nodi u e v possiamo stabilire se esiste un path tra u e v semplicemente agendo ricorsivamente. Ad esempio immaginiamo di avere un grafo come 


Consideriamo un predicato 

$$
P(u,v,i) = \text{"esiste un path tra i nodi u e v di al massimo lunghezza } 2^i \text{ "}
$$
i casi sono quindi 

$$
\begin{aligned}
P(u,v,0) &\to \text{ esiste un percorso tra u e v di lunghezza massima } 2^1 \\
P(u,v, 1) &\to \text{u è raggiungibile da v in } 2^1 \text{ passi al massimo} \\
P(u,v, i+1) &\to \text{u è raggiungibile da v in }2^{i+1} \text{ passi al massimo}\\
&\to \exists z.P(u,z,i) \land P(z,v,i)
\end{aligned}
$$
Un esempio di predicati per il grafo sopra potrebbe essere 
$$
\begin{aligned}
(A, B, 0) &\to true / false\\
(A, B, 3) &\to (A,C,2) \land (B,C,2) &\to  (A,D,1) \land (C,D,1) \\
&&\to(B,D,1) \land (C,D,1) \\

\end{aligned}
$$

Ovviamente il numero massimo di passi da percorrere è $2^i \leq \mid V \mid$ perchè il caso limite è che i nodi abbiano al più una connessione, quindi siano disposti in fila. In questo caso, perciò $i \leq \log \mid V \mid$


Perciò possiamo rappresentare ogni passo di ricorsione da una tripla costruita come 

$$
(u, v, i)
$$
dove:
- $u, v$ sono due nodi che possono essere rappresentati da degli indici di dimensione $log(\mid V \mid)$
- $i$ è un indice che abbiamo detto essere limitato da $\log \mid V \mid$ quindi anch'esso rappresentabile in spazio logaritmico rispetto alla dimensione di V 

Su ciascun nastro della macchina di input posso inserire le possibili combinazioni di passi ricorsivi e verificarne la veridicità. Sappiamo che i passi ricorsivi sono $log(\mid V \mid)$ quindi su un singolo nastro occuperemo $O(log \mid V \mid \cdot 3\log \mid V \mid) = O((\log \mid V \mid)^2)$ spazio.

Quindi
$$
Reachability \in SPACE((\log n)^2)
$$

$\square$

## Considerazioni
Il teorema di Savitch permette di raggiungere un nuovo corollario

### Corollario: se $f(x) \ge \log n$ allora $NSPACE(f(n)) \subseteq SPACE(f(n)^2)$

**DIMOSTRAZIONE**

### Corollario: se $f(x) \ge \log n$ allora $NPSPACE = PSPACE$

### Corollario: se $f(x) \ge \log n$ allora $N\mathbb{L}\subseteq SPACE((\log n)^2)$


### Corollario: se $f(x) \ge \log n$ e $f$ è propria allora $coNSPACE(f(n)) = NSPACE(f(n))$

**DIMOSTRAZIONE**



