---
layout: post
title: Information Theory - Lecture 2
truncated_preview: true
categories: article
excerpt_separator: <!--more-->
tags:
  - miscellaneous
---
<!--more-->

Dalle lezioni viste a mobile communication sappiamo che un messaggio viene trasmesso seguendo un pattern ben preciso

1. prima decido il messaggio
2. poi lo codifico in un *source code* (che ad esempio lo comprime con zip o bzip)
3. poi lo codifico in un *channel code* (che ad esempio applica error correction, Viterbi o Reed Soloman)
4. infine lo trasmetto sul canale

Dalla parte del ricevente quello che succede è invece
1. ricevo il messaggio e lo decodifico attraverso un canale di decoding
2. trasformo il messaggio decodificato in una nuova versione *source decoded*
3. infine leggo il messaggio che se tutto va bene è leggibile

## Problema:
immaginiamo di avere due alfabeti:

$$
\begin{aligned}
A &= \{a_{1},a_{2},\dots,a_{n}\} \\
B &= \{b_{1}, b_{2}, \dots, b_{k}\} 
\end{aligned}$$

L'encoding Source code sarà una funzione del tipo 

$$
\varphi: A^* \to B^*
$$

ossia una funzione ***iniettiva***.

Ora, ci sono varie casistiche mediante le quali possiamo mappare gli elementi di 
$A^* \text{ in } B^*$ 

### Block-Block
Semplicemente uso una funzione del tipo:

$$
\varphi(a_{1}a_{2}\dots a_{m}) = \varphi(a_{1})\varphi(a_{2})\dots \varphi(a_{n})
$$

In questo modo creo una funzione che mappa elementi di $A$  lunghezza $n$, con elementi di $B$ di lunghezza $n$. 
### Block-variable length
Ci sono però situazioni in cui potremmo avere due alfabeti che usano un numero di elementi differente. In questo caso la mappatura non potrà essere 1 a 1, ma ci servirà un metodo differente.

Posso usare un metodo simile a questo:

$$
\begin{aligned}
A &= \{a,b,c\}\\
B &= \{0,1\}\\
\\
a &\to 0 \\
b &\to 10 \\
c &\to 01  \\
\end{aligned}
$$

In questo modo sono in grado di mappare correttamente gli elementi di $A$ in $B$.

Però il problema è che in questo caso la funzione di encoding **NON** è ***Uniquely Decodable***, ossia non è iniettiva rispetto a $A^*$ 

Ad esempio

$$\varphi(ab) = 010 = \varphi(ca)$$

### Uniquely Decodable with delay 1
Un metodo simile al precedente potrebbe essere usare dei prefissi comuni e dei suffissi che si modificano. 

$$
\begin{aligned}
A &= \{a,b,c\}\\
B &= \{0,1\}\\
\\
a &\to 0 \\
b &\to 01 \\
c &\to 011  \\
\end{aligned}
$$

Il problema principale in questo genere di codifica è che la lunghezza del testo codificato aumenta di parecchio. 
Difatti 

$$\varphi(abccbba) = 00101101101010$$

### Uniquely Decodable with unlimited delay
Un ulteriore metodo è quello di usare un numero di elementi differente come postfisso.
Ad esempio

$$
\begin{aligned}
A &= \{a,b,c\}\\
B &= \{0,1\}\\
\\
a &\to 00 \\
b &\to 1 \\
c &\to 10  \\
\end{aligned}$$

In questo caso avremmo che 

$$\varphi(b^{3}acac\dots) = 110100100010 \dots 100 \dots 01 \dots$$

se abbiamo situazioni ambigue come $100$, possiamo capire a quale simbolo facciamo riferimento guardando quanti elementi ci sono postfissi al valore 1.
- se 1 è seguito da un numero pari di 0, allora indichiamo $b$
- se 1 è seguito da un numero dispari di 0, allora indichiamo $c$

