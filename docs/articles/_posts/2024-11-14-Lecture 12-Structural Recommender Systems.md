---
layout: post
title: Structural Recommender Systems
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
---

Quando ho letto per la prima volta "structural recommender systems" ho subito pensato "cavolo, non ho mai sentito parlare di modelli di AI strutturali!". Spoiler: si parla di graph neural networks.
<!--more-->

I sistemi di raccomandazione strutturali sono quei sistemi che sfruttano la struttura di un grafo per effettuare delle previsioni. Se non riuscite a immaginare dove vengono impiegate, basta che pensiate i social networks. Su Instagram non vi siete mai chiesti perchè vi vengono suggerite certe persone che potreste conoscere? Secondo voi sono suggerite in base ai gusti degli utenti? Ma allora come si spiega che vi sia stata suggerita un vostro/a ex quando non avevamo neanche un gusto in comune con questa persona (sennò non sareste ex)? Semplice: perchè alcuni sistemi di raccomandazione costruiscono una sorta di albero genealogico di amicizie e poi cercano di capire se due persone dell'albero potrebbero andare d'accordo tra loro.

L'idea di questa lezione è quindi questa: trovare un modo di rappresentare dei legami tra items/utenti e poi imparare dalla struttura che si viene a formare.

Ed è per questo che le Graph Neural Networks fanno al caso nostro, ma prima di parlarne bisogna fare alcune premesse.
## Graphs
Andrò piuttosto veloce su alcune nozioni, come questa, perchè dò per scontato che chi legge questi articoli abbia studiato almeno una volta cosa sono i grafi, ma in caso in futuro ci farò un articolo bello approfondito che linkerò a questo qua. Se non c'è alcun link sulla parola "grafi" significa che me ne sono dimenticato.

Un grafo è questa roba qua:
![[Pasted image 20241128164153.png]]
ossia una struttura dati composta da
- nodi che possono contenere delle informazioni
- archi
    - orientati o non orientati
    - con una label o senza una label

That's it.

Possiamo avere tanti nodi o pochi nodi, ma sicuramente se c'è un arco deve esserci almeno un nodo. Questa regola non vale anche al contrario, possiamo avere tantissimi nodi, ma nessun arco che li connette.

Quando un grafo presenta dei nodi con delle label e da un nodo possono partire più archi con label diverse, parliamo di multi-relational graphs. Un esempio di grafo multi-relazionale potrebbe essere quello che rappresenta la nostra famiglia. Se ogni componente della famiglia è un nodo, tu avrai un arco "è figlio di" che parte da te e si dirige verso i tuoi genitori. Allo stesso tempo se hai fratelli potresti avere un arco "è fratello di" che parte da te e si dirige verso tuo fratello (e viceversa). Questo è un grafo multi-relazionale.

Ora possiamo già iniziare a parlare di alcuni modi per modellare le caratteristiche di grafo.

Consideriamo l'esempio di un social network. Ogni nodo è un utente e abbiamo utenti più o meno famosi, dove gli utenti hanno certi gusti musicali. Abbiamo gli utenti "rockettari" e quelli "metallari" e quelli "neomelodici". Tra loro ci saranno degli archi che li uniscono se hanno dei gusti simili.
## Informazioni sulle relazioni tra nodi

Ora presento alcuni metodi per estrarre informazioni da un grafo. In particolare questi metodi studiano le relazioni tra coppie di nodi

### Graph overlap detection

Il primo modo per estrarre informazioni è questo qua.

L'idea è quella di confrontare il vicinato di due nodi, per capire se ha senso creare un link tra questi due nodi.

Mettiamo che la situazione sia questa

