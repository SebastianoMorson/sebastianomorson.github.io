---
layout: post
title: RECSYS Lecture 11 - Learning-to-Rank
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
categories: article

---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_7c960901cf744863aa4eec5fc3a1aa10~mv2.png/v1/fill/w_257,h_190,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_7c960901cf744863aa4eec5fc3a1aa10~mv2.png)

La domanda a cui vogliamo rispondere oggi è: perchè studiare solo modelli che si concentrano sull'ottimizzazione di predizioni, quando possiamo studiare modelli che si concentrano sul risolvere problemi di ranking?

  

Fin'ora abbiamo visto dei modelli che a partire da dati di input generavano delle predizioni (per esempio predicevamo le canzoni da consigliare e ordinavamo la ranking list in modo che le canzoni più coerenti fossero poste in cima), ma ora vogliamo cercare di trovare dei modelli che ci permettano di fare ranking degli elementi in modo da ottimizzare l'efficacia del ranking, senza dare troppa importanza allo score dei singoli elementi.

  

Facciamo subito un esempio: devo consigliare a Bob delle canzoni C1 C2 C3 C4 C5. Sò che lo score delle canzoni è rispettivamente del 98%, 97%, 80%, 75%, 70%. Sò che C1 e C2 sono molto simili tra loro. Posso decidere di presentare la lista come C1, C2, C3, C4, C5. Facciamo finta di ottenere da Bob un punteggio di apprezzamento del ranking di 80/100. Perchè non abbiamo ottenuto il 100/100? Perchè Bob non apprezza granchè le canzoni C1 e C2. Noi credevamo che gli piacessero, ma scopriamo che invece non è così. Se avessimo distanziato gli elementi C1 e C2 avremmo portato nelle prime posizioni le canzoni C3,C4 che invece sono quelle che Bob apprezza e avremmo ottenuto un ranking migliore.

In questo esempio è evidente come basarsi unicamente sugli score non sempre è la scelta più efficace.

  

Ed ecco quindi che possiamo presentare una nuova classe di algoritmi: la classe Learning-to-Rank.

  

## Learning-to-Rank

Gli algoritmi learning-to-rank hanno lo scopo di ottimizzare l'ordinamento degli elementi all'interno della ranking list, quindi come ho già detto, prediligono il punteggio complessivo ottenuto dalla lista finale piuttosto che sugli score individuali degli elementi nella lista.

Nei modelli tradizionali avevamo due casi:

- modelli query dipendenti: ossia gli elementi vengono ordinati in base a un set di queries a cui devono "rispondere"
    
- modelli query indipendenti: ossia gli elementi vengono ordinati non in base a una query, ma in base alle caratteristiche degli elementi stessi o in modo da rispondere alle preferenze dell'utente
    
      
    

Perciò già qui possiamo fare alcune previsioni su come funzionano:

- dovranno implementare un modo per confrontare due o più elementi al fine di ordinarli
    
- dovranno considerare una lista che funga da ground-truth per poter paragonare la lista prodotta con quella corretta
    

  

In effetti i metodi learning-to-rank possono adottare 3 tipi di approcci che rispondono in modo diverso proprio alle due previsioni che abbiamo fatto:

- point-wise approach:
    
    - considero un elemento candidato e una query e calcolo uno score di "coerenza" tra il candidato e la query
        
    - la lista finale viene generata seguendo l'ordinamento degli score
        
    - il problema è che gli elementi vengono considerati in modo indipendente, quindi non viene catturato l'ordinamento relativo tra gli elementi
        
- pair-wise approach:
    
    - confronto ogni elemento della ranking list con ogni elemento della ranking list per capire quale sia l'ordine parziale degli elementi rispetto a una certa query. Per ogni coppia (xi, xj) viene calcolata uno score di rank e gli elementi verranno ordinati in base a quello score
        
    - ciascun elemento non è considerato in modo indipendente, ma sempre rispetto a un altro elemento della lista
        
