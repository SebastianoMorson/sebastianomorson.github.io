---
layout: post
title: Relazione tra Classi di Complessità 1 - Proper Functions, Precise MdT, Complement Class
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
# Funzioni Proprie e Macchine Precise
Alcune domande a cui cercheremo di dare una risposta in questo articolo sono:
- Perchè abbiamo avuto bisogno di stabilire l'esistenza di funzioni proprie e di macchine precise?
- Perchè le funzioni proprie sono interessanti?
- Esistono funzioni non proprie e macchine non precise?
- Esistono macchine precise che non calcolano funzioni proprie?
- Esistono macchine non precise che calcolano funzioni proprie?

(SUGGERIMENTO, riscriverò la domanda nel momento in cui vi risponderò, quindi se vi interessa sapere la risposta alla domanda basta fare un bel CTRL+F e cercare la domanda che vi interessa. Avrei potuto mettere un link diretto, ma perchè rendere la vita più semplice a me e a voi?)

Diamo una definizione di **funzione propria**:

--------
#### --- DEFINIZIONE DI FUNZIONE PROPRIA ---
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

-----

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

##### --- DEFINIZIONE DI MACCHINA DI TURING PRECISA ---

**Una macchina di Turing precisa è una macchina precisa se esiste una funzione $f$ e una funzione $g$ tale che per ogni $x \in \Sigma^*$ $\mathcal{M}$ termina in tempo $f(\mid x\mid)$ usando spazio $g(\mid x\mid)$.**

***Ma perchè abbiamo avuto bisogno di stabilire l'esistenza di funzioni proprie e di macchine precise?***

Perchè se dobbiamo parlare di "relazioni tra classi di complessità" abbiamo bisogno di confrontare delle funzioni. Se le funzioni che confrontiamo non hanno caratteristiche in comune o i modelli che usiamo sono diversi, i risultati potrebbero essere incoerenti o ambigui.
Per questa ragione abbiamo deciso di definire le funzioni proprie e le macchine precise. Le prime sono funzioni che godono delle proprietà sopra enunciate, ossia dei limiti spaziali e temporali richiesti per la loro computazione. Le macchine precise invece sono un modello di macchine ragionevole e realistico, in cui, ad esempio, il calcolo di una funzione lineare non richiede tempo esponenziale.  

### Che relazione c'è tra MdT precise e funzioni proprie?
Sembrerebbe che **se esiste una funzione propria allora esiste una macchina precisa che la calcola in $f(n)$ steps.**

$$

\begin{gathered}
\mathcal{L} \in TIME(f(\mid x\mid)), f \text{ is proper } \\
\Downarrow \\
\exists\mathcal{M} \text{ MdT precise such that }\mathcal{M} \text{ decides } \mathcal{L}\text{ using } O(f(\mid x\mid)) \text{ steps.}
\end{gathered}
$$

**Dimostrazione**