### Sono utili i codici con delay?
bè, sì perchè permettono una compressione migliore di altri possibili codici senza delay. Sì, anche se prima ho detto che in realtà allungano il messaggio iniziale. In generale sono comunque un buon compromesso (a caval donato non si guarda in bocca).

## Prefix code definition
Dato

$$\varphi: A^* \to B^*$$

Con il termine *prefix code* si intende 

$$\forall a_{1},a_{2} \in A \;\;\;\; \varphi(a_{1})\; \text{is not a prefix of}\;\varphi(a_{2}) $$ 

## Lemma
If $\varphi$ **is prefix**, the $\varphi$ uniquely decodable without delay.

***Code representation***

$$B=\{b_{1}, \dots, b_{D}\} \to \text{D-ary tree}$$

Esempio:

$$
\begin{aligned}
a &\to 00 \\
b &\to 1 \\
c &\to 10
\end{aligned}
$$

Una possibile rappresentazione è la seguente
![](/assets/images/Pasted image 20240306143316.png)

Non è un prefix code
esiste un path con più di una label

Questo metodo di codifica consiste nel scansionare l'encoded message e visitare l'albero. Quando incontriamo una label la decodifichiamo stampandola e ricominciamo di nuovo a partire dalla root.

## Definition 

$$
\begin{aligned}
A &= \{a_{1},a_{2},\dots,a_{k}\} \\
l_{i} &= \mid\varphi(a_{i})\mid
\end{aligned}
$$

## Kraft MacMillan Theorem (inverse theorem)
Se $\varphi$ è ***uniquely decodable*** allora 

$$\sum_{i=1}^k D^{-l_{i}} \le 1$$

$\varphi:A\to B^* \;\;\;\; \mid B\mid=D$ = lunghezza dell'alfabeto di output

Praticamente il teorema di Kraft-MacMillan afferma che per una data lunghezza di codice, ci devono essere abbastanza codewords disponibili per coprire tutte le possibili combinazioni di simboli di quella lunghezza.


Example 

$$
\begin{align}
\mid A\mid = k &> \mid B\mid =D \\ \\
l_{i} = 1 &= \mid\varphi(a_{i})\mid  \\ \\ 
\frac{1}{D}+\frac{1}{D}&+\dots+\frac{1}{D} = \frac{K}{D} > 1
\end{align}
$$

### Proof for prefix codes
Consideriamo un codice prefisso $\varphi$ dove

$$
\begin{aligned}
l(a_{1}) &\le l(a_{2}) &\le &\dots &\le l(a_{k}) \\
l_{1} &\le l_{2} &\le &\dots &\le l_{k} = l
\end{aligned}
$$

Consideriamo quindi il corrispondente D-albero
![](/assets/images/Pasted image 20240307090324.png)
Sappiamo che le foglie totali possono essere $D^{l_{k}}$. Difatti se ho un alfabeto {0,1} e voglio formare stringhe di lunghezza massima 4, so che il numero totale di stringhe componibili sarà $2^4$

Nel nostro caso voglio avere codici prefissi, di conseguenza se ho una stringa di lunghezza $l_i < l_k$ so per certo che tutto il ramo dell'albero a partire dal nodo $i$ dovrà essere inevitabilmente scartato. Il numero totale di stringhe scartate sarà $D^{l_{k}-l_{i}}$. Ad esempio se ho creato un albero binario di altezza massima 5 e il mio codice prevede una parola di output di lunghezza 3, avrò che il numero totale di parole sarà 32, ma di queste, quelle effettivamente utilizzabili sarà $32-8 = 26 = 2^5 - 2^{5-3}$

Il numero di foglie a cui non può essere associata una label con $a_{k}$ è 

$$\sum_{i=1}^{k-1} D^{l_{k} - l_{i}}$$

Considero il numero di foglie utilizzabili $D^{l_{k}}$ e sottraggo il numero di foglie inutili. Sò che se la codifica è ammissibile deve ammettere almeno una foglia, perciò 

