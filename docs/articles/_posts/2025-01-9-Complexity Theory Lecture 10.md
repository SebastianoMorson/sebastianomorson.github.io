---
layout: post
title: Cook-Levin Theorem
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Il teorema di Cook-Levin è uno dei miei preferiti, perchè a partire da quello si eseguono un sacco di riduzioni.

## -- TEOREMA DI COOK-LEVIN 
Il teorema di Cook-Levin dimostra due lemma 

### Lemma 1: 
$$
\text{CIRCUIT-VALUE è P-complete} 
$$
##### DIMOSTRAZIONE

###### Dimostro che $\text{CIRCUIT-VALUE} \in P$
$\text{CIRCUIT-VALUE}$ è il problema di stabilire il valore di un circuito booleano. Per farlo è sufficiente eseguire un ordinamento topologico del circuito partendo dall'output gate. La procedura si svolge in tempo polinomiale, perciò $CIRCUIT-VALUE \in P$
###### Dimostro la completezza mostrando che se $L \in P \to L \preceq CIRCUIT-VALUE$ 
Questa dimostrazione è un po' più articolata, ma non troppo.
Per cominciare bisogna considerare un generico linguaggio $\mathcal{L} \in P$. Essendo incluso in $P$, esiste una macchina di Turing $\mathcal{M}$ a k-nastri deterministica, che decide il linguaggio $\mathcal{L}$ in al più $n^k$ steps.  
Questo significa che ogni nastro è lungo al più $n^k$ celle e che è possibile rappresentare il comportamento della macchina $\mathcal{M}$ sull'input $x$ mediante una tabella $T_{\mathcal{M}}$ di dimensione $n^k \cdot n^k$ .

**OBBIEZIONE**

Qualcuno potrebbe obbiettare dicendo "eh, ma se la macchina è a k nastri abbiamo detto che ciascun nastro è lungo al più quanto il numero di step impiegati per terminare. Inoltre possiamo avere tanti nastri quanti sono il numero di passi impiegati dalla macchina per terminare. Quindi in totale il numero di celle complessivo dovrebbe essere $O(f(n))\cdot k = O(f(n))\cdot O(f(n)) = O(f(n)^2)$. Quindi se creiamo la tabella, questa dovrebbe avere dimensione $n^k \cdot n^{2k}$ ". Questa obbiezione è legittima, anche se in realtà ogni nastro contribuisce solo localmente alla modifica della configurazione successiva (non è che mi servono tutte le celle del nastro per poter calcolare le celle del nastro successivo, perchè al più viene modificata una singola cella). 

$T_{\mathcal{M}}$ è così costruita:
- ogni colonna rappresenta una stessa cella del working tape 
- ogni riga rappresenta un istante diverso di computazione
- $T_{\mathcal{M}}[i,j]$ è il contenuto della j-esima cella all'i-esimo istante di tempo

Ogni cella della tabella può contenere uno dei simboli dell'alfabeto, oppure la coppia (simbolo-stato) nel caso in cui la configurazione della macchina si trovi esattamente su quella cella, oppure (nel nastro di output) $yes/no$.
Perciò creiamo un nuovo alfabeto 
$$
\Gamma = \Sigma \cup (\Sigma \times K)\cup \{yes/no\}
$$
Ogni cella $T_{\mathcal{M}}[i,j] \in \Gamma$.

È immediato notare che la cella $T_{\mathcal{M}}[i,j]$ dipende da 3 celle in particolare
- $T_{\mathcal{M}}[i-1,j-1]$ ossia lo stato della cella alla sua sinistra nell'istante precedente
- $T_{\mathcal{M}}[i-1,j]$ ossia lo stato della cella stessa nell'istante precedente
- $T_{\mathcal{M}}[i-1,j+1]$ ossia lo stato della cella alla sua destra nell'istante precedente

A questo punto, convertendo ogni simbolo in $\Gamma$ in una stringa binaria possiamo costruire un circuito booleano che calcola il valore di $T_{\mathcal{M}}[i,j]$ prendendo in input le codifiche dei simboli contenuti in $T_{\mathcal{M}}[i-1,j-1]$, $T_{\mathcal{M}}[i-1,j]$ e $T_{\mathcal{M}}[i-1,j+1]$.

Possiamo quindi vedere la tabella $T_{\mathcal{M}}$ come l'applicazione di $n^k \cdot n^k$ circuiti booleani costruiti come sopra. 
La valutazione dei circuiti si può fare in tempo polinomiale e per tenere traccia del circuito calcolato è sufficiente usare dei puntatori che occupano spazio logaritmico. 
Termina così la dimostrazione.


### Lemma 2:

$$
\text{CIRCUIT-SAT è NP-complete}
$$

Il ragionamento in questo caso non cambia rispetto a prima, semplicemente cambiamo il nostro circuito per indicare anche quale scelta non deterministica stiamo considerando. Difatti in una NMdT potremmo avere due transizioni diverse anche se lo stato è lo stesso, Perciò aggiungiamo un ulteriore ingresso al circuito.

