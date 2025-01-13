---
layout: post
title: Relazione tra Classi di Complessità 2 - Hierarchy Theorem
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Lo Hierarchy Theorem è uno di quei teoremi che ti cambiano la vita. Uno di quelli che guardi e ti dici "ecco la prova che non siamo soli nell'universo".
Il teorema dice che:

---
## -- HIERARCHY THEOREM --
**Data una funzione propria $f(n)\geq n$ allora la classe di complessità $TIME(f(n))$ è contenuta strettamente in quella $TIME(f(2n+1)^3)$.**

$$
TIME(f(n)) \subsetneq TIME(f(2n+1)^3)
$$

---

Allora...
piccolo recap delle puntate precedenti:
una funzione propria abbiamo detto che è una funzione monotona non decrescente tale per cui esiste una MdT $\mathcal{M}_{f}$ che calcola proprio $f$ e che $\forall x\in\Sigma^*$ termina scrivendo sul nastro di output il risultato di $f(x)$ in unario: quindi in poche parole una sfilza di $\sqcap^{f(n)}$.
Ci serviva questa definizione per formalizzare il tipo di funzioni con caratteristiche comuni.

Ebbene avevamo anche visto che da una funzione propria riuscivamo anche a ricondurci a una macchina precisa (che altro non era che una macchina con I/O per la quale esistono due funzioni $f$ e $g$ t.c. $\forall x$ viene usato tempo $f(n)$ e $g(n)$.


### DIMOSTRAZIONE DEL TEOREMA
La dimostrazione del teorema verte sulla dimostrazione di due lemma fondamentali
1. $H_{f}\in TIME(f(n)^3)$
2. $H_{f} \not\in TIME\left( f\left( \lfloor\frac{n}{2}\rfloor \right) \right)$ 

Dove $H_{f}$ non è altro che una riformulazione di un teorema famosissimo, che è l'Halting Problem.

##### DEFINIZIONE HALTING PROBLEM
L'halting problem definisce una insieme 

$$
H = \{M;x \mid M(x)\downarrow\}
$$

ossia l'insieme delle coppie $(M;x)$ per cui la computazione di $M(x)$ termina. 

$\square$


Ebbene $H_f$ è definito come 

$$
H_{f} = \{M;x \mid M(x) \text{ termina in } f(x)+5n+4 \text{ steps}\}
$$

Perciò invece di dire semplicemente che termina, diciamo anche in quanti passi vogliamo che lo faccia.
Se vi state chiedendo "ma perchè consideriamo quel $5n+4$", la risposta è che è il tempo che serve per svolgere una certa operazione nel lemma 2 che vedremo. 

#### Lemma $H_{f} \in TIME(f(n)^3)$ con $f$ proper

**Dimostrazione**

Partiamo con il presupposto che quando scrivo $n$ indico la lunghezza di $x$, quindi $n=\mid x \mid$
Consideriamo una macchina universale $\mathcal{U}$ costruita come segue:
1. riceve in input una k-MdT $\mathcal{M}$ e una stringa $x$
2. sul primo nastro ricopia la codifica binaria della macchina $\mathcal{M}$
3. sul secondo nastro scrive $f(n)+5n+4$ in unario (quindi $\sqcap^{f(n)+5n+4}$) 
4. usa il terzo nastro per tener traccia delle configurazioni di $\mathcal{M}$ 

La macchina di Turing M richiede $O(f(n))$ steps di computazione per essere simulata. 
Analizziamo quindi quanti steps sono richiesti a $\mathcal{U}$ per simulare $\mathcal{M}(x)$:
1. Durante tutta la computazione eseguirò sicuramente una scansione del primo e del secondo nastro, perchè devo leggere l'input e scrivere in unario i simboli quasi-blank. Queste operazioni sono costanti
2. All'inizio bisogna leggere la configurazione di $\mathcal{M}$, che è una k-MdT quindi avrà potenzialmente $O(f(n))$ nastri lunghi $O(f(n))$. Quindi per leggerli tutti avrò bisogno di $O(f(n)^2)$ steps
3. Lette le configurazioni dovrò quindi decidere quali modifiche apportare, ma questo non è un problema, perchè comunque sarà solo uno il nastro da modificare
4. terminato lo step cancello un simbolo quasi-blank e ricomincio.

Quindi per ogni step impiego sicuramente $O(f(n)^2)$ steps.

La simulazione richiede dunque $O(f(n)^2*f(n))=O(f(n)^3)$ steps.

$\square$

Io mi sono inceppato più e più volte su un certo dettaglio, ossia sul **perchè ho $f(n)$ nastri**. 
La spiegazione si basa sul fatto che una $k$-Turing machine ha un numero <u>indefinito</u> di nastri ($k$ per l'appunto)
###### 1. **Ragionevolezza del numero di nastri:**

Se k fosse estremamente grande, molto maggiore di $f(n)$ (ad esempio $k = f(n)^2$, i nastri stessi diventerebbero difficili da gestire. Ad esempio:

- Ogni nastro potrebbe avere una lunghezza massima di $O(f(n))$
- Avere più nastri di quelli "necessari" non dà vantaggi pratici, e aumenta inutilmente la complessità costruttiva della macchina.

---

###### 2. **Collegamento tra k e $O(f(n))$**

L'assunzione $k \leq O(f(n))$ deriva dall'idea che, nel **peggiore dei casi**, potresti avere un nastro per ogni unità di spazio occupato dalla computazione. Ad esempio:

- Se una macchina usa $f(n)$ celle in totale, potresti progettare $f(n)$ nastri, ciascuno con una sola cella occupata. Questo caso è ovviamente teorico e irragionevole in pratica, ma definisce un limite massimo al numero di nastri.

Quindi, assumere $k \leq O(f(n))$ è una convenzione utile per evitare casi patologici.


#### Lemma 2 $H_{f} \not \in TIME\left( f(\lfloor \frac{n}{2}\rfloor \right))$

**Dimostrazione**

La dimostrazione in questo caso avviene per diagonalizzazione e assurdo.

La macchina D è così costruita:
1. in input prende la codifica della macchina $\mathcal{M}$
2. copia l'input $(\mathcal{M};\mathcal{M})$ come input della macchina universale $\mathcal{U}$

Per copiare l'input della macchina $\mathcal{D}$ è necessario:
- leggere l'input (tempo = n)
- leggere il simbolo $\sqcup$ per capire dove finisce M (tempo 1)
- scrivere n sull'input tape della macchina $\mathcal{U}$ 
- scrivere il simbolo ; sull'input tape 

Consideriamo quindi la macchina M costruita come nella dimostrazione del primo lemma, e una seconda macchina D.

$$
\mathcal{\mathcal{D}}(\mathcal{M}) = 
\begin{cases}
yes \iff \mathcal{M}_{H_{f}}(\mathcal{M},\mathcal{M}) = no \iff  \begin{cases} 
\mathcal{M}(\mathcal{M}) = no \\
\mathcal{M}(\mathcal{M})\uparrow \\
\mathcal{M}(\mathcal{M}) \text{ terminates after } f(n)+5n+4 \text{ steps}
\end{cases} \\ \\
no \iff \mathcal{M}_{H_{f}}(\mathcal{M},\mathcal{M}) = yes \iff\mathcal{M}(\mathcal{M}) = yes 
\end{cases}
$$

Consideriamo quindi alcune casistiche

$$
\mathcal{\mathcal{D}}(\mathcal{D}) = 
\begin{cases}
yes \iff \mathcal{M}_{H_{f}}(\mathcal{D},\mathcal{D}) = no \iff  \begin{cases} 
\mathcal{D}(\mathcal{D}) = no  \text{ ASSURDO}\\
\mathcal{D}(\mathcal{D})\uparrow \text{ ASSURDO perchè allora come facciamo a dire che =yes?}\\
\mathcal{D}(\mathcal{D})\downarrow \text{ after } f(n)+5n+4 \text{ steps, IMPOSSIBILE per costruzione}
\end{cases} \\ \\
no \iff \mathcal{M}_{H_{f}}(\mathcal{D},\mathcal{D}) = yes \iff\mathcal{D}(\mathcal{D}) = yes \text{ ASSURDO} 
\end{cases}
$$

$\square$

Ma quindi considerando $m=2n+1$ otteniamo che

$$
\begin{aligned}
TIME( f( \lfloor\frac{m}{2}\rfloor ) ) &\subsetneq TIME(f(m)^3) \\
TIME( f( \lfloor\frac{2n+1}{2}\rfloor ) ) &\subsetneq TIME(f(2n+1)^3) \\
TIME( f(n) ) &\subsetneq TIME(f(2n+1)^3)
\end{aligned}

$$

$$

\square \text{  END OF HIERARCHY THEOREM's PROOF  } \square
$$


## Conseguenze
Questo teorema è importante perchè dice una cosa molti importante: esistono classi di problemi che non possono collassare. 
Facciamo un esempio:

$$
\begin{aligned}
P &= \bigcup_{k \in \mathbb{N}}TIME(n^k) \\ \\
EXP &= \bigcup_{k \in \mathbb{N}} TIME(2^{n^k})
\end{aligned}
$$

Per cui $P \subseteq TIME(2^n)$ perchè ogni problema in $n^k$ può essere risolto in $2^{n^k}$

Ma sappiamo che per lo Hierarchy theorem la funzione esponenziale è una funzione propria, quindi 

$$

\begin{aligned}
TIME(f(n)) &\subsetneq TIME(f(2n+1)^3) \\
TIME(2^n) &\subsetneq TIME(2^{(n+1^3}))
\end{aligned}
$$

e quindi

$$
\begin{aligned}
P \subseteq TIME(2^n) &\subsetneq TIME(2^{(n+1^3})) \subseteq EXP \\
P &\subsetneq EXP
\end{aligned}
$$

Perciò EXP contiene tutti i problemi di P, ma anche qualcuno in più.
EXP contiene però problemi risolvibili in P, ma anche problemi che non sono risolvibili in P. 
Le due classi quindi non possono collassare.
