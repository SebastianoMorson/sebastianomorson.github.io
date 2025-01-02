---
layout: post
title: Secondo teorema di Büchi
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
# Teorema
Un $\omega$-linguaggio $L \subseteq A^\omega$ è $\omega$-regolare se e solo se è definibile in $S1S$.

**Dimostrazione**
***->"Se un $\omega$-linguaggio $L \subseteq A^\omega$ è $\omega$-regolare allora è definibile in $S1S$"***

Ovviamente direi che la dimostrazione si basa sul fatto di riuscire a definire una $S1S$ formula a partire da qualsiasi linguaggio $\omega$-regolare.

Per prima cosa quindi bisogna dare una descrizione generale di un linguaggio in $S1S$ che descrive proprio il linguaggio riconosciuto da un automa $\cal{A} =$ ($Q$, $\Delta$, $q_{0}$,$F$,_A_).
Consideriamo quindi il linguaggio riconosciuto dall'automa $\cal{A}$ e associamolo a una formula in $S1S$ che possa modellarne il comportamento.

$$
L(A) = \exists Y_{0}\exists Y_{1}\dots\exists Y_{m}\varphi(Y_{0},Y_{1},\dots,Y_{m})
$$

Perciò ogni $\omega$-parola $\alpha \in A^\omega$ accettata dall'automa $\cal{A}$ se e solo se avremo che $\underline{\alpha}$ è un modello di $\exists Y_{0}\exists Y_{1}\dots\exists Y_{m}\varphi(Y_{0},Y_{1},\dots,Y_{m})$ 
Se vi state chiedendo cosa dovrebbero essere quegli $Y_{i}$ ebbene non sono altro che gli insiemi delle posizioni in cui la computazione finisce nello stato $i$.

A questo punto quindi abbiamo una versione generica di una formula che possa rappresentare il mio $\omega$-linguaggio.
Ora dobbiamo raffinare la formula $\varphi$ mostrando come dovrebbe fare per gestire 
1. lo start della computazione nello stato $q_0$
2. il fatto che la computazione possa trovarsi in un solo stato ad ogni step
3. le transizioni date dalla funzione $\Delta$
4. il fatto che una parola è accettata solo l'automa passa infinite volte per uno stato finale

###### 1. imporre che la computazione inizi dallo stato 0

$$
\varphi_{1}(Y_{1},\dots,Y_{m}) = 0 \in Y_{0}
$$

Semplicemente diciamo che l'istante $0$ è presente all'interno dell'insieme degli step in cui l'automa si trova nello stato $q_0$
###### 2. Imporre che la computazione in ogni istante si trovi in al più uno stato

$$
\varphi_{2}(Y_{1},\dots, Y_{m}) = \bigwedge_{i\neq j}\lnot\exists y(y \in Y_{i} \land y \in Y_{j})
$$

L'idea della formula è: non possono mai esistere due stati $i$ e $j$ per $i$ quali un qualsiasi step di computazione che si trova sia nell'insieme degli step dello stato $i$, che in quello dello stato $j$
###### 3. Imporre che la computazione debba procedere sulla base della relazione di transizione $\Delta$ di $\cal{A}$

$$
\varphi_{3}(Y_{1},\dots,Y_{m}) = \forall x \bigvee_{(i,a,j)\in \Delta} (x \in Y_{i} \land x \in Q_{a} \land x+1 \in Y_{j})
$$

L'idea della formula è: per ogni istante della computazione $x$, se esiste una transizione da uno stato $i$ a uno stato $j$, allora l'istante $x$ si trova dentro l'insieme degli step in cui si finisce nello stato $i$ e il successivo step $x+1$ si trova all'interno dell'insieme degli step in cui si finisce nello stato $j$.

###### 4. Imporre che una computazione di successo debba attraversare infinite volte almeno uno stato finale

$$
\varphi_{4}(Y_{1},\dots,Y_{m}) = \bigvee_{i \in F}\forall x \exists y(x<y \land y \in Y_{i})
$$

L'idea della formula è: per ogni istante della computazione $x$ esiste uno step successivo che rientra tra gli step in cui si finisce all'interno di uno stato finale.

###### Fusione:
A questo punto è chiaro che posso costruire una rappresentazione in S1S per ogni automa $\cal{A}$ che riconosce un linguaggio $\omega$-regolare unendo assieme le formule fin qui definite

