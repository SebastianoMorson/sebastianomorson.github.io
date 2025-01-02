---
layout: post
title: Information Theory - Lecture 1
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
La quantità di informazione presente all'interno di una frase non è fissa, ma è dipendente dal contesto. 

Ad esempio se dico "oggi piove", nel caso in cui mi trovi a Gemona non sarebbe chissà che novità, ma se lo dicessi mentre sono nel deserto, allora è ben altra storia.


Assumiamo quindi di avere un evento $E$ e che a tale evento sia associata una probabilità $p(E)$.
- se $p(E) \to 0$ allora $\frac{1}{p(E)} \to \infty$
- se $p(E) \to 1$ allora $\frac{1}{p(E)}\to 1$

Vedendo questa cosa con la notazione logaritmica, otteniamo che $log\left( \frac{1}{p(E)} \right) = -\log(p(E))$
- se $p(E) \to 0$        allora $-\log p(E) \to +\infty$
- se $p(E) \to 1$        allora $-\log p(E)\to 0$

---
#### Definizione di Shannon Entropy
Dato un alfabeto $A=\{a_{1},a_{2},\dots,a_{k}\}$ e una distribuzione di probabilità $P=\{p_{1},p_{2},\dots,p_{k}\}$
dove $p_{i}$ è la probabilità di osservare $a_i$ 
Allora la quantità di informazione media (Average Quantity of Information) è data da 
$$  H(P) = \sum_{i}^k \cdot{p_{i}} \cdot{\left(-\log p_{i}\right)}$$

Esempio:
Se ho una moneta non truccata, avrò due eventi possibili: $A=\{H,T\}$ con probabilità associate $p(H) = \frac{1}{2}$ e $p(T) = \frac{1}{2}$ 

Allora la Shannon Entropy sarà data da 
$$
\begin{aligned}
H(P) &= \frac{1}{2}\cdot{\left(-\log{\frac{1}{2}}\right)}+\frac{1}{2}\cdot{\left(-\log\ \frac{1}{2} \right)}\\
&=\frac{1}{2}+\frac{1}{2} = 1
\end{aligned}

$$

Se invece la moneta non è equa e ha una probabilità $p(H) = 0.9$ e $p(T)=0.1$ allora 
$$
\begin{aligned}
H(P) &= 0.9\cdot{\left(-\log{0.9}\right)}+0.1\cdot{\left(-\log\ 0.1 \right)}\\
&=\text{circa}\;\;0.32
\end{aligned}

$$

Se invece provo a usare probabilità $p(H)=1$ e $p(T)=0$ scopro che $H(P)=0$ (giustamente, perchè che informazione ottengo se capita sempre testa o sempre croce?)



Ma cosa succede se invece di avere un alfabeto composto da 2 singoli eventi, ho un alfabeto composto da eventi che sono la combinazione di altri eventi?

Per esempio, come calcolo l'entropia di Shannon se ho un alfabeto $A=\{\text{sequenze di lunghezza}\; n\; \text{di H e T} \}$
con $P(HHH...H) = \frac{1}{2^n}$  (ossia dove tutti hanno la stessa probabilità)?


Bè, in questo caso $H(P)$ dev'essere visto in questo modo:

$$
\begin{aligned}
H(P) &= \sum_{i=1}^{2^n} \frac{1}{2^n}\left(-\log \frac{1}{2^n} \right)\\
&= 2^n\left( \frac{1}{2^n}\cdot n \right)\\
&=n
\end{aligned}
$$

$n$ bit di informazione sono ricevuti flippando n volte una moneta *fair*.


Perciò in generale 
#### General case
$$
\begin{aligned}
H(P) &= \sum_{i=1}^k p_{i}\cdot(-\log p_{i})\\
&= - \sum_{i=1}^k p_{i}\log p_{i}\\
&= \sum_{i=1}^k p_{i}\log \frac{1}{p_{i}}
\end{aligned}
$$

dove $(-\log p_{i})$ è la quantità di informazione dell'evento $i$

#### Property of $H(P)$ n° 1: $H(P) \ge 0$
Non può essere negativa l'entropia perchè abbiamo che l'entropia minima è 0 nel caso in p(E) = 1 (evento certo), mentre è $\infty$ se la probabilità di un evento è 0.

#### Property of H(P) n°2 : $H(P) =0$ se e solo se c'è un evento con probabilità 1
Questo è dato dal fatto che log(1)=0 e quindi l'argomento del logaritmo annullerebbe l'entropia nel caso p(E) = 1

#### Property of H(P) n°3: $H$ è continua in P
Difatti infinitesimali variazioni in P producono infinitesimali variazioni in H.

#### Property of H(P) n°4 : se un evento viene diviso l'entropia deve essere additiva


#### Property of H(P) n°5 : date due distribuzioni indipendenti X,Y allora $H(X\land Y) = H(X)+H(Y)$
Ecco, questa è la proprietà che al primo appello dell'esame di complexity mi scordai e mi costrinse a ritirarmi per la vergogna.

