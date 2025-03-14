---
layout: post
title: V&V-Automi a stati finiti su parole infinite
excerpt_separator: <!--more-->
categories: article
truncated_preview: true
tags:
  - miscellaneous
---
<!--more-->
# Automi di Büchi (non deterministici)
Perchè non ci bastano DFA,NFA e epsilon-NFA?
Quali sono le situazioni reali modellabili solo attraverso l'uso di automi a parole infinite?

## Definizione di automa di Büchi 
Un automa di Büchi è una cosa di questo genere

![BuchiAutomata](/assets/images/Automate_de_Buchi.jpg)

Voi direte: "ma quello non è un NFA"?
Bè, in realtà sì, la forma è quella, ma a conti fatti:
- un **NFA** accetta una <u>parola finita</u> $x$ se <u>almeno una computazione</u> termina in uno stato finale.

- un **automa di Büchi** tanto per cominciare non lavora su parole finite, ma invece lavora su <u>parole infinite</u> e poi accetta una parola <u>se uno stato finale viene visitato infinite volte</u> durante la computazione della parola infinita.


## Linguaggi regolari e linguaggi $\omega$-regolari
Se ben ricordiamo, i linguaggi regolari erano definiti come quei linguaggi riconosciuti da un automa a stati finiti. In particolare a partire da un linguaggio regolare avevamo detto che era sempre possibile costruire il suo corrispettivo DFA e a partire da un DFA è sempre possibile ricondursi all'espressione regolare che identifica il linguaggio riconosciuto dall'automa (che è ovviamente regolare).

Ecco, un linguaggio $\omega$-regolari non è altro che un linguaggio riconosciutio da un automa che opera su parole infinite. Perciò un linguaggio $\omega$-regolare sarà composto da parole infinite.

Dato un automa di Büchi, il linguaggio da esso riconosciuto sarà $\omega$-regolare.

Ora però ci vogliamo domandare: che relazioni ci sono tra linguaggi regolari e $\omega$-regolari? 

### Proprietà di chiusura
>**Per i linguaggi regolari e $\omega$-regolari valgono le seguenti proprietà:**
>1. **se $V\subseteq A^*$ è regolare allora $V^{\omega}$ è $\omega$-regolare**
>2. **se $V \subseteq A^*$ è regolare e $\mathcal{L} \subseteq A^\omega$ è $\omega$-regolare allora $V\cdot \mathcal{L}$ è $\omega$-regolare**
>3. **se $\mathcal{L}_{1},\mathcal{L}_{2}\subseteq A^\omega$ sono $\omega$-regolari allora $\mathcal{L}_{1}\cup \mathcal{L}_{2}$ e $\mathcal{L}_{1}\cap \mathcal{L}_{2}$ sono $\omega$-regolari.**

