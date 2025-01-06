---
layout: post
title: Complexity Theory - Lecture 4
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
# Relazione tra Classi di Complessità

## Funzioni Proprie e Macchine Precise
Alcune domande a cui cercheremo di dare una risposta in questo articolo sono:
- Perchè abbiamo avuto bisogno di stabilire l'esistenza di funzioni proprie e di macchine precise?
- Perchè le funzioni proprie sono interessanti?
- Esistono funzioni non proprie e macchine non precise?
- Esistono macchine precise che non calcolano funzioni proprie?
- Esistono macchine non precise che calcolano funzioni proprie?

(SUGGERIMENTO, riscriverò la domanda nel momento in cui vi risponderò, quindi se vi interessa sapere la risposta alla domanda basta fare un bel CTRL+F e cercare la domanda che vi interessa. Avrei potuto mettere un link diretto, ma perchè rendere la vita più semplice a me e a voi?)

Diamo una definizione di **funzione propria**:
##### Definizione di funzione propria
Una funzione $f$ si dice propria quando:
- è monotona, non decrescente
- per ogni input $x$ esiste una MdT $\mathcal{M}$ con I/O che calcola $f(x)$ e tale che a partire da una configurazione iniziale 

$$
(s, \triangleright,x,\triangleright,\varepsilon, \triangleright,\varepsilon,\dots )
$$
Dove:

- $s$: stato iniziale.
- $x$: input scritto in un formato valido (ad esempio, in binario o unario).
- $\varepsilon$: nastro vuoto (non ancora utilizzato).
- $\triangleright$: delimitatore che indica l'inizio del nastro.

la MdT in $t$ steps di computazione giunge a una configurazione finale 

$$
(\{no,yes,halt\}, \triangleright, x, \triangleright,\sqcup, \dots, \triangleright,\sqcup, \sqcap^{f(x)})
$$
ossia a una configurazione in cui il nastro di output contiene solo simbolo quasi-blank per un numero di volte pari a f(x) (in binario).

**La cosa importante è che:**
1. $t \in O(\mid x\mid + \log(x))$ 
2. $\mathcal{M} \in SPACE(f(\mid x\mid))$

È importante che  $t \in O(\mid x\mid + \log(x))$  per 
1. evitare computazioni che rallentano eccessivamente al crescere di $x$. 
   Ad esempio se considerassi $t \in O(2^{\mid x\mid})$ con $\mid x\mid  = 10$ avrei $t \in O(2^{10})$, ma con $\mid x \mid = 100$ avrei $t \in O(2^{100})$ che è un numero improponibile di passi di computazione)
2. se in $t$ non ci fosse il termine $\mid x\mid$ non potremmo considerare la funzione logaritmo come una funzione propria. Se solo per leggere l'input $x$ impiego $\mid x\mid$, con $t \in  O(log(x))$ non riuscirei a terminare la lettura di $x$ !


**Esempi di funzioni proprie sono**:
- polinomio
- costante
- logaritmo
- torre di esponenziali

##### Definizione di macchina di Turing precisa
Una macchina di Turing precisa è una macchina precisa se esiste una funzione $f$ e una funzione $g$ tale che per ogni $x \in \Sigma^*$ $\mathcal{M}$ termina in tempo $f(\mid x\mid)$ usando spazio $g(\mid x\mid)$.

***Ma perchè abbiamo avuto bisogno di stabilire l'esistenza di funzioni proprie e di macchine precise?***

Perchè se dobbiamo parlare di "relazioni tra classi di complessità" abbiamo bisogno di confrontare delle funzioni. Se le funzioni che confrontiamo non hanno caratteristiche in comune o i modelli che usiamo sono diversi, i risultati potrebbero essere incoerenti o ambigui.
Per questa ragione abbiamo deciso di definire le funzioni proprie e le macchine precise. Le prime sono funzioni che godono delle proprietà sopra enunciate, ossia dei limiti spaziali e temporali richiesti per la loro computazione. Le macchine precise invece sono un modello di macchine ragionevole e realistico, in cui, ad esempio, il calcolo di una funzione lineare non richiede tempo esponenziale.  

### Che relazione c'è tra MdT precise e funzioni proprie?
Sembrerebbe che se esiste una funzione propria allora esiste una macchina precisa che la calcola in f(n) steps.

$$

\begin{gathered}
\mathcal{L} \in TIME(f(\mid x\mid)), f \text{ is proper } \\
\Downarrow \\
\exists\mathcal{M} \text{ MdT precise such that }\mathcal{M} \text{ decides } \mathcal{L}\text{ using } O(f(\mid x\mid)) \text{ steps.}
\end{gathered}


$$


### Hierarchy Theorem
