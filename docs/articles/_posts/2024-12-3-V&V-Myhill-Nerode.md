---
layout: post
title: V&V-Myhill-Nerode Theorem
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->

# Myhill-Neorede Theorem
Per 3 anni sono riuscito a scampare la dimostrazione del teorema di Mayhill-Nerode, ma è arrivata l'ora di impararlo definitivamente.

Il teorema afferma che <b>i seguenti enunciati sono equivalenti:

1. $L \subseteq \Sigma^*$ è regolare
2. $L$ è l'unione di classi di equivalenza di $\Sigma^*$ indotte da una relazione di invariante a destra di indice finito
3. $R_{L}$ è di indice finito
</b>

Uououo, calma:
1. Che cos'è una classe di equivalenza di $\Sigma^*$ indotta?
2. Che cos'è una relazione di invariante a destra?
3. Che significa che la relazione è di indice finito? 

Cominciamo dando queste definizioni, poi riguardiamo l'enunciato del teorema:

### Che cos'è una classe di equivalenza di $\Sigma^*$ indotta?
Fondamentalmente è un sottoinsieme della classe di equivalenza principale. Il motivo principale per considerare solo la classe di equivalenza indotta è che l'intera classe di equivalenza potrebbe essere estremamente grande (se non addirittura avere cardinalità infinita). 

Facciamo un esempio: ho un alfabeto $\mathcal{A}=\{a,b\}$, $R$ è una relazione su $\mathcal{A}^*$  definita come $\text{"la stringha finisce per }a"$, allora 
- $aaaa\sim bbbba$ 
- la classe di equivalenza su $R$ è l'insieme delle strighe che terminano con $a$
- una possibile classe di equivalenza indotta potrebbe essere $\{aaa,bbba,ba,aaabbaaa\}$.

### Che cos'è una relazione di invariante a destra?
Diciamo che è un tipo di relazione con una particolarità.
Consideriamo un generico insieme $\mathcal{A}$ e una relazione $R$ su $\mathcal{A}$. Assumiamo che l'insieme sia chiuso per le operazioni di concatenazione.

$$
\forall x,y \in \mathcal{A} \mid x\sim y \to \forall z \in \mathcal{A} , xz\sim yz
$$

Quindi in pratica stiamo dicendo che la relazione è invariante a destra se concatenando lo stesso elemento dell'insieme a due elementi che sono tra loro equivalenti, abbiamo che l'equivalenza continua a valere.

### Che significa che la relazione è di indice finito? 
Questa è la cosa più facile da intuire, sbagliando, arrivati a questo punto. 
La relazione si dice di indice finito quando il numero di classi di equivalenza indotte dalla relazione è in un numero finito.

Attenzione, la definizione non dice che il numero di elementi della classe di equivalenza è finito, ma che il numero di classi indotte lo è!

Che poi in realtà se ci ragioniamo non potremmo avere delle classi di equivalenza indotte di indice finito se la classe di equivelenza principale avesse un numero infinito di elementi... vabbè.

--- 
#### Torniamo a noi...

Perciò, parafrasando il teorema visto all'inizio sappiamo che queste affermazioni sono tutte legate tra loro, ed equivalenti:
1. L è un linguaggio regolare
2. L è l'unione di sottoinsiemi di parole che appartengono alla stessa classe di equivalenza, ossia parole equivalenti tra loro rispetto a una relazione $R$. In particolare in questo caso la relazione $R$ è una relazione invariante a destra di indice finito, ossia una relazione del tipo
   
   $$
   xR_{L}​y⟺(∀z∈Σ^∗)(xz∈L⟺yz∈L).
   $$
   
   dove il numero di classi di equivalenza indotte da R è finito
3. il numero di classi di equivalenza indotte da $R_L$ è finito

Perciò in poche parole dobbiamo dimostrare che
1. 1 -> 2 : Se $$L\subseteq\Sigma^*$$ è regolare allora $L$ è l'unione di classi di equivalenza di $\Sigma^*$ indotte da una relazione di invariante a destra di indice finito
2. 2 -> 3: Se $L$ è l'unione di classi di equivalenza di $\Sigma^*$ indotte da una relazione di invariante a destra di indice finito allora $R_{L}$ è di indice finito
3. 3 -> 1: Se $R_{L}$ è di indice finito allora  $L \subseteq \Sigma^*$ è regolare

#### Dimostrazione (1)
Essendo L un linguaggio regolare, esiste un DFA $\cal{M}$ che lo accetta.

Per definizione di linguaggio riconosciuto da un automa:

$$
L = \bigcup_{q \in F} \;\{\;x\in\Sigma^* \;| \;\hat{\delta}(q_{0}, x) = q \;\}
$$
definiamo quindi la relazione $\sim_{\cal{A}}$ su un DFA $\cal{A}$ come la relazione per cui se $x,y\in\cal{A}^*$, allora $x\sim_\mathcal{A} y$ se $\delta(q_0, x) = \delta(q_0,y)$

Ci accorgiamo quindi che L è l'unione delle classi di equivalenza di $\sim\cal{_A}$ che contengono una parola $x$ per la quale $\delta(q_0,x) \in F$ 


#### Dimostrazione (2)

#### Dimostrazione (3)