- list-wise approach:
    
    - viene considerata la lista per intero e si cerca di ottimizzare l'ordinamento della lista
        
    - il problema è l'evaluation gaming, bisogna fare in modo che il modello non apprenda il modo in cui il modello valuta la lista generata rispetto alla lista "ground-truth"
        

Se ve lo state domandando: sì, i metodi pointwise sono molto simili a quelli tradizionali, l'unica cosa che cambia è che i metodi tradizionali possono lavorare anche su dati non etichettati (perchè possono essere algoritmi non supervisionati) mentre i metodi learning-to-rank di tipo pointwise hanno bisogno di dati etichettati (perchè sono modelli supervisionati). Però almeno con i metodi pointwise si ha a che fare con modelli che riescono a catturare relazioni non lineari tra le features di input, al contrario dei metodi tradizionali.

Per fare un piccolo recap:  

![](https://static.wixstatic.com/media/4959fd_5f96058c45bf4c20aafb0938bd3f3ed1~mv2.png/v1/fill/w_592,h_309,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5f96058c45bf4c20aafb0938bd3f3ed1~mv2.png)

  

Bene, ora partiamo subito con il parlare di RankMART

  

## RankMART

RankMART è un modello pairwise che si basa sulla logistic regression.

Dati due documenti, l'idea è quella di

1. calcolare la probabilità che due elementi siano uno più grande dell'altro
    
    ![](https://static.wixstatic.com/media/4959fd_d8b7681c7e424966924478ef352ff12a~mv2.png/v1/fill/w_435,h_91,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_d8b7681c7e424966924478ef352ff12a~mv2.png)
    
2. calcolare il ground truth delle coppie
    
    ![](https://static.wixstatic.com/media/4959fd_d79bdffbf3ba46cda79a763bbecedee4~mv2.png/v1/fill/w_169,h_31,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_d79bdffbf3ba46cda79a763bbecedee4~mv2.png)
    
3. lasciare che un albero di regressione come ranking function f
    
    ![](https://static.wixstatic.com/media/4959fd_69d6fc59c1ac455a94adb614e533be2c~mv2.png/v1/fill/w_206,h_74,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_69d6fc59c1ac455a94adb614e533be2c~mv2.png)
    
4. calcolo la cross entropy tra gli score di ranking calcolati tramite function f
    

![](https://static.wixstatic.com/media/4959fd_bb0381f234114a7c9a94606e5ee8b790~mv2.png/v1/fill/w_280,h_56,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_bb0381f234114a7c9a94606e5ee8b790~mv2.png)

## LambdaMART

Non è che mi è preso il matto, RankMART serviva per poter spiegare LambdaMART.

Questo nuovo modello è di tipo listwise piuttosto interessante. I passaggi che svolge sono i seguenti:

1. uso RankMART su un insieme di documenti
    
2. prima di addestrare un nuovo albero di regressione ordino gli elementi sulla base dei risultati che ho ottenuto fino adesso
    
3. verifico che la lista ordinata abbia dei buoni margini tra gli elementi e migliori le valutazioni della lista
    
4. procedo con l'addestramento del nuovo albero
    

  

### Possibili domande

1. What is the input space in listwise approaches for learning to rank?
    

L'input space negli approcci listwise, data una generica query Q, è una lista di items L = {i1,i2,...,in} che viene considerata nella sua interezza come singolo elemento di input. Gli algoritmi listwise a differenza degli approcci pointwise e parwise, cercano di calcolare una funzione f tale che sull'input L e sui parametri theta, minimizzano la loss tra l'ordinamento della lista "predetto" e l'ordinamento "ground truth". Come funzione di loss alcuni algoritmi come LambdaMART usano la cross entropy loss (prima calcolano la posizione corretta degli elementi, poi controllano se la funzione f riesce a calcolare uno score corretto dando in pasto alla cross function il valore f(xi)-f(xj) calcolato su due elementi di indice i e j per cui sappiamo che yi<yj

2. What is the loss function used in the pairwise approach, specifically in RankNet?
    

La funzione di loss usata è la cross entropy function. RankNet è un approccio pairwise e la cross entropy mira a misurare se due elementi del ground truth vengono posizionati correttamente uno sopra l'altro o meno. L'idea della cross entropy è usare una logistic regression per calcolare la posizione relativa tra due elementi. Viene usata questa funzione perchè le metriche di ranking tradizionali come NDCG non sono direttamente derivabili e quindi non possono essere usate per aggiornare i parametri di un modello. La cross entropy è derivabile e consente di misurare la discrepanza tra le preferenze P(i>j) e le preferenze reali yij Migliorare la cross entropy del modello porta a migliorare indirettamente alcune metriche globali come NDCG.

  

3. What does LambdaMART do to minimize pairwise mismatches?
    

LambdaMART prima di addestrare un nuovo albero di regressione confronta l'ordinamento della lista ottenuto precedentemente con i risultati ottenuti nell'ultimo passaggio. In base ai risultati ottenuti dall'addestramento dell'ultimo albero (se migliorano o peggiorano la loss della lista delle preferenze) vengono aggiornati i parametri. La nuova lista delle preferenze viene quindi fornita in input per addestrare il nuovo albero di regressione.

LambdaMART perfeziona l'algoritmo pairwise RankMART integrandolo con l'aggiornamento dei gradienti in base alle metriche di ranking.

4. What is the main difference between Logistic Regression and traditional regression models?
    

La logistic regression permette di risolvere problemi di classificazione, mentre i metodi di regressione in generale permettono di risolvere problemi di predizione di valori continui (ad esempio la linear regression cerca di calcolare una retta che meglio approssima i punti su cui viene addestrata).

  

5. Explain the concept of the hypothesis space in listwise approaches to learning to rank. How does it differ from pointwise and pairwise approaches?
    

L'hypothesis space nella listwise approaches è definito come l'insieme delle funzioni che preso in input un insieme di insieme di descrizioni degli elementi da valutare, restituiscono come output una permutazione degli elementi dell'insieme di input ordinato in maniera coerente con quello che è l'ordinamento dell'insieme ground truth contenente i medesimi elementi.

Negli approcci pointwise l'hypothesis space è formato dalle funzioni che presa in input la descrizione di un elemento, restituiscono in output uno score valutato sulle caratteristiche dell'elemento in input.

Negli approcci pairwise l'hypothesis space è formato dalle funzioni che presi in input una coppia di elementi, restituiscono in output l'ordinamento relativo tra i due elementi in input.

  

6. What is the role of the sigmoid function in Logistic Regression, and how does it contribute to the model's output?
    

  

7. Discuss the significance of LambdaMART in the context of information retrieval metrics. How does it enhance the learning to rank process?
    

  

8. What are the main differences between the Boolean model and the Vector Space Model in information retrieval?
    

  

9. What are the key components of the loss function in listwise approaches, and how do they differ from those in pointwise and pairwise approaches?
    

  

10. Explain the significance of using a neural network in the RankNet algorithm for learning to rank. What advantages does it provide over traditional methods?
    

  

11. Discuss the implications of ignoring position information in pointwise and pairwise approaches to learning to rank. How does this affect the ranking outcomes?
    

  

12. Compare and contrast the Vector Space Model (VSM) and Latent Semantic Indexing (LSI) in terms of their approach to document representation and relevance assessment.
    

  

13. What is the primary focus of listwise approaches in learning to rank?
    

A) To evaluate individual documents

B) To process the entire group of documents associated with a query

C) To compare pairs of documents

D) To ignore document relevance

  

14. What loss function is used in RankMART?
    

A) Mean Squared Error (MSE)

B) Cross-entropy

C) Mean Absolute Error (MAE)

D) Logistic Loss

  

15. What does LambdaMART minimize in its ranking process?
    

A) Document retrieval time

B) Pairwise mismatches

C) User interaction rates

D) Document length

  

16. What type of model is Logistic Regression classified as?
    

A) Non-parametric regression model

B) Parametric classification model

C) Unsupervised learning model

D) Clustering model

  

17. Which model is based on the probabilistic ranking principle?
    

A) Boolean model

B) Vector Space Model

C) BM25

D) Latent Semantic Indexing