![](https://static.wixstatic.com/media/4959fd_952d49ce561b402f99d4d3333789262b~mv2.png/v1/fill/w_430,h_286,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_952d49ce561b402f99d4d3333789262b~mv2.png)

con N(u) definisco l'insieme dei nodi che appartengono al vicinato del nodo u.

Con "probabilità di link" intendiamo la probabilità che abbia senso creare un link tra due nodi. Allora la probabilità di link potrebbe essere calcolata come:

![](https://static.wixstatic.com/media/4959fd_bd2ced9797014c89b6cc88b01a6b38a1~mv2.png/v1/fill/w_299,h_62,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_bd2ced9797014c89b6cc88b01a6b38a1~mv2.png)
### Jaccard Measure
Il problema è che due nodi potrebbero avere tanti nodi in comune semplicemente perchè i due nodi sono molto popolari. Se pensiamo al mio amore platonico Rachel McAdams e al mio secondo amore platonico Jennifer Lawrence, sicuramente hanno tantissimi utenti che seguono entrambe, ma i loro gusti/conoscenze reali potrebbero essere estremamente diverse.

Bisogna tenerne in considerazione e lo fa bene la measure di Jaccard

![](https://static.wixstatic.com/media/4959fd_ba743690ac9b482a8d8c4e078c04ad52~mv2.png/v1/fill/w_322,h_80,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_ba743690ac9b482a8d8c4e078c04ad52~mv2.png)
### Adamic-Adar Meausure
C'è un altro problema.
Su instagram seguo Ryan Reynolds perchè è una leggenda, ma sono uno dei suoi 59 milioni di follower. Anche la mia ex probabilmente seguiva Ryan Reynolds (chi è che non lo segue?) però ciò nonostante avevamo dei gusti super diversi e sarebbe stato meglio che non ci fosse stato il link tra noi. Ci serve un modo per controllare se il nostro vicinato è famoso e in tal caso depotenziare il suo contributo nel calcolo della "probabilità di link".

Il modo c'è ed è questo qua

![](https://static.wixstatic.com/media/4959fd_a47962decfe547d5884055db8800e07a~mv2.png/v1/fill/w_322,h_58,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a47962decfe547d5884055db8800e07a~mv2.png)

  

### Katz index

L'overlap va piuttosto bene finchè abbiamo dei grafi di questo tipo

![](https://static.wixstatic.com/media/4959fd_8aedf84051bd435fb280b3e8c427cfca~mv2.png/v1/fill/w_280,h_186,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_8aedf84051bd435fb280b3e8c427cfca~mv2.png)

ma che succede se ho dei grafi come questo?

![](https://static.wixstatic.com/media/4959fd_b99151db5839480c97d373b7c4cfb2d0~mv2.png/v1/fill/w_280,h_247,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_b99151db5839480c97d373b7c4cfb2d0~mv2.png)

L'overlap tra i due nodi A e B è praticamente nullo, eppure i due nodi appartengono a un cluster globalmente molto informativo.

L'obiettivo del Katz index è proprio scavalcare il problema dell'overlap, misurando, invece della dimensione del vicinato comune, il numero di paths che a partire da un nodo A raggiungono il nodo B.

La formula è la seguente

![](https://static.wixstatic.com/media/4959fd_1c470c04add44b7fb3184523b53bcd14~mv2.png/v1/fill/w_352,h_102,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1c470c04add44b7fb3184523b53bcd14~mv2.png)

Più è alto il numero di paths, più i due nodi hanno una probabilità di link maggiore.

### Page rank

Il page rank è un metodo che ha come obiettivo capire se un nodo è importante o no. Il nodo potrebbe essere una pagina web, un utente, un documento, etc.

Per attribuire l'importanza a questo elemento PageRank usa un "surfer" che salta da una pagina a un'altra.

Immaginiamo di avere un grafo come questo  

![](https://static.wixstatic.com/media/4959fd_addd1f75e2c24c20a744e1a50beb038a~mv2.png/v1/fill/w_280,h_219,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_addd1f75e2c24c20a744e1a50beb038a~mv2.png)
Può essere rappresentato dalla matrice di transizione

|     | A   | B   | C   | D   | E   | F   | G   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| A   | 0   | 0   | 1   | 0   | 0   | 0   | 0   |
| B   | 1/3 | 0   | 0   | 0   | 1/3 | 0   | 1/3 |
| C   | 0   | 0   | 0   | 1/2 | 0   | 1/2 | 0   |
| D   | 1   | 0   | 0   | 0   | 0   | 0   | 0   |
| E   | 0   | 0   | 1/2 | 0   | 0   | 1/2 | 0   |
| F   | 0   | 0   | 0   | 0   | 0   | 0   | 1   |
| G   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

  

All'inizio quindi la probabilità che il surfer si trovi a visitare uno dei nodi è data da **π** = [1/n, ..1/n].

Aggiorniamo quindi la distribuzione di probabilità **π** moltiplicando **π** per la matrice di transizione. A poco a la distribuzione di probabilità tenderà a convergere.
#### I soliti problemi
Come al solito non va mai tutto bene all'inizio. Anche in questo caso con l'approccio descritto incappiamo in alcuni problemi:
- dead-end: nel caso il surfer finisca su un nodo che non ha nessun arco che punta a un'altra pagina, l'esplorazione terminerebbe.
- spider-trap: il surfer potrebbe finire ingabbiato in un loop dato da due nodi i e j in una relazione del tipo i->j j->i
Questo non vogliamo che accada, perciò usiamo un parametro di teletrasporto ẞ che fa saltare il surfer in un nodo casuale.
Aggiorniamo quindi la formula per il calcolo della transition matrix

Il punto cruciale è che alla fine la distribuzione delle probabilità di transizione rispecchierà l'importanza della pagina stessa.
## Node Embedding

Fin'ora abbiamo cercato di catturare le relazioni tra nodi a partire da una matrice di adiacenza (neighborhood methods) o una matrice di transizione (PageRank), ma perchè non trovare una rappresentazione del grafo più informativa? Magari dove nodi simili sono mappati in un latent space come punti vicini nello spazio?
### Encoder-decoder approach

Per creare un embedding dei nodi del grafo, un approccio è quello di usare un'architettura encoder-decoder. Essenzialmente in questa architettura le features di un nodo vengono date in pasto a un encoder che ne genera una rappresentazione su uno spazio continuo e di dimensione inferiore rispetto a quello di partenza. Banalmente, l'encoder potrebbe essere una embedding lookup table che usa l'indice del nodo per estrarne la rappresentazione. Il decoder è in grado a partire dall'embedding generato di ricostruire il grafo di partenza.

  

### Shallow embeddings

Un modello encoder-decoder semplice genera embeddings shallow, ossia rappresentazioni semplici che si basano unicamente sui dati osservati in fase di training, ma non consentono di derivare rappresentazioni per nuovi items.

### Graph Neural Networks (GNN)

Il problema principale degli shallow embeddings è che permettono di generare una rappresentazione per nodi che sono visti in fase di training, ma non per nodi che non lo sono. Ad esempio se ho un grafo e voglio generare una rappresentazione per i suoi nodi, posso addestrare un Encoder-Decoder, ma se alla stesso grafo aggiungo un nuovo nodo, dovrò addestrare nuovamente il modello.

Questo non è ottimale quando ho a che fare ad esempio con i social networks in cui quotidianamente ho dei nuovi utenti.

  

In questo senso, le GNN sono la soluzione migliore.

Le GNN servono per creare degli embeddings dei nodi del grafo che permettano di eseguire previsioni. Gli embeddings saranno quindi versioni condensate e molto informative dei nodi, che considerano la morfologia e le features del grafo.

  

Le GNN si basano su un meccanismo chiamato **message passing** che aggiorna la conoscenza di ciascun nodo del grafo.

  

#### Message Passing

Il message passing si basa su due funzioni fondamentali

- AGGREGATE è la funzione che aggrega le informazioni dei nodi nel vicinato di un nodo u. Bisogna pensare questa funzione come una maximum o una minimum, o una weighted sum, ...
    
- UPDATE è la funzione che aggiorna le informazioni del nodo considerando la rappresentazione aggregata dei nodi nel vicinato
    

  

Il risultato del message passing viene svolto su tutti i nodi del grafo attraverso il calcolo della seguente funzione

![](https://static.wixstatic.com/media/4959fd_e6396755229642a5b2b8aff399d27746~mv2.png/v1/fill/w_435,h_88,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_e6396755229642a5b2b8aff399d27746~mv2.png)

Ma possiamo anche immaginare di creare un hidden layer che calcola la funzione di UPDATE come

![](https://static.wixstatic.com/media/4959fd_0f89ab8da6ea49be87251873df1f8192~mv2.png/v1/fill/w_436,h_101,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_0f89ab8da6ea49be87251873df1f8192~mv2.png)

  

## Possibili domande

1. What is the significance of the eigenvectors corresponding to the 0 eigenvalue of the Laplacian in graph clustering?
    
2. How does the Neural Message Passing mechanism work in Graph Neural Networks (GNNs)?
    
3. What are the two main issues addressed in Graph Construction for GNNs?
    
4. What is the purpose of the Personalized PageRank algorithm?
    
5. What are some methods for graph-level classification mentioned in the document?
    
6. What are the mathematical properties of Laplacians formed from adjacency matrices, and why are they important in graph analysis?
    
7. How do social recommender systems utilize local neighbors’ preferences to enhance user modeling?
    
8. What challenges arise when integrating user representations from social influence and interaction behavior perspectives in GNNs?
    
9. What is the role of the encoder-decoder approach in graph-based learning, and how does it function?
    
10. What is the significance of local overlap measures in graph analysis, and how are they calculated?
    
11. What is the concept of multi-relational data in knowledge graphs, and how does it affect graph analysis?
    
12. How does node centrality differ from eigenvector centrality in measuring the importance of a node in a graph?
    
13. What are the implications of using sampling strategies in GNNs for large-scale graph-based recommendation tasks?
    
14. What is the purpose of the 'message' in the Neural Message Passing framework, and how is it utilized?