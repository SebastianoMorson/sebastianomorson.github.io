---
layout: post
title: Relazione tra Classi di Complessità 4 - Reachability Method
excerpt_separator: <!--more-->
truncated_preview: true
tags:
  - miscellaneous
categories: article
---
<!--more-->
Dallo Hierarchy Theorem siamo riusciti a dimostrare alcune relazioni interessanti tra le classi di complessità che ora elencherò e che sappiamo valere nel caso $f$ sia propria:

$$
\begin{aligned}
&1.\;\; TIME(f(n)) \subseteq NTIME(f(n))\\ \\
&2 \;\; SPACE(f(n)) \subseteq NSPACE(f(n)) \\ \\
&3 \;\; TIME(f(n)) \subseteq SPACE(f(n)) \\ \\
&4 \;\; SPACE(f(n)) \subseteq TIME(c^{f(n)+\log(n)}) \\ \\
&5 \;\; NTIME(f(n)) \subseteq SPACE(f(n)) \\ \\
&6 \;\; NSPACE(f(n)) \subseteq TIME(c^{f(n)+\log(n)}) 
\end{aligned}
$$

Analizziamo una per una queste relazioni:

## 1. $TIME(f(n)) \subseteq NTIME(f(n))$
C'è poco da dire, è abbastanza evidente che un problema deterministico è risolvibile mediante un programma di una macchina non deterministica. Semplicemente si perde la capacità di svolgere le operazioni contemporaneamente.

## 2. $SPACE(f(n)) \subseteq NSPACE(f(n))$
Anche in questo caso è evidente che non ci siano ragioni per le quali la simulazione di una macchina deterministica su una non deterministica comporti un aumento dello spazio. D'altronde il determinismo non è che una variante del non determinismo in cui il numero di path percorribili è unico.

## 3. $TIME(f(n)) \subseteq SPACE(f(n))$
Questa considerazione era stata data per scontata. È piuttosto ovvio che se ho una MdT che esegue al più f(n) operazioni non potrà eseguirle su un numero di celle superiore a f(n). Se lo facesse vorrebbe dire che le operazioni che esegue non sono atomiche, ma sono combinazioni di più operazioni. 
In una MdT le operazioni possibili vengono eseguite su celle singole e possono essere solo di scrittura e lettura o spostamento, quindi non sono previste operazioni combinate. 

