---
layout: post
title: Relazione tra Classi di Complessità 3 - Gap Theorem
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->

Il Gap Theorem è uno di quei teoremi che quando vedi pensi: "ma perchè non rinuncio agli studi?"
Ditemelo voi, è mai possibile che esiste una funzione $f$  ricorsiva (non propria) tale che

$$
TIME(f(n)) = TIME(2^{f(n)})
$$
?

# -- GAP THEOREM --
Esiste una funzione $f$ ricorsiva tale che
$$
TIME(f(n)) = TIME(2^{f(n)})
$$
La dimostrazione è un po' tricky, quindi vorrei dare prima degli hints per avere un'intuizione, per poi passare alla dimostrazione vera e propria.

#### HINTS
1. $\mid \Sigma_{h} \mid^t$ è il numero di input della macchina $\mathcal{M}_{h}$ di lunghezza t
#### Dimostrazione
Partiamo con definire un semplice predicato:

$$
P(i,k) = \forall \mathcal{M}_{h} \text{ con } h<i \text{ e } \forall x \text{ tale che } \mid x\mid=i, \text{ ho che }\mathcal{M}_{h}(x)\downarrow \text{ in al più k passi}
$$ 
Ciò significa che 

$$
P(i,k) = 
\begin{cases}
yes \iff  \forall \mathcal{M}_{h} \text{ con } h<i \text{ e } \forall x \text{ tale che } \mid x\mid=i, \text{ ho che }\mathcal{M}_{h}(x)\downarrow \text{ in al più k passi}\\ \\
no \iff \begin{cases} \\
\exists \mathcal{M}_{h} \text{ con } h<i \text{ o } \exists x \text{ tale che } \mid x\mid=i, \text{ per cui ho che }\mathcal{M}_{h}(x)\uparrow \\ \\ 
\exists \mathcal{M}_{h} \text{ con } h<i \text{ o } \exists x \text{ tale che } \mid x\mid=i, \text{ per cui ho che }\mathcal{M}_{h}(x)\downarrow \text{ in più di k passi}
\end{cases}
\end{cases}
$$

Ottimo.

Perciò considerata una certa $i$ possiamo enumerare le macchine $\mathcal{M}$ con $h<i$ 

$$
\mathcal{M}_{1}, \mathcal{M}_{2},\dots,\mathcal{M_{h}},\dots\mathcal{M}_{i}
$$
Sappiamo che ciascuna di queste può terminare oppure no. Se termina, lo fa all'interno di un certo intervallo di tempo.

Ora consideriamo quindi una bella successione di valori $k$ 
$$
\begin{aligned}
k_{1} &= 2i\\ 
k_{2} &= 2^{k_{1}}+1\\
\dots \\
k_{n} &= 2^{k_{n-1}}+1
\end{aligned}
$$
Consideriamo quante stringhe possiamo avere per ogni $x$ tale che $\mid x \mid = i$  per ogni macchina $\mathcal{M}_{h}$.

$$
N(i) = \sum_{h=0}^i \mid \text{alfabeto di }\mathcal{M}_h \mid ^h = \sum_{h=0}^i \mid \Sigma_{h} \mid^h
$$
Quindi in parole povere sto contando quante diverse computazioni potrebbero andarmi bene, indipendentemente dalla macchina che sto usando. 

Consideriamo $N(i)$ intervalli della successione dei k. 
Ci serve per assicurarci che la macchina $\mathcal{M}_h$ su ogni input di lunghezza uguale a $i$ termini in uno di questi intervalli.

Gli intervalli sono di dimensione crescente, difatti possono essere rappresentati come

$$
[\dots)[\;\dots\;\;)[\;\;\;\dots\;\;\;)[\;\;\;\;\dots\;\;\;\;\;)[\;\;\;\;\;\dots\;\;\;\;\;\;\;)[\;\;\;\;\;\;\;\dots\;\;\;\;\;\;\;\;)[\;\;\;\;\;\;\;\;\dots\;\;\;\;\;\;\;\;\;\;)[ \dots
$$ 
Potrebbe succedere che tutte le computazioni finiscano all'interno dello stesso intervallo, ma potrebbe anche succedere che ciascuna computazione caschi all'interno di un intervallo specifico. In questo caso non sarebbe un problema, perchè abbiamo fatto in modo di avere un intervallo per ciascuna computazione. 

Facciamo però una cosa strana. Consideriamo $N(i)+1$ intervalli. In questo modo se ciascuna computazione finisce in un distinto intervallo, almeno uno di questi intervalli non verrà raggiunto da nessuna computazione della macchina $\mathcal{M}_{h}$ (perchè se gli intervalli non si intersecano tra loro non è possibile che una computazione finisca in due intervalli...).

Questo importante intervallo che considero sarà l'intervallo $k_l = [k_{l},k_{l+1}]$

Se la macchina più lenta è $\mathcal{M}_{z}$ e cade nell'intervallo $k_z = [k_z,k_{z+1}]$ noi consideriamo come ultimo intervallo $k_{z+1} = [k_{z+1}, k_{z+2}]$

Assumiamo quindi $f(i) = k_l$

Dimostriamo quindi che $TIME(f(n)) = TIME(2^{f(n)})$ ossia che  

$$
TIME(f(n)) \subseteq TIME(2^{f(n)}) \land TIME(2^{f(n)})  \subseteq TIME(f(n))
$$

$TIME(f(n)) \subseteq TIME(2^{f(n)})$ è immediato verificare che è vero

$TIME(2^{f(n)})  \subseteq TIME(f(n))$ è un po' più tricky da dimostrare.
Se $\mathcal{L} \in TIME(2^{f(n)})$ allora sappiamo che esiste una k-MdT $\mathcal{M}$ che decide $\mathcal{L}$ in $2^{f(n)}$ steps.
Ma allora se $n=i$ otteniamo che $\mathcal{L} \in TIME(2^{f(n=i)}) = TIME(2^{k_{l}})$ 

Il problema è che $2^{k_{l}}$ rientra all'interno dell'intervallo $[2^{k_{l-1}}+1, 2^{k_{l}}+1]$ eppure avevamo detto che nessuna macchina poteva terminare all'interno di quell'intervallo. Di conseguenza la macchina deve per forza terminare in al più $f(n)$ steps, quindi $TIME(2^{f(n)}) \subseteq TIME(f(n))$. 

$\square$

## Considerazioni
The Gap Theorem is important to computational complexity theory because it shows
that not every function has sensible properties when used as a complexity bound.
This leads to the concept of an proper function in computational complexity. Such
functions behave properly when used as complexity bounds.