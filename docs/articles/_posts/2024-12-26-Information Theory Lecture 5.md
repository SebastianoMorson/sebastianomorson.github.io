---
layout: post
title: Information Theory - Lecture 5
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
# Kolmogorov Complexity 1965
Con la complessità di Kolmogorov si cerca di descrivere la quantità di complessità di un oggetto (stringa). Fin'ora abbiamo visto quanta informazione può trasmettere una stringa, ora vogliamo studiare la sua complessità.

## Ripasso sulle Turing Machines
$$
M = (K, \Sigma, \delta, 1) \\
$$
dove 
- $K$ è un insieme finito di stati
- $\Sigma$ è un alfabeto finito tale per cui
	1. $K\cap \Sigma = \emptyset$
	2. $\rhd, \sqcup \in \Sigma$ 
- $\delta: K \times \Sigma \to (K \cup \{\text{yes,no,halt}\}) \times \Sigma \times \{\leftarrow, \rightarrow, -\}$
	$\rhd$ cannot be changed
	$\rhd$ cannot go to its left
	$\delta(q,\rhd) = (q', \rhd, \rightarrow)$ 
- $s \in K$ starting state

1-tape
k-tape $\to \delta: K \times \Sigma^K \to (K \cup \{y,n,h\}) \times \Sigma^K \times \{\leftarrow,\rightarrow,-\}^k$

### Definizione M.d.T. Universale
Esiste una macchina di Turing $\mathcal{U}$ che, ricevuto un input $bin(M); bin(X)$ si comporta come $M$ su un input $x$. Questa macchina è chiamata macchina di Turing universale.

$$
\mathcal{U}(\mathcal{M};x) = \mathcal{M}(x)
$$

### Definizione Kolmogorov Complexity
La complessità di Kolmogorov associata a una stringa $x \in \Sigma^*$ è data dalla lunghezza minima della MdT $\mathcal{M}$ che calcola x. 

$$
K_{\mathcal{U}}(x) = min_{\mathcal{M}:\mathcal{U}(\mathcal{M})=x}\mid M \mid
$$

A partire da questa definizione possiamo concludere alcune interessanti proprietà:
- se $\mathcal{M}$ è calcolata senza avere informazioni a priori, allora la sua complessità di Kolmogorov sarà più alta di quella della medesima macchina, avendo una conoscenza $y$

$$
K_{\mathcal{U}}(x) \ge K_{\mathcal{U}}(x\mid y)
$$

	Se ho un programma che stampa $m$ volte la stringa "Hello world", ma non conosco $m$, sarà necessario scrivere $m$-volte "print(Hello world)". Ma se conosco $m$ posso scrivere un ciclo $for$ che sono sicuro termini dopo $m$ passi.
	Perciò invece di avere $m$ istruzioni, me ne bastano 2:
	for i in range(0,m):
		print("hello world")

- se ho due macchina di Turing universali $\mathcal{U}$ e $\mathcal{A}$ avremo sempre che 

$$ 
K_{\mathcal{U}}(x\mid y) \le K_{\mathcal{A}}(x \mid y) + c_{\mathcal{UA}}
$$

#### Limiti alla complessità di Kolmogorov sulle stringhe
Il problema della complessità di Kolmogorov sulle stringhe è che non è definibile in maniera precisa. In altre parole non è mai possibile dire con precisione che la complessità di Kolmogorov della stringa $x$ è "295", perchè per dimostrarlo dovrei provare che tutte le MdT di lunghezza <295 non producono in output la stringa $x$. Ma basterebbe solo che una di queste MdT non terminasse per rendere impossibile la dimostrazione e quindi impossibile stabilire con certezza che la Kolmogorov Complexity su $x$ sia esattamente "295".

Ma, c'è un ma.

Non potremo dire con certezza assoluta quale sia la complessità di Kolmogorov su $x$, però possiamo dire a spanne quanto potrebbe valere: invece di dire "vale 295" diremo "è un valore tra 250 e 330".

Il teorema sui limiti alla complessità di kolmogorov sulle stringhe afferma più precisamente che:

$$
\mid x \mid \leq K(x) \leq \mid x \mid + c
$$

**Dimostrazione** $K(x) \le \mid x \mid + c$

k

**Dimostrazione** $\mid x \mid \leq K(x)$

l