## 4. $SPACE(f(n)) \subseteq TIME(c^{f(n)+\log n})$
Questo è dimostrato nella lezione 4. 
L'idea è quella di mostrare come sia necessario copiare tutte le configurazioni della macchina non deterministica sul working tape della macchina deterministica. A quel punto è facile osservare come il linguaggio viene deciso dalla macchina non deterministica seguendo ogni possibile diramazione dell'albero delle scelte non deterministiche. Se ho un grado di non determinismo $d$ e ho $n$ variabili, allora otterrò un albero composto da $d^{n}$ possibili percorsi radice-foglia. Perciò simulando la macchina non deterministica dovrò eseguire tutti i percorsi senza godere però della possibilità di esplorarli contemporaneamente. Quindi tramite backtracking esplorerò tutti i path (che sono $O(d^{f(n)})$.

## 5. $SPACE(f(n)) \subseteq SPACE(f(n))$
Se ho $\mathcal{L} \in NTIMES(f(n))$ allora esiste una macchina di Turing a k nastri $\mathcal{N}$ che decide il linguaggio in al più f(n) step. Essendo non deterministica, significa che la computazione di uno dei path non deterministici è computabile in al più O(f(n)) steps. Cerchiamo quindi di simulare la macchina $\mathcal{N}$ con una macchina deterministica $\mathcal{M}$. Possiamo a questo punto tenere traccia su un tape delle nostre scelte non deterministiche e quando arriviamo a una foglia "no" eseguiamo backtracking. In questo modo lo spazio usato è sempre quello che useremmo per verificare se un singolo path porta a una foglia "yes" o a una foglia "no". Il problema di stabilire se un path è "no" o "yes" è un problema computabile in f(n) step e quindi lo spazio richiesto è al massimo f(n). Da qui $NTIME(f(n)) \subseteq SPACE(f(n))$.

## 6. $NSPACE(f(n)) \subseteq TIME(c^{f(n)+\log n})$
Se un linguaggio $\mathcal{L}$ appartiene a $NSPACE(f(n))$ significa che esiste una macchina di Turing non deterministica con I/O $\mathcal{N}$ che decide $\mathcal{L}$ usando spazio al più $f(n)$ .

Consideriamo quindi questa macchina di Turing e cerchiamo di simularla su una macchina deterministica.

Ricordiamo che le configurazioni generiche di una MdT sono del tipo 

$$
\begin{aligned}
&(1,\triangleright, x) \text{ per la configurazione iniziale nel caso deterministico}\\
&(H,w,u) \text{ per la configurazione generica nel caso deterministico} \\
&(1,\triangleright,x,w_{1},u_{1},\dots,w_{k}, u_{k}) \text{ nel caso iniziale di una Mdt non deterministica con I/O} \\
&(H,\triangleright,x,w_{1},u_{1},\dots,w_{k}, u_{k}) \text{ nel caso generico di una Mdt non deterministica con I/O} 
\end{aligned}
$$

Perciò le configurazioni di $\mathcal{N}$ sono del tipo

$$
(q, w_{1},u_{1},\dots,w_{k}, u_{k,})
$$

Il contenuto dei nastri di input e di output (rispettivamente $(w_1, u_1)$ e $(w_{k},u_{k})$) può essere considerato irrilevante al fine della dimostrazione, quindi consideriamo semplicemente le configurazioni senza il nastro di input e di output. 

$$
(q,j, w_{2},u_{2},\dots,w_{k-1}, u_{k-1})
$$

dove:
- $q$ indica lo stato della configurazione
- j indica la posizione della testina sul nastro di input

Ci chiediamo ora quante sono le possibili configurazioni che si potrebbero presentare nella macchina $\mathcal{N}$:
- $q$ può assumere $\mid K \mid+2$ stati ($\{q_0,q_1,...,q_{\mid K \mid-1}, \text{yes, no}\}$)
- j può assumere $\mid x \mid +2$ posizioni sul nastro di input (le posizioni in x, più il simbolo blank e il simbolo iniziale)
- di nastri di lavoro possiamo averne $k$ (ATTENZIONE, k minuscolo è il numero di nastri, K grande è l'insieme degli stati di una MdT) e ciascun $w$ e $u$ può essere lungo $f(n)$, quindi le stringhe contenuto in esso sono $\Sigma^{f(n)}_{\mathcal{N}}$

Perciò il numero totale di configurazioni è:

$$
NumConf = (\mid K \mid +2)\cdot(\mid x \mid +2)\cdot \prod_{k=2}^{k-1} \Sigma^{f(n)}_{\mathcal{N}}\cdot\Sigma^{f(n)}_{\mathcal{N}}
$$

Perciò il numero totale è dato da

$$
\begin{aligned}
&NumConf = (\mid K \mid +2)\cdot(\mid x \mid +2)\cdot \prod_{k=2}^{k-1} \Sigma^{f(n)}_{\mathcal{N}}\cdot\Sigma^{f(n)}_{\mathcal{N}} \\ \\
&NumConf = (\mid K \mid +2)\cdot(\mid x \mid +2)\cdot \Sigma^{f(n)\cdot 2\cdot (k-2)}_{\mathcal{N}} \\ \\
&NumConf \ge \Sigma_{\mathcal{N}}^{\log_{\Sigma_{\mathcal{N}}}(\mid x \mid +2)}\cdot \Sigma^{f(n)\cdot 2\cdot (k-2)}_{\mathcal{N}} \\ \\
&NumConf \ge \Sigma_{\mathcal{N}}^{\log_{\Sigma_{\mathcal{N}}}(\mid x \mid +2) +f(n)\cdot 2\cdot (k-2)} \\ \\
&NumConf \ge c^{\log_{\Sigma_{\mathcal{N}}}(\mid x \mid) +f(n)} \\
\end{aligned}
$$

Da qui è quindi sufficiente costruire il grafo delle computazioni $G_{\mathcal{N}}(x)$ con un costo lineare rispetto al numero di nodi del grafo.
A quel punto posso applicare reachability sul grafo $G_{\mathcal{N}}(x)$ che è un'operazione lineare rispetto alla dimensione del grafo se uso uno pseudocodice, altrimenti è polinomiale nel caso di una MdT deterministica, quindi impiego $O((c^{\log_{\Sigma_{\mathcal{N}}}(\mid x \mid) +f(n)})^b)$.
Perciò posso concludere che 

$$
NSPACE(f(n)) \subseteq TIME(c^{f(n)+\log n})
$$

$\square$