$$
\begin{aligned}
&D^{l_{k}} - \sum_{i=1}^{k-1} D^{l_{k} - l_{i}} \ge 1 \\
&D^{l_{k}} - \sum_{i=1}^{k-1} \frac{D^{l_{k}}}{D^{l_{i}}} \ge 1 \\ \\
&D^{l_{k}} - \sum_{i=1}^{k-1} D^{l_{k}} \cdot D^{-l_{i}} \ge 1 \\ \\
&\text{raccolgo }D^{l_{k}} \\ \\
&D^{l_{k}} \left( 1-\sum_{i=1}^{k-1} D^{-l_{i}} \right) \ge 1 \\
&1-\sum_{i=1}^{k-1}D^{-l_{i}} \ge D^{-l_{k}}\\
&\text{porto } -\sum_{i}^{k-1}D^{-l_{i}} \text{ a destra } \\
&1 \ge \sum_{i=1}^{k} D^{-l_{i}}\\

\end{aligned}
$$


### Proof for a general Uniquely Decodable

$$
l(a_{1}) \le l(a_{2}) \le \dots \le l(a_{k}) = l
$$

Il numero di stringhe di lunghezza $n$ ($A^n$) che corrispondono a stringhe di lunghezza n è  $N(n,h)$

$N(n,h) \le D^h$ è il numero di stringhe di lunghezza $h$ di $B$

Tesi: $\sum_{i=1}^k D^{-l_{i}} \le 1$
Studio $\left(\sum_{i=1}^k D^{-l_{i}}\right)^n$ per un $n$ generico
Il passaggio cruciale è qui. 
Scomponendo la sommatoria, ottengo che 

$$
\begin{aligned}
\left(\sum_{i=1}^k D^{-l_{i}}\right)^n  = \left( D^{-l_{1}} +D^{-l_{2}} + \dots + D^{-l_{k}} \right)^n = \\
D^{n\cdot (-l_{1})}+D^{n\cdot (-l_{2})}+ \dots + D^{n\cdot (-l_{1}-l_{2}-\dots-l_{k}\cdot)} + D^{n(-l_{k})} = \\
\end{aligned}
$$

Ora mi accorgo che $D^{n\cdot (-l_{1}-l_{2}-\dots-l_{k}\cdot)}$  si può vedere come $D^{-J}$ dove $J$ è la lunghezza dell'encoding di una sequenza di $n$ simboli dell'alfabeto primario.

Quante volte $D^{-J}$ occorre nella somma? Bè, tante volte quanto il numero di parole di lunghezza $n$ i cui encoding hanno lunghezza J, perciò occorre $N(n,J$)

Ma allora, la formula di prima diventa 

$$
\begin{aligned}
&D^{n\cdot (-l_{1})}+D^{n\cdot (-l_{2})}+ \dots + D^{n\cdot (-l_{1}-l_{2}-\dots-l_{k}\cdot)} + D^{n(-l_{k})} = \\

&N(n,1)D^{-1} +N(n,2)D^{-2} + \dots + N(n,n\cdot l)D^{-n\cdot l} =  \\\\
&\text{ma sappiamo che } N(n,1) \text{ è } \le D^1 \text{e così via per tutti gli altri termini, quindi} \\ \\
&=N(n,1)D^{-1} +N(n,2)D^{-2} + \dots + N(n,n\cdot l)D^{-n\cdot l} \le \\
&\le D^1\cdot D^{-1} + D^2\cdot D^{-2} + \dots + D^{n\cdot l}\cdot D^{n\cdot l} \\
&\le n\cdot l \\

&\left(\sum_{i=1}^{k} D^{-l_{i}} \right)^n \le n \cdot l \text{(costant)} \\
&\implies \sum_{i=1}^k D^{-l_{i}} \le 1
\end{aligned}
$$