$$
L(A) = \exists Y_{0}\exists Y_{1}\dots\exists Y_{m}(\varphi_{1}(Y_{0},\dots,Y_{m}) \land \varphi_{2}(Y_{0},\dots,Y_{m}) \land \varphi_{3}(Y_{0},\dots,Y_{m}) \land \varphi_{4}(Y_{0},\dots,Y_{m}))
$$

E qui finisce la dimostrazione in un verso. Ora viene il verso opposto.

***<-"Se definisco un linguaggio in $S1S$ allora è un $\omega$-linguaggio $L \subseteq A^\omega$  $\omega$-regolare"***

In questo caso l'obiettivo è costruire un $\omega$-linguaggio $\omega$-regolare dato qualsiasi linguaggio in $S1S$. Per farlo invece di usare una qualsiasi $S1S$ usiamo $S1S_{0}$, quindi dobbiamo anche dimostrare che $S1S \equiv S1S_{0}$ (ossia una doppia inclusione)

Ma che vuol dire $S1S_{0}$? 
$S1S_{0}$ è una variante di $S1S$ che fa uso solo di 
- variabili del second'ordine
- formula atomica nella forma Succ(X,Y)  
- formule atomiche nella forma $X\subseteq Y$

Per chi se lo fosse dimenticato, $S1S$ usava praticamente di tutto:
- variabili del prim'ordine (per esprimere proprietà puntuail)
- variabili del second'ordine (per esprimere proprietà globali)
- quantificatori esistenziali e universali
- operatori booleani


##### 1. Dimostriamo che $S1S_{0}$ ha lo stesso potere espressivo di $S1S$ 
Vogliamo dimostrare che le due formulazioni hanno lo stesso potere espressivo perchè altrimenti sarebbe inutile dimostrare che ogni formulazione di $S1S_0$ è rappresentabile attraverso un automa di Büchi...

Perciò
###### Dimostriamo che ogni formula in $S1S$ può essere riscritta come $S1S_0$
Qui c'è una lunga lista di traduzioni da fare, per cui usiamo delle abbreviazioni per evitare di prendere fuoco mentre scriviamo formule lunghissime.
Useremo

$$
\begin{equation}
\begin{aligned}  
X = Y &\equiv X\subseteq Y \land Y \subseteq X \\
X \neq Y &\equiv \lnot (X=Y) \\
Sing(X) &\equiv \exists Y(Y\subseteq X \land Y \neq X \land \lnot \exists Z(Z \subseteq X \land Z\neq Y\land Z\neq X)) 
\end{aligned}
\end{equation}
$$

Sing non fa altro che stabilire che dentro X ci sta solo un elemento.

Dopo di che eliminiamo i simboli 0 e il predicato < perchè in $S1S_0$ non compaiono.
Trasformiamo +1 in 

$$
\exists y (y=x+1 \land y \in X)
$$

A questo punto non facciamo altro che trasformare le formule atomiche che usano variabili del prim'ordine in formule atomiche che usano solo variabili del second'ordine. Perciò:

$$
\begin{equation}
\begin{aligned}
x=y &\equiv X = Y \land Sing(X) \land Sing(Y) \\
x+1=y &\equiv Succ(X,Y) \\
\end{aligned}
\end{equation}

$$

Trasformiamo le quantificazioni del prim'ordine in quantificazioni del second'ordine

$$
\begin{equation}
\begin{aligned} \\
\exists x &\equiv \exists X(Sing(X) \land \dots) \\
\forall x &\equiv \forall X(Sing(X) \implies \dots)
\end{aligned}
\end{equation}
$$

###### Dimostriamo che ogni formula in $S1S_{0}$ può essere riscritta come $S1S$
- traduciamo $X \subseteq Y$ 

$$
\forall x(x \in X \to x \in Y)
$$

- traduciamo Succ(X,Y) 

$$
\exists x \exists y((x \in X \land \lnot \exists z \neq x \land z \in X) \land ((y \in Y \land \lnot \exists z \neq y \land z \in Y)) \land (x +1 = y))
$$

#### 2. Dimostriamo che ogni formulazione in $S1S_{0}$ è modellabile tramite automa di Büchi 

![[Pasted image 20241218093604.png]]