Consideriamo il linguaggio 
$\mathcal{L} \in TIME(f(\mid x\mid)$ 
e una macchina di Turing $\mathcal{M}$ che decide il linguaggio in $f(n)$ steps.  
Consideriamo quindi una seconda macchina $\mathcal{M}'$ che simula $\mathcal{M}$. Questa macchina fonde assieme la macchina $\mathcal{M}$ e la macchina $\mathcal{M}_{f}$ che calcola la funzione propria $f$.
$\mathcal{M}'$ non fa altro che:
1. eseguire la macchina $\mathcal{M}$ e
2. a ogni step di $\mathcal{M}$ cancellare un simbolo $\sqcap$ dal nastro di output di $\mathcal{M}_{f}$.
3. terminare quando tutti gli $\sqcap^{f(n)}$ sono stati cancellati o quando $\mathcal{M}$ termina

$$\square$$

Molto semplice come dimostrazione. 

----
# COMPLEMENTO DI CLASSI DI COMPLESSITÀ

#### --- DEFINIZIONE DI CLASSE DI COMPLESSITÀ ---
**Una classe di complessità è un insieme di linguaggi (o problemi) che possono venir risolti usando una certa complessità temporale e spaziale.**

Ad esempio sappiamo che la classe P è la classe di problemi che può essere risolta in tempo polinomiale da una MdT deterministica, mentre NEXP è la classe di problemi che può essere risolta in tempo esponenziale da una MdT non-deterministica.

Ebbene, un linguaggio (o problema) è un insieme di stringhe costruite secondo un certo schema. Ad esempio tutte le stringhe che iniziano per $a$ e terminano con $b$. È facile immaginare un possibile automa a stati finiti che riconosce tale linguaggio. 

Il complemento del linguaggio non è altro che l'insieme di tutte le stringhe che non appartengono al linguaggio. 

$$
\overline{\mathcal{L}} = \{x \mid x \not\in \mathcal{L}\}
$$
Le domande che ci vogliamo porre sono:
1. da cosa è formato il complemento di una classe di complessità?
2. Se $C$ è una classe di complessità e ho un linguaggio $\mathcal{L} \in C$ allora il suo complementare  $\overline{\mathcal{L}} \in C$ ?

Cerchiamo di rispondere alla prima domanda.

#### --- DEFINIZIONE DI COMPLEMENTO DI UNA CLASSE DI COMPLESSITÀ ---
**Il complemento di una classe di complessità C è definito come co-C e corrisponde all'insieme dei linguaggi tali per cui i loro complementari appartengono alla classe di complessità C**

$$
co-C = \{\overline{\mathcal{L}} \mid \mathcal{L} \in C\}
$$

Se $C=NP$ allora $co-NP$ corrisponde all'insieme dei linguaggi tali per cui è possibile stabilire se $x\not \in \mathcal{L}$ in tempo polinomiale.

Quindi c'è una sostanziale differenza tra scrivere $co-C$ e $\overline{C}$
- $co-C$ indica l'insieme dei linguaggi complementari ai linguaggi che appartengono a C ed è definito come sopra
- $\overline{C}$ indica l'insieme dei linguaggi che non appartengono alla classe C viene definito come $\overline{C} = \{\mathcal{L} \mid \mathcal{L} \not \in C\}$.  Quindi tutti i linguaggi che non appartengono a C appartengono a $\overline{C}$

---
**!!! OSSERVAZIONE !!!** 
È interessante notare come 
$$ co-P = P$$. 
Questo perchè ogni problema ***DETERMINISTICO*** è complementabile invertendo gli stati positivi con quelli negativi.
Di conseguenza 

$$
\begin{aligned}
TIME(f(n)) &= co-TIME(f(n)) \\
SPACE(f(n)) &= co-SPACE(f(n))
\end{aligned}
$$

Attenzione però! Questo vale solo per le classi **DETERMINISTICHE**.

---

### La complessità delle classi NON DETERMINISTICHE
Le classi di complessità NON DETERMINISTICHE non godono però della stessa doppia-inclusione, o in altre parole **non sappiamo se** $NC = co-NC$.

**Idea del motivo:**
Nel cercare di pensare a questa affermazione mi sono sempre incartato perchè sbagliavo il punto di vista da adottare, per cui voglio partire proprio con quello. 

Quando parliamo co-NP non dobbiamo pensare erroneamente all'insieme dei problemi che non si trovano in NP, **ma all'insieme dei problemi il cui <u>complemento</u> è risolvibile in NP**. 

Quando ho un problema non deterministico, ad esempio NP, ho dei linguaggi dove ci sono varie scelte non deterministiche che possono far arrivare a uno stato $yes$ o a uno stato $no$.
Se L è un linguaggio che appartiene a NP allora sò che esiste una macchina di Turing non deterministica che decide L. 

Se 
- $x\in \mathcal{L}$ la macchina di Turing non det. $\mathcal{M}$ verifica se esiste <u>almeno un</u> path radice-foglia che porta a "yes". CASO ESISTENZIALE
- $x\not\in \mathcal{L}$ la macchina di Turing non det. $\mathcal{M}$ verifica se <u>tutti</u> i path portano a foglie "no".  CASO UNIVERSALE

Il problema è che non è sempre detto che verificare il caso universale sia computabile nello stesso tempo del computare il caso esistenziale.

#### Esempio con SAT e UNSAT
SAT è un problema NP completo, difatti per verificare se una formula è soddisfacibile è sufficiente analizzare l'albero delle computazioni e in tempo polinomiale identificare un path che rappresenti un'interpretazione valida.

UNSAT è un problema che richiede di stabilire l'insoddisfacibilità della formula. Perciò UNSAT richiede di verificare tutti i possibili assegnamenti, problema che rientra in NP (?). 