La costruzione degli automi è **effettiva** (ossia esiste un algoritmo che in modo sistematico e non ambiguo permette di costruire un automa a partire da un'espressione).

---
#### 👁️ Osservazione: 
Quando giocavamo con i linguaggi regolari, abbiamo visto che $\mathcal{L}^*$ indica il linguaggio composto da parole formate  concatenando un numero indefinito (ma finito) di elementi di $\mathcal{L}$.

Ebbene con i linguaggi $\omega$-regolari non possiamo usare la stessa notazione della chiusura di Kleene per indicare la concatenazione di un numero indefinito di elementi di $\mathcal{L}$, perchè la concatenazione potrebbe essere di un numero <u>infinito</u> di elementi di $\mathcal{L}$.
Per questo usiamo la notazione $\mathcal{L}^\omega$ per definire l'operazione di concatenazione di un numero infinito di elementi di $\mathcal{L}$.

---

#### Dimostrazione 1 
È sufficiente osservare che V è regolare esiste un DFA che lo riconosce. Ma allora per riconoscere una parola infinita è sufficiente sostituire ogni arco che porta a uno stato finale con un arco che porta allo stato iniziale dell'automa, imponendo che l'unico stato finale sia proprio lo stato iniziale.

$\square$

#### Dimostrazione 2
Se $\mathcal{V}$ è regolare allora esiste una automa $\cal{M}$ che lo riconosce e se V è anch'esso regolare esiste un secondo automa $\cal{M}'$ che lo riconosce. Ma allora se costruissimo $\cal{M}''$ come l'automa $\cal{M}$ a cui ogni stato finale porta allo stato iniziale di $\cal{M}'$, avremmo che $\cal{M}''$ riconosce la concatenazione di $\mathcal{L}$ e $\mathcal{V}$. Perciò $\mathcal{V}\cdot \mathcal{L}$ è $\omega$-regolare.

$\square$


#### Dimostrazione 3
Per dimostrare la chiusura rispetto all'unione è sufficiente costruire un automa $\cal{M}$ come la composizione dei due automi $\cal{M}',\cal{M}''$ (gli automi che riconoscono $\mathcal{L}_{1}$ e $\mathcal{L}_{2}$ ). In particolare lo stato iniziale di $\cal{M}$ raggiunge tramite una $\epsilon$-transizione sia lo stato iniziale di $\cal{M}'$ che quello di $\cal{M}''$


Posso dimostrare la chiusura rispetto all'intersezione usando le leggi di deMorgan. 

$\square$

### Espressioni $\omega$-regolari 
Un'espressione regolare è un'espressione per cui esiste un DFA che accetta tale espressione. 

>Un'espressione $\omega$-regolare invece è un'espressione strutturata come
>$$
>\bigcup_{i=1}^n U_{i}\cdot V_{i}^\omega
>$$
>dove $\forall\; 1<i<n$ abbiamo che $U_{i}$ e $V_i$ sono espressioni regolari. 
>
>$n$ indica il numero di unioni che vogliamo considerare.

---
**🎯Esempio:**

Consideriamo un alfabeto $A=\{a,b\} $ e immaginiamo di descrivere un linguaggio $\omega$-regolare composto da parole infinite che:

- Possono iniziare con una sequenza arbitraria di a (cioè $U_1=a^*$).
    - Poi ripetono all'infinito solo la parola ab (cioè $V_1=ab$).
- Oppure possono iniziare con una sequenza arbitraria di $b$ (cioè $U_2=b^∗$).
    - Poi ripetono all'infinito solo la parola baba (cioè $V_2=ba$).

L'espressione $\omega$-regolare corrispondente sarebbe:

$$
(a^∗\cdot(ab)^\omega)\cup(b^*⋅(ba)^\omega)
$$

dove n=2, perché abbiamo due componenti nell'unione.

---

**Esistono linguaggi di parole infinite non $\omega$-regolari?**

Bè, a rigor di logica sì. Esempio se considerassi le parole con un numero finito di $a$ e $b$ non sarei in grado di definire un automa di Büchi in grado di riconoscerlo. Questo perchè l'automa dovrebbe essere in grado di determinare quando sono state incontrate un numero finito di occorrenze di $a$ e $b$, ma questo va oltre le capacità del modello. 

### Equivalenza tra automi di Büchi e linguaggi $\omega$-regolari

#todo

## PROPRIETÀ DI CHIUSURA

### $\omega$-chiusura
L'$\omega$-chiusura corrisponde a modificare l'automa di Buchi corrispondente sostituendo ogni stato finale con lo stato iniziale.


### Concatenazione
Se ho due automi $\mathcal{A}$ e $\mathcal{B}$ che riconoscono i linguaggi $\omega$-regolari $\mathcal{L}$ e $\mathcal{L}'$ è possibile costruire l'automa che riconosce la loro concatenazione semplicemente modificando l'automa $\mathcal{A}$ in modo che gli stati finali siano sostituiti dagli stati iniziali dell'automa $\mathcal{B}$.


### Intersezione
Se ho due automi $\mathcal{A}$ e $\mathcal{B}$ che riconoscono rispettivamente i linguaggi $\mathcal{L(A)}$ e $\mathcal{L(B)}$ allora è possibile costruire l'automa $\mathcal{A'}$ che riconosce l'intersezione tra $\mathcal{L(A)}$ e $\mathcal{L(B)}$. 