Allora... All'esame avrei dovuto ricordarmi di 2 cose fondamentali:
1. il logaritmo gode di una proprietà interessante, ossia che $\log(x\cdot y) = \log(x)+\log(y)$
2. il buon vecchio teorema di Bayes ci dice che la probabilità condizionata è data da 

$$
p(A\mid B) = \frac{P(B\mid A) \cdot P(A)}{P(B)}
$$

Se X e Y sono due distribuzioni indipendenti significa che 

$$
H(X,Y) = - \sum_{i,\;j}{p(x,y)\cdot \log(p(x,y)})
$$
Però sappiamo che per gli eventi indipendenti $p(x,y) = p(x)\cdot p(y)$
Ma allora

$$
H(X,Y) = - \sum_{i,\;j}{p(x)\cdot p(y)\cdot \log(p(x)\cdot p(y)})
$$
e quindi

$$
\begin{equation}
\begin{aligned}
H(X,Y) &= - \sum_{i,\;j}{p(x_{i})\cdot p(y_{j})\cdot \log(p(x_{i})+ \log(p(y_{j})))} \\
H(X,Y) &= - \sum_{i,\;j}{p(x_{i})\cdot p(y_{j})\cdot \log(p(x_{i}))} - \sum_{i,\;j}{p(x_{i})\cdot p(y_{j})\cdot \log(p(y_{j}))} \\

H(X,Y) &= - \sum_{i}{p(x_{i})\cdot \log(p(x_{i}))}\cdot\sum_{j}(p(y_{j})) - \sum_{j}{p(y_{j})\cdot \log(p(y_{j}))}\cdot \sum_{i}{p(x_{i})} \\

\end{aligned}
\end{equation}

$$
Essendo $\sum_{j}p(y_{j}) =1$ (perchè sommo tutte le probabilità della distribuzione Y, quindi la somma deve fare 1 per forza di cose) ottengo che

$$
\begin{equation}
\begin{aligned}
H(X,Y) &= - \sum_{i}{p(x_{i})\cdot \log(p(x_{i}))} - \sum_{j}{p(y_{j})\cdot \log(p(y_{j}))}\\
H(X,Y) &= H(X) + H(Y)
\end{aligned}
\end{equation}
$$

##### Che succede se le distribuzioni sono dipendenti?
Se X e Y sono due distribuzioni dipendenti significa che 

$$
H(X,Y) = - \sum_{i,\;j}{p(x\mid y)\cdot \log(p(x\mid y)})
$$
Però sappiamo che per gli eventi dipendenti $p(x\mid y) = \frac{p(y\mid x)\cdot p(x)}{p(y)}$
Ma allora

$$
\begin{equation}
\begin{aligned}
H(X,Y) &= - \sum_{i,\;j}{\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})} \cdot \log\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})}} \\ \\

H(X,Y) &= - \sum_{i,\;j}\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})} \cdot (\log(p(y_{j}|x_{i}))\cdot p(x_{i}))-\log(p(y_{j}))) \\ \\

H(X,Y) &= - \sum_{i,\;j}\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})} \cdot (\log(p(y_{j}|x_{i}))+\log(p(x_{i}))-\log(p(y_{j}))) \\ \\

H(X,Y) &= - \sum_{i,\;j}\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})} \cdot (\log(p(y_{j}|x_{i}))) - \sum_{i,\;j}\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})} \cdot (\log(p(x_{i})))  - \sum_{i,\;j}\frac{p(y_{j}|x_{i})\cdot p(x_{i})}{p(y_{j})} \cdot (\log(p(y_{j}))) \\ \\
\end{aligned}
\end{equation}
$$
possiamo rimuovere $p(x_i)$ e $p(y_j)$ perchè possiamo vederli come $\frac{\sum_{i}p(x_{i})}{\sum_{j}p(y_{j})} = 1$
$$
\begin{equation}
\begin{aligned}
H(X,Y) &= - \sum_{i,\;j}p(y_{j}|x_{i}) \cdot \log(p(y_{j}|x_{i})) - \sum_{i,\;j} p(y_{j}|x_{i}) \cdot \log(p(x_{i}))  - \sum_{i,\;j}p(y_{j}|x_{i}) \cdot \log(p(y_{j})) \\ \\
\end{aligned}
\end{equation}
$$



La funzione $H$ definita da Shannon è l'unica funzione che soddisfa tutte queste proprietà


#### Proprietà di Shannon 
$$H(p_{1},\dots,p_{k}) \le \log k = H\left(\frac{1}{k},\dots, \frac{1}{k}\right)$$
$k=|A|$

che fondamentalmente significa che l'entropia è massima se la distribuzione delle probabilità è uniforme

**Dimostrazione**:
Uso Jensen (al contrario), ossia $$\sum \lambda_{i} \cdot f(x_{i})\le f\left( \sum(\lambda_{i} \cdot x_{i}) \right)$$
La dimostrazione per intero è presente [[Lect 1 Nota 28 set 2020.pdf#page=8|qui]]
