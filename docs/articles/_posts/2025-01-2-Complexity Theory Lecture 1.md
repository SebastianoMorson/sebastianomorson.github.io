---
layout: post
title: Turing Machines 1 - Uniform vs Logarithmic cost criterium
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Nell'information theory abbiamo studiato quanta informazione è possibile rappresentare e trasmettere in modo efficiente attraverso un encoding, analizzando la relazione tra la quantità di informazione (misurata tramite l'entropia) e la lunghezza media dell'encoding.  

Successivamente, abbiamo introdotto il concetto di complessità di Kolmogorov, che misura la lunghezza del programma più corto capace di generare una certa stringa, consentendoci di quantificare quanta "casualità" contiene una stringa e quanto essa sia comprimibile.  

Ora, con la teoria della complessità computazionale, il nostro obiettivo si sposta: non studiamo quanto sia difficile generare una stringa, ma quanto sia difficile risolvere determinati problemi. Attenzione: non parliamo di misurare la complessità di un algoritmo specifico che risolve un problema, ma di analizzare quanto, **indipendentemente dall'algoritmo scelto**, sia intrinsecamente difficile risolvere il problema, considerando risorse come tempo e spazio.

Esistono 3 tipi di problemi:
- di **decisione**: posso raggiungere qualsiasi nodo a partire da un nodo qualsiasi?
- di **funzione**: come posso raggiungere un qualsiasi nodo a partire da un nodo qualsiasi?
- di **ottimizzazione**: come posso raggiungere un qualsiasi nodo a partire da un nodo qualsiasi eseguendo il numero minimo di attraversamenti?

Noi ci concentreremo sui problemi decisionali.

### Turing Machine 
Vogliamo ricordare che una macchina di Turing è definita da una tupla $<K,\Sigma,\delta, s>$ dove:
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

k-tape $\to \delta: K \times \Sigma^K \to (K \cup \{y,n,h\}) \times \Sigma^K \times \{\leftarrow,\rightarrow,-\}^k$


Una generica configurazione è data dalla tripla $(q,w,u)$.
Nello stato iniziale avremo una configurazione 

$$
(1, \triangleright, x)
$$

mentre uno stato finale è descritto dalla configurazione 

$$
(H,w,u)
$$

con $H=\{yes,no,halt\}$

Un passo di computazione tra due configurazioni è descritto come

$$
(q,w,u)  \rightarrow^\delta (q',w,u')
$$

Se dopo $t$ passi di computazione una MdT passa da uno stato iniziale a uno finale, diciamo che la MdT ha **complessità di tempo** $t$.


### Criterio di costo uniforme e logaritmico
È importante osservare che nella macchina di Turing il tempo di computazione ( la sua complessità temporale) è data dal numero di passi svolti dalla macchina. Non ci interessa che l'input sia grande 10, 100 o 1 milione, ogni passo vale $\Theta$(1) ***(criterio di costo uniforme)***.

Ma ci sono modelli di calcolo che ci mostrano come non sia sempre adatto usare un **criterio di costo uniforme**. 

Partiamo con un piccolo pezzo di storia:
##### Tesi di Church estesa
La Tesi di Church-Turing Estesa afferma che tutti i modelli **ragionevoli** di computazione possono simulare l'un l'altro in tempo correlato polinomialmente. 


Questo implica che, se un problema può essere risolto in tempo $O(f(n))$ da un modello di computazione, allora un altro modello ragionevole risolverà lo stesso problema in tempo al massimo $O(p(f(n)))$, dove $p(f(n)$ è una funzione polinomiale.  
Ad esempio, se ho un programma $P$ scritto in Python che impiega $O(f(n))$ tempo per completarsi, lo stesso programma tradotto in un altro linguaggio e sugli stessi input impiegherà un tempo diverso, ma correlato polinomialmente a quello del primo programma. Questo presuppone che entrambi i linguaggi siano implementati in modo efficiente e che non vi siano overhead artificiale non correlato alla dimensione dell'input.


Il problema è che certi modelli potrebbero non essere ragionevoli. Ad esempio il modello URM (Unlimited Register Machine) con prodotto non è ragionevole. Ad esempio il calcolo di $x^{2^x}$ non è lineare rispetto alla dimensione di x, se lo fosse vorrebbe dire che potremmo calcolare un'operazione esponenziale in modo lineare! 
Essendo l'operazione di prodotto un'operazione che dipende dal numero di cifre dell'input, per misurare la complessità in maniera non ambigua usiamo un ***criterio di costo logaritmico.***

##### Definizione 
- ***Criterio di costo uniforme:*** usato quando le operazioni non dipendono dal numero di cifre dell'input. Le operazioni non fanno crescere significativamente l'input e l'output. La complessità è data dal numero di operazioni eseguite
- ***Criterio di costo logaritmico:*** usato quando le operazioni fanno crescere l'input o l'output in modo più o meno veloce proporzionalmente al numero di cifre coinvolte