Il metodo per costruire $\mathcal{A'}$ non è proprio immediato e richiede un attimo di ragionamento.

Per prima cosa dobbiamo ragionare sul problema: se vogliamo individuare il linguaggio intersezione, dobbiamo verificare se una parola che appartiene a $\mathcal{A}$ appartenga anche a $\mathcal{B}$. In quel caso l'elemento fa parte dell'insieme intersezione.

Il problema principale è che questa parola è infinitamente lunga e quindi dobbiamo far sì che l'automa che riconosce tale parola verifichi infinite volte che il prefisso della parola attraversi un numero infinito di volte uno stato finale di $\mathcal{A}$ E uno stato finale di $\mathcal{B}$. 

Per fare questo controllo costruiamo in questo modo $\mathcal{A'}$:
1. consideriamo come stati le triple $(q_A, q_B, i)$ (con i = 1 o i = 2). L'indice $i$ fungerà da "gate" tra le computazioni di $\mathcal{A}$ e $\mathcal{B}$.

2. consideriamo come funzione di transizione la funzione $\Delta'$ definita come $\Delta' = \Delta_1 \cup \Delta_2$ dove:
    - $\Delta_1 = \{(q_\mathcal{A}, q_\mathcal{B}, 1) \to (q_\mathcal{A}', q_\mathcal{B}', i) \mid (q_\mathcal{A}, a, q_\mathcal{A}') \in \delta_\mathcal{A} \land  (q_\mathcal{B}, a, q_\mathcal{B}') \in \delta_\mathcal{B} \}$
        - $i=1$ se $q_\mathcal{A} \not \in F_\mathcal{A}$ (ossia se non mi trovavo in uno stato finale di $\mathcal{A}$)
        - $i=2$ se $q_\mathcal{A} \in F_\mathcal{A}$ (ossia se mi trovavo in uno stato finale di $\mathcal{A}$ e quindi ora voglio usare il gate per switchare sulla computazione di $\mathcal{B}$)

    - $\Delta_2 = \{(q_\mathcal{A}, q_\mathcal{B}, 2) \to (q_\mathcal{A}', q_\mathcal{B}', i) \mid (q_\mathcal{A}, a, q_\mathcal{A}') \in \delta_\mathcal{A} \land  (q_\mathcal{B}, a, q_\mathcal{B}') \in \delta_\mathcal{B} \}$
        - $i=1$ se $q_\mathcal{B} \in F_\mathcal{B}$ 
        
            (ossia se mi trovavo in uno stato finale di $\mathcal{B}$ e quindi ora voglio usare il gate per switchare sulla computazione di $\mathcal{A}$)

        - $i=2$ se $q_\mathcal{B} \not \in F_\mathcal{B}$

3. consideriamo come stati finali gli stati del tipo $(\_, q \in F_\mathcal{B}, 2)$. 

    $$
    F = \{ (q_\mathcal{A}, q_\mathcal{B}, 2) \mid q_\mathcal{B} \in F_\mathcal{B} \}
    $$

    In pratica ci troviamo in uno stato finale se mi trovo in uno stato finale di $\mathcal{B}$ e il gate si trova su $2$. 

    Ovviamente questo vale se la prima computazione da cui inizio è quella di $\mathcal{A}$, altrimenti imporrò che gli stati finali siano $(q \in F_\mathcal{A}, \_, 1)$

### Unione
Se ho due automi $\mathcal{A}$ e $\mathcal{B}$ che riconoscono i linguaggi $\omega$-regolari $\mathcal{L}$ e $\mathcal{L}'$ è possibile costruire l'automa che riconosce la loro unione semplicemente fondendo assieme l'insieme degli stati (assumendo che siano disgiunti).



## Complementazione 
Prima di capire cosa vuol dire "complementazione" bisogna dare alcune definizioni:

#### CONGRUENZA: 
>Una relazione di equivalenza $\sim$ su un generico insieme A si dice di congruenza se $\forall x,y,x',y' \in A$ si ha che se $x \sim y$ e $x' \sim y'$ allora $xx' \sim yy'$ 

In altre parole, definita una relazione di equivalenza su A è necessario verificare se ogni coppia di equivalenza è invariante rispetto all'operazione di concatenazione.

---

**🎯Esempio:**

$A = \{a,b\}$ 
$x \sim y \iff  \text{"x e y hanno la stessa lunghezza"}$
Assumendo $x = a \;\; y=b$ 
Assumendo $x' = b \;\; y' = a$
Si ha che $x\sim y$ e $x'\sim y'$ e $xx' \sim yy'$

---

Si può verificare che ogni possibile coppia di equivalenza è invariante rispetto alla concatenazione, perciò $\sim$ è una relazione di congruenza

#### SATURAZIONE:
>Dato un linguaggio $L\subseteq A^\omega$ e una congruenza $\sim$, diciamo che $\sim$ satura $\mathcal{L}$ se per ogni coppia di $\sim$-classi U e V, $U\cdot V^\omega \cap L \neq \emptyset$ allora $U\cdot V^\omega \subseteq L$.

Spieghiamoci meglio:
- quando dico $\sim$-classi intendo, nell'esempio in cui $\sim$ è la relazione "x e y hanno la stessa lunghezza", gli insiemi del tipo "tutte le stringhe di lunghezza 1", "tutte le stringhe di lunghezza 2", ... 
- in teoria le $\sim$-classi sono costruite a partire all'insieme di parole finite $A^*$ 

Da quello che ho capito, è importante specificare che $U\cdot V^\omega \cap L \neq \emptyset$ perchè in questo modo basta trovare una parola che si trovi all'interno di L, per poter concludere che tutte le parole in $U\cdot V^\omega \subseteq L$.  
Questa cosa però non torna: anche se una parola si trova dentro L, non vuol dire che i due insiemi sono inclusi uno nell'altro. Potrebbero avere solo quell'elemento in comune. Cosa fa quindi la differenza?
 
La differenza la fa la presenza della relazione di conguenza $\sim$ .
$U$ e $V$ sono due classi di equivalenza, quindi gli elementi al loro intero possiedono le medesime proprietà (ad esempio $U$ è composta da tutte e sole le stringhe di lunghezza 2, mentre $V$ da tutte e sole le stringhe di lunghezza 3). Perciò ogni stringa della classe di equivalenza può essere sostituita con una qualsiasi altra parola della stessa classe. 

Ma quindi se l'intersezione tra $\mathcal{L}$ e $U\cdot V^\omega$ non è vuota, significa che almeno una parola $x$ appartiene a entrambi gli insiemi, ma essendo che  $x$ è costruita a partire da 2 classi di equivalenza, posso riscrivere $x$ sostituendo qualsiasi elemento delle classi di equivalenza di partenza, e quindi posso saturare il linguaggio $\mathcal{L}$. 

Pensare a questa cosa mi ricorda un virus dentro una cellula. Una volta che è dentro inizia a moltiplicarsi fino a saturare completamente il suo contenuto con tante copie di sè stesso.


--- 

##### 🎯 Esempio pratico
Congruenza: stessa lunghezza

Supponiamo che $\omega$ sia la relazione "due parole hanno la stessa lunghezza".
Allora le $\omega$-classi sono insiemi del tipo:
```
U={a,b}U={a,b} (parole di lunghezza 1)
V={aa,bb,ab,ba}V={aa,bb,ab,ba} (parole di lunghezza 2)
```
Allora $V^\omega$ è l'insieme di tutte le parole infinite ottenute concatenando ripetutamente parole di lunghezza 2.

Ora consideriamo un linguaggio $\mathcal{L}$ che accetta solo parole infinite in cui ogni coppia di lettere consecutive ha la stessa lunghezza di ciclo, e supponiamo che una parola $a(aa)(bb)(ab)(ba)^\omega$ appartenga a $\mathcal{L}$.
Poiché qualsiasi parola in $U⋅V^\omega$ ha la stessa struttura e proprietà, allora tutto $U\cdot V^\omega$ deve essere incluso in $\mathcal{L}$, altrimenti $\mathcal{L}$ non sarebbe coerente con la classe di equivalenza.

--- 


### Proposizione
>Siano $\sim$ una congruenza e $L \subseteq A^\omega$. Se $\sim$ satura $L$ allora $\sim$ satura pure $\overline{L}$

**Dimostrazione:**

Assumiamo che L sia saturato da $\sim$
Se $U\cdot V^\omega \cap L \neq \emptyset$  allora $U\cdot V^\omega \subseteq L$

Assumiamo per assurdo che $\overline{L}$ non sia saturato da $\sim$.
Perciò se $U\cdot V^\omega \cap L \neq \emptyset$ allora $U\cdot V^\omega \nsubseteq \overline{L}$ 

Se $U\cdot V^\omega \nsubseteq \overline{L}$ significa che esiste una parola $\alpha \in U\cdot V^\omega$ che non appartiene a $\bar{L}$. Ma se non appartiene a $\bar{L}$ allora appartiene a $L$.

Essendo che $\sim$ satura $L$ vorrebbe dire che $U\cdot V^\omega \subseteq L$ che è contraddittorio. 
All'inizio non vedevo la contraddizione, perchè mi concentravo sul punto sbagliato.

Avevamo detto che $\alpha$ appartiene solo a $L$, ma se $U\cdot V^\omega$ è un sottoinsieme di $L$, allora l'intersezione con $\bar{L}$  sarà sempre vuota ($U\cdot V^\omega \cap \bar{L} = \emptyset$), ma avevamo detto che $U\cdot V^\omega \cap \bar{L} \neq \emptyset$. Contraddizione.

Perciò se $\sim$ satura $L$ satura anche $\bar{L}$

$\square$

-----------------------
***NOTAZIONE***: 
###### Dato un automa di Büchi $A = (Q, A, \Delta, q_{0}, F)$, $w \in A^*$, $s,s' \in Q$ indichiamo con $s \to_{w}^F s'$ l'esistenza di una computazione su w che porta da uno stato s a uno stato s' attraversando uno stato finale.
###### Indichiamo con $W_{ss'}^F$ l'insieme delle parole $w \in A^*$ tali per cui esiste una computazione $s\to_{w}^F s'$
--------------

> #### RELAZIONE $\approx_{\cal{A}}$
>La definizione è piuttosto semplice:
prendiamo un automa di Büchi $A = (Q, A, q_{0}, \Delta, F)$. Se due parole $u$ e $v$ sono in relazione $\approx_{\cal{A}}$ significa che per ogni $s, s' \in Q$, abbiamo che 
>1. $s\to_{u}s'$ sse $s\to_{v} s'$ 
>2. $s \to_{u}^F s'$ sse $s \to_{v}^F s'$ 

Ora sarebbe piacevole verificare se su un automa di Büchi la relazione $\approx_{\cal{A}}$ è una relazione di congruenza di indice finito che satura $L(\cal{A})$ .

**Dimostrazione:**

Avendo a che fare con un automa di Büchi sappiamo che il numero di stati è finito, quindi anche l'indice delle classi dev'essere finito.
Inoltre la definizione di congruenza ci dice che $\approx_{\cal{A}}$ è una relazione di congruenza.

Ci manca da verificare se satura $\mathcal{L}(\cal{A})$.

> $\approx_\mathcal{A}$ satura il linguaggio $\mathcal{\mathcal{L}_\mathcal{A}}$

**Dimostrazione**

Consideriamo una parola $\alpha \in U\cdot V^\omega \cap \mathcal{L}(\cal{A})$. 

Essendo $\alpha \in L(\cal{A})$ sappiamo che esiste una computazione che termina (altrimenti non finiremmo in $\mathcal{A}$). Perciò $\alpha$ genera una computazione del tipo $q_0, s_1, s_2, s_3, ...$ in cui si passa infinite volte in uno stato finale dell'automa $\mathcal{A}$.



Però sappiamo anche che $\alpha \in U\cap V^\omega$, perciò possiamo vedere $\alpha$ come $\alpha = u \in U \cdot v_{1} \in V \cdot v_{2} \in V  \cdot v_{3} \in V  \cdot \dots$

Uniamo le due cose, e vediamo $\alpha$ come una computazione del tipo

$$
\alpha = q_0 \to _u s_1 \to _{v_1} s_2 \to _{v_2} s_3 \to \dots
$$

in cui siamo sicuri che abbiamo infiniti $i$ per cui vale $s_i \to _{v_i} s_{i+1}$.

Questo vale per ogni $u \in U$ e per ogni $v \in V$ chiaramente, perchè U e V sono due classi di equivalenza.

Beneee. 

Ora consideriamo una parola $\beta \in \mathcal{L}(\mathcal{A})$ e cerchiamo di dimostrare che qualsiasi parola consideriamo, se $\beta \in U\cdot V^\omega $ allora $\beta \in \mathcal{L}(\mathcal{A})$ e quindi $U\cdot V^\omega \subseteq \mathcal{L}(\mathcal{A})$.

Riscriviamo $\beta$ come 

$$
\beta = u'v_1'v_2'\dots
$$

Poichè $u\approx_\mathcal{A} u'$ e $v\approx_\mathcal{A} v'$ (sempre per il fatto che $u$ e $u'$ sono dentro la stessa classe di equivalenza $U$) allora le stesse cose che valevano prima valgono anche ora, ossia che $\beta$ genera una computazione di $\mathcal{A}$ del tipo

$$
q_0 \to _{u'} s_1 \to _{v_1'} s_2 \to _{v_2'} s_3 \to \dots
$$

ma allora è evidente che per un numero infinito di $i$ abbiamo computazioni che portano a uno stato finale, ossia transizioni del tipo 

$$
s_i \to _{v_i'}^F s_{i+1}
$$

Perciò qualsiasi parola $\beta$ viene accettata da $\mathcal{A}$ per definizione di accettazione. 
Abbiamo dimostrato che se $U \cdot V^\omega \cap \mathcal{L}(\mathcal{A})\ne \emptyset$ allora $U \cdot V^\omega \subseteq \mathcal{L}(\mathcal{A})$.

$\square$

#### Lemma
**Sia $\sim$ una congruenza su $A^\omega$ di indice finito. Allora considerata una qualsiasi parola $\alpha \in A^\omega$ è possibile trovare due classi $U$ e $V$ tali per cui $\alpha  \in U \cdot V^\omega$  e $V\cdot V \subseteq V$**

Sembra strano a primo impatto. Che vuol dire che $V\cdot V \subseteq V$?

Significa che la forma di $\alpha$ è ripetitiva nella parte di V, perciò possiamo concatenare elementi di V avendo la certezza che continuino a rimanere dentro V. 

🎯 Esempio per visualizzare la situazione. 

Se ho ad esempio $ A=\{ a,b \} $ e una classe $V=\{x \mid x \text{ inizia e finisce per } a\}$ posso osservare come $V$ contiene un insieme infinito di elementi, ma possiamo renderci conto che concatenando qualsiasi elemento di $V$ con qualsiasi altro elemento di $V$ otteniamo una parola che rimane dentro a $V$.


Il lemma ci dice che, dato che $\sim$ ha indice finito:

1. Ogni parola infinita può essere vista come:
    - Un "prefisso iniziale" appartenente a una classe finita U,
    - Seguito da una parte infinita generata ripetendo blocchi che appartengono a un'altra classe finita V.
2. Questo riflette la struttura ricorrente e regolare delle parole in $A^\omega$ rispetto a congruenze finite.
#### Corollario:
Se $L\subseteq A^\omega$ e $\sim$ è una relazione di congruenza di indice finito che satura $L$ allora $L = \bigcup U\cdot V^\omega$ con $U,V \sim$-classi tali che $U\cdot V^\omega \cap L \neq \emptyset$.



### Teorema di Büchi sulla chiusura rispetto alla complementazione (IMPORTANTE)
Se $L\subseteq A^\omega$ è $\omega$-regolare, allora $\bar{L}$ è $\omega$-regolare.
Inoltre se ho un automa di Büchi che riconosce L, sono in grado di costruirne uno che riconosce il suo complementare $\bar{L}$.

**Dimostrazione:**
*"se $L\subseteq A^\omega$ è $\omega$-regolare, allora $\bar{L}$ è $\omega$-regolare"*
Assumiamo $L\subseteq A^\omega$ linguaggio $\omega$-regolare.
Sappiamo inoltre che $\approx_{\cal{A}}$ è una relazione di congruenza di indice finito e invariante a destra che satura L, perciò satura anche $\bar{L}$, dato che avevamo detto che "Siano $\sim$ una congruenza e $L \subseteq A^\omega$. Se $\sim$ satura L allora $\sim$ satura pure $\bar{L}$". 

Ci ricordiamo a questo punto che il teorema di Myhill-Nerode diceva che se ho un linguaggio esprimibile come l'insieme di classi di equivalenza indotte da una relazione invariante a destra, L è regolare.

Ebbene dal corollario precedente abbiamo visto che L può essere espresso come l'unione 
$$
L = \bigcup U \cdot V^\omega
$$
dove U e V sono proprio delle classi di equivalenza indotte da una relazione invariante a destra. 
Di conseguenza L è regolare e possiamo usare il teorema sulle chiusure dei linguaggi $\omega$-regolari per costruire un automa per $L$ e di conseguenza anche un automa di Büchi per il complementare $\bar{L}$.

In alternativa possiamo il lemma che diceva "se $\sim$ è una relazione di congruenza che satura $\mathcal{L}$ allora satura anche $\overline{\mathcal{L}}$ e applicare lo stesso ragionamento alla classe complementare. 
$\square$


*"se ho un automa di Büchi che riconosce L, posso costruirne uno che riconosce $\bar{L}$."*

---
##### 👁️ ATTENZIONE

Non è sufficiente invertire gli stati per far sì che l'automa di Büchi riconosca il suo complementare. 

---

# Automi di Büchi (deterministici)
L'unica roba che cambia rispetto agli automi di Büchi non deterministici è la funzione di transizione, che prima era 
$$
\Delta \subseteq Q×A×Q
$$
e ora diventa
$$
\delta: Q×A \to Q
$$
C'è un'altra cosa piuttosto importante che distingue gli automi di Buchi non deterministici da quelli deterministici: <u>i primi sono chiusi rispetto alla <b><i>complementazione</i></b>, i secondi NO</u>. Vedremo la dimostrazione di questa cosa dopo la definizione di caratterizzazione dei linguaggi riconosciuti da automi di Büchi deterministici.

## Proprietà di chiusura
> **Gli  automi di Büchi deterministici godono di 3 chiusure fondamentali:**
>- **unione**
>- **intersezione**

Le dimostrazioni sono piuttosto semplici:
- nel primo caso è sufficiente osservare come l'unione di due linguaggi riconosciuti da due distinti automi di Büchi sia possibile mediante la costruzione di un nuovo automa di Büchi in cui l'insieme degli stati finali è costruito come 

$$
F = \{(f_1, q_2) \mid f_1 \in F_1 \land q_2 \in Q_2\} \cup \{(q_1, f_2) \mid q_1 \in Q_1 \land f_2 \in F_2\}
$$

- nel secondo caso l'insieme degli stati finali è invece composto come

$$
F = \{(f_1,f_2) \mid f_1 \in F_1 \land f_2 \in F_2\}
$$


## Caratterizzazione dei linguaggi riconosciuti dagli Automi di Büchi deterministici
Definiamo a partire dall'insieme $W\subseteq A^*$:
- $W^\omega = \{\alpha \in A^\omega \mid \alpha =  \alpha_{0} \alpha_{1} \dots\alpha_{i} \in W\}$ $\omega$-chiusura di W
- $\overrightarrow{W} = \{\alpha \in W^\omega \mid \exists^\omega n \; \alpha (0,n) \in W \}$ chiusura vettoriale di W
- $In(\alpha) = \{a \in A\ \mid \exists^\omega n \; \alpha(n) = a\}$ infinità di $\alpha$


>**Diciamo che un linguaggio $L \subseteq A^\omega$ è riconosciuto da un automa di Büchi deterministico se e solo se $L = \overrightarrow{W}$ per qualche $W\subseteq A^{*}$**.

**Dimostrazione**


$(\rightarrow)$ se un linguaggio $L \subseteq A^\omega$ è riconosciuto da un automa di Büchi deterministico allora $L = \overrightarrow{W}$ per qualche $W \subseteq A^*$

Consideriamo un linguaggio $L \subseteq A^\omega$ e un automa di Buchi $\cal{A}$ deterministico che lo riconosce. Consideriamo anche un DFA $\mathcal{A}'$ con struttura uguale ad $\cal{A}$. 
Per definizione di accettazione, per ogni parola $\alpha \in L$ abbiamo che $In(\alpha) \cap F \neq \emptyset$. Ovverosia, per ogni parola si passa infinite volte per uno stato finale di $\cal{A}$ e di conseguenza  esistono infiniti prefissi $w$ di $\alpha$ che passano per uno stato finale. Essendo $\cal{A}'$ un DFA strutturalmente uguale ad $\cal{A}$ abbiamo che $w \in \cal{A}'$ e quindi $L \subseteq \overrightarrow{W}$ per definizione. 

$(\leftarrow)$ se un linguaggio $L\subseteq A^\omega$ è tale per cui $L = \overrightarrow{W}$ per qualche $W \subseteq A^*$ allora L è riconosciuto da un automa di Büchi deterministico.

Consideriamo una parola $\alpha \in \overrightarrow{W}$. Questo significa che esistono infiniti prefissi di $\alpha$ tali che $w \in W \subseteq A^*$. Ma allora $W = L(\cal{A}')$ e una computazione $\sigma$ di $\cal{A}'$ può essere vista come un prefisso della computazione di $\cal{A}$. Perciò $\cal{A} \cap F \neq \emptyset$.


## Non-chiusura per complementazione
Come ho detto all'inizio, <u>gli automi di Büchi deterministici non godono della proprietà di chiusura per complementazione</u>.

Vediamo ora la dimostrazione.

**Dimostrazione:**

L'idea è quella di mostrare un linguaggio $\omega$-regolare che non viene accettato da un automa di Büchi deterministico, per poi mostrare che il complementare dello stesso linguaggio invece viene accettato. Se l'automa di Büchi deterministico fosse chiuso per complementazione avremmo che entrambi i linguaggi possono essere riconosciuti da un automa di Büchi deterministico. 

Ricordiamoci che affinchè un linguaggio $\mathcal{L}$ sia accettato da un automa di Büchi deterministico, $\mathcal{L} \subseteq \overrightarrow{W}$ per quale $W\subseteq A^*$

Consideriamo il linguaggio 

$$
\mathcal{L} = \{\alpha \in A^\omega | \exists^{<\omega}n\; \alpha(n)=a \}
$$

La simbologia $\exists^{<\omega}$ sta a significare che esiste un numero finito di $n$.

Ricordiamo che $\overrightarrow{W}$ è definito come

$$
\overrightarrow{W} = \{\alpha \in A^\omega \mid \exists^\omega n\;\alpha(0,n) \in W\}
$$

$\mathcal{L}$ non può essere scritto come $\overrightarrow{W}$ e ora dimostreremo per assurdo che è così.

1. Assumiamo $L = b^\omega$
2.  allora esiste un valore $n_{1}$ tale per  cui $b^{n_{1}} \in W$.
3. Considerato $b^{n_{1}}ab^{\omega} \in L$ anche in questo caso dovrà esistere $n_{2}$ tale per  cui $b^{n_{1}}ab^{n_{2}} \in W$.
4. Posso andare avanti all'infinito e finirò sempre per ottenere che esiste un $n$ finito. A quel punto però esistono un'infinità di prefissi che appartengono a $W$ e quindi non è possibile che un qualsiasi simbolo sia considerato una sola volta a meno che l'alfabeto non sia infinito, che non è possibile.

D'altra parte però il complementare di $\mathcal{L}$ è il linguaggio composto da stringhe k 
In conclusione possiamo dire che $DBA \subset NDBA$

$\square$

# Automi di Muller
Gli automi di Muller sono una variante degli automi di Buchi. La cosa particolare che cambia sta nella definizione dell'insieme degli stati finali.

La definizione di automa di Buchi era data da una tupla costituita come segue

$$
\mathcal{A} = <Q,A, q_{0}, \delta, F> 
$$
con 
- $\delta:Q\times A \to Q$ 
- $F \subseteq Q$
La condizione di accettazione era che $In(\alpha) \cap F \neq \emptyset$.

Quindi l'automa di Buchi accettava qualcosa se veniva raggiunto infinite volte uno stato appartenente all'insieme F.

Nel caso degli automi di Muller invece, l'insieme F non è più un insieme di stati, bensì un insieme di insiemi di stati. 
La definizione rigorosa è che un automa di Muller è definito da una tupla 

$$
\mathcal{A} = <Q,A,q_{0}, \delta, \mathcal{F}> 
$$
dove:
- $\mathcal{F} = 2^Q$ ossia un insieme composto da sottoinsiemi dell'insieme $Q$
- $\delta: Q\times A \to Q$ 

È facile osservare che a partire da un automa di Buchi ci si può ricondurre a un automa di Muller semplicemente imponendo che $\mathcal{F} = \{P \in 2^Q | P\cap F \neq \emptyset\}$
ovverosia che ogni stato finale dell'automa di Buchi di partenza sia presente in ciascuno dei sottoinsiemi che compongono l'insieme $\mathcal{F}$ del Muller automata.

#### Chiusura rispetto alla complementazione
Rispetto agli automi di Buchi deterministici, i Muller automata deterministici godono della proprietà di chiusura rispetto alla complementazione. 
La dimostrazione è banale, è sufficiente considerare un linguaggio $L\subseteq A^\omega$ e un Muller automata $\mathcal{A}= <Q,A, q_{0}, \delta, \mathcal{F}>$ che lo riconosce.  A questo punto se costruissimo un Muller automata $\mathcal{A}' = <Q,A,q_{0},\delta, \mathcal{F}'>$ dove l'insieme $\mathcal{F}$ è definito come $\mathcal{F}' = 2^Q\setminus \mathcal{F}$ , otterremmo proprio un automa di Muller che accetta il complemento del linguaggio $L$.


# Automi di Rabin

# Possibili domande

Esercizio 
Dato un automa di Büchi $A = (Q, A, \Delta, q_{0}, F )$, dimostrare che, per ogni $s, s'\in Q, W_{ss}^F′$ è regolare.

Esercizio 
Dimostrare che la relazione $\approx_{\cal{A}}$ è una congruenza di indice finito.

Esercizio 2.3 
Sia A l’automa dell’Esempio

"Consideriamo il linguaggio L che contiene tutte e sole le ω-parole su A = {a, b, c} tali che tra ogni coppia di occorrenze “consecutive” di a (ossia, tali che tra di esse non vi è alcuna altra occorrenza di a) esiste un numero pari di occorrenze di simboli diversi da a (b e/o c). Il linguaggio L è e ω-regolare in quanto è riconosciuto dall’automa di B¨uchi A = ({q0, q1, q2}, A, q0, ∆, {q0, q1, q2}), la cui relazione di transizione ∆ è descritta in Figura 2.1. Si osservi come tutti e tre gli stati dell’automa siano stati finali. In particolare, si è scelto di considerare finale anche lo stato più a destra in Figura 2.1. Si osservi anche come non sia possibile accertare in un numero finito di passi se una data ω-parola α ∈ L, mentre è sufficiente un numero finito di passi per stabilire se una data ω-parola α 6 ∈ L. ""


Si consideri l’automa A′ ottenuto da A rimuovendo lo stato q0, e le transizioni in esso entranti e da esso uscenti, e facendo diventare q1 il nuovo stato iniziale. Si stabilisca se A e A′ riconoscono o meno lo stesso linguaggio. 

Esercizio 2.4
Si costruisca l’automa A′ che riconosce la variante finita (linguaggio di parole finite) dell’esempio precedente. 


Esercizio 2.5
Sia W il linguaggio riconosciuto dall’automa A′ dell’Esercizio 2.4. Si caratterizzi il linguaggio $\top{W}$ 

Esercizio 2.44 
Dimostrare la chiusura della classe dei linguaggi riconosciuti dagli automi di B¨uchi deterministici rispetto alle operazioni di unione e intersezione

Esercizio 2.46 
Sia A = {a, b} e L = {α ∈ Aω . ∃<ω n α(n) = a}. Si costruisca un automa di B¨uchi non deterministico che riconosca il linguaggio L

Esercizio 2.48 
Sia A = {a, b} e L = −−−→ (b∗a)∗. Si costruisca un automa di B¨uchi deterministico che riconosca il linguaggio L.

Esercizio 2.50 
Dimostrare che la classe degli ω-linguaggi ω-regolari coincide con la classe degli ω- linguaggi riconosciuti dagli automi di Muller non deterministici.