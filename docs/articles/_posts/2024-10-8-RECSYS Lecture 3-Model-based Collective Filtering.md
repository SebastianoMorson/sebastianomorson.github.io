---
layout: post
title: RECSYS Lecture 3 - Model-based Collective Filtering
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
---
<!--more-->

![[Pasted image 20241128172825.png]]
Ormai sembrava tutto così chiaro, così comprensibile e magico. Invece no. Ecco a voi i model-based methods per il collective filtering!

Praticamente i metodi Neighborhood-based che abbiamo conosciuto nella lezione precedente erano carini, coccolosi, ma per nulla efficienti. Sono vecchi e si comportano da schifo quando abbiamo a che fare con matrici sparse, dove tante valutazioni sono mancanti.

Oltretutto nel mondo dell'intelligenza artificiale c'è una cosa chiamata "preprocessing dei dati" che serve per tenere occupati i data scientists mentre i grandi fanno cose da grandi, ma con i metodi neighborhood-based abbiamo visto che non ci sono grandi lavori da fare sulle matrici sparse. Non c'è divisione in training set, validation set e test set. Non c'è nemmeno una pulizia dei dati, nè uno studio di correlazione per rimuovere features fortemente correlate. Nulla di nulla. Come nei campi di riso cinesi, con i metodi neighborhood based ci lanciamo in mezzo al fango e raccogliamo tutto quello che ci capita a tiro.

  

Non è esattamente il comportamento più efficiente.

  

Ed ecco a voi la famiglia di metodi Model-based.

Questi metodi hanno la caratteristica di eseguire le previsioni solo dopo che i dati sono stati preprocessati e manipolati a nostro piacimento. Già qua vediamo un bel vantaggio: viene ridotta la dimensione della matrice. Ciò significa che, per quanto nel caso peggiore non riusciremo a ridurre un bel niente, nel caso medio avremo a che fare con complessità temporali inferiori a quelle dei metodi item-based e user-based visti con l'approccio basato su vicinato.

Bando alle ciance, iniziamo a parlare del primo di questi metodi: il metodo Naive Bayes.

## Naive Bayes Collective Filtering

Il metodo Naive Bayes dovrebbe far scattare una lampadina dentro la nostra scatola cranica: TEOREMA DI BAYES. Per chi non se lo ricordasse ho inserito l'articolo dove ne parlo tra gli articoli in fondo alla pagina.

Il teorema di Bayes era quello che diceva fondamentalmente che se ho un evento A condizionato a un evento B, allora la probabilità P(A|B) era uguale al prodotto della probabilità di A, moltiplicata per la probabilità P(B|A), divisa per la probabilità dell'evento B.

![](https://static.wixstatic.com/media/4959fd_a81bb00f5f9940b3a0bd796f00af8a0c~mv2.png/v1/fill/w_370,h_85,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a81bb00f5f9940b3a0bd796f00af8a0c~mv2.png)

L'idea di questo metodo è quello di usare come predizione la probabilità che un certo items j riceva una certa valutazione q da parte di un utente target t, sapendo che t ha valutato un insieme di elementi I.

Per cominciare immaginiamo la nostra solita tabella R che contiene i ratings degli items da parte degli utenti. Ricordiamo che questa tabella può essere fortemente sparsa.

Ora, assumiamo che i nostri ratings siano di tipo categoricorico e che ci siano _l_ categorie {v1,v2, .., v_l_}.

Assegnamo a q1..ql il numero di valutazioni in cui è stato indicata la categoria v1,..vl.

Otteniamo un qualcosa del tipo

|   |   |   |
|---|---|---|
|q1|"poco"|3|
|q2|"tanto"|6|
|...|...|...|
|ql|"mediamente"|4|

Questa enumerazione serve perchè quando parliamo di probabilità abbiamo bisogno di uno spazio campionario, e in particolare di individuare quali sono i nostri eventi.

Ora se vogliamo capire quanto il film "titanic" piace a Bob, ci basta calcolare la probabilità che il rating per i="Bob" e j = "titanic" sia uguale a una certa categoria v, sapendo che Bob ha valutato gli altri film con certe categorie che conosciamo.

La formula per il calcolo della probabilità condizionale è quindi:

![](https://static.wixstatic.com/media/4959fd_ccfafa3659524b4e93755e12abac8c32~mv2.png/v1/fill/w_592,h_70,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_ccfafa3659524b4e93755e12abac8c32~mv2.png)

Per calcolare la probabilità delle valutazione in I sapendo che l'elemento target è stato valutato v, usiamo la produttoria, essendo eventi dipendenti

![](https://static.wixstatic.com/media/4959fd_919eb30482814ce4aa38724fbb74d9bc~mv2.png/v1/fill/w_592,h_82,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_919eb30482814ce4aa38724fbb74d9bc~mv2.png)

Quindi possiamo riscrivere la formula iniziale come

![](https://static.wixstatic.com/media/4959fd_c05c2f240eea4f0a8f324749676a3543~mv2.png/v1/fill/w_592,h_113,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c05c2f240eea4f0a8f324749676a3543~mv2.png)

Sappiamo che il termine a sinistra è proporzionale al numeratore del termine a destra, perciò possiamo scrivere

![](https://static.wixstatic.com/media/4959fd_321ae06ad2c64418ad7a5b7f8d4bfc06~mv2.png/v1/fill/w_309,h_46,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_321ae06ad2c64418ad7a5b7f8d4bfc06~mv2.png)

Perciò

![](https://static.wixstatic.com/media/4959fd_bfa499092da149658c9b23c243f54565~mv2.png/v1/fill/w_592,h_73,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_bfa499092da149658c9b23c243f54565~mv2.png)

Ma allora per prevedere il nostro rating è calcolare la media pesata delle probabilità:

![](https://static.wixstatic.com/media/4959fd_43c6ca5edf8c4474ac6991624051d58f~mv2.png/v1/fill/w_592,h_410,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_43c6ca5edf8c4474ac6991624051d58f~mv2.png)

Il problema principale di questo metodo è che funziona finchè abbiamo tante valutazioni e la matrice non è tanto sparsa, ma se la matrice inizia ad avere poche valutazioni per un certo elemento j oppure ci sono pochi elementi che sono stati valutati "good"(ad esempio), il metodo diventa meno robusto perchè non c'è tanto "condizionamento". Per questo usiamo il "laplacian smoothing" per gestire questa situazione.

Quello che facciamo non è altro che modificare il modo in cui gestiamo la probabilità che Bob abbia valutato "titanic" come "carino", inserendo un parametro **α**. Lo smoothing si calcola con la formula seguente:

![](https://static.wixstatic.com/media/4959fd_5b9a9923482e403283e234e318feb377~mv2.png/v1/fill/w_382,h_120,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5b9a9923482e403283e234e318feb377~mv2.png)

- Se **α è <1 avremo che la probabilità si alza**
    
- Se **α circa 1 avremo che la probabilità rimane pressochè uguale**
    
- Se **α è >1 avremo che la probabilità si abbassa**

Grandi valori di **α servono quindi ad aumentare le probabilità di quelle categorie che non vengono spesso assegnate.**

## Latent Factor Models

Uuuh, ecco che parliamo di un argomento tanto interessante quanto non tanto user-friendly: il metodo dei fattori latenti.

Praticamente l'idea è semplice: in una matrice delle valutazioni potrei avere delle righe o delle colonne che sono fortemente correlate, ossia certi utenti hanno valutato in maniera simile degli items che hanno delle caratteristiche simili.

L'esempio classico è quello dei film.

Consideriamo una tabella come questa in cui 1 indica "mi piace" e -1 indica "non mi piace"

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||American Sniper|Titanic|Chernobyl|Harry Potter|Narnia|
|Bob|1|1|1|-1|-1|
|Alice|1|1|1|-1|-1|
|Carlos|1|1|1|-1|-1|
|Pietro|-1|-1|1|1|1|
|Lucilla|-1|-1|1|1|1|

posso notare alcune cose, ad esempio che ci sono porzioni della tabella con una prevalenza di 1 o di -1.

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||American Sniper|Titanic|Chernobyl|Harry Potter|Narnia|
|Bob|1|1|1|-1|-1|
|Alice|1|1|1|-1|-1|
|Carlos|1|1|1|-1|-1|
|Pietro|-1|-1|-1|1|1|
|Lucilla|-1|-1|-1|1|1|

Il fatto curioso è che i film appartengono a categorie ben definite:
- american sniper, titanic e chernobyl sono film basati su una storia vera
- harry potter e narnia sono film di fantasia

Non è che Bob, Alice e Carlos sono amanti del genere realistico, mentre Pietro e Lucilla del genere Fantasy? Può darsi. Ma allora a questo punto per sapere se il genere del film è "realistico" o "fantasy" per capire se piacerà o meno a quell'utente.

Perciò potrei: prendere un certo numero di "fattori latenti"(ossia attributi nascosti che non conosciamo), una matrice U di dimensione m·k che rappresenta la descrizione degli utenti e una matrice V di dimensione n·k che rappresenta la descrizione degli items. Dopodichè spariamo dei valori dentro U e V cercando di fare assomigliare il loro prodotto cartesiano alla matrice iniziale. In poche parole l'idea è fattorizzare la matrice per riconoscere dei comportamenti non espliciti.

### Fattorizzazione della matrice

Partiamo da una cosa molto semplice: le componenti latenti non le conosciamo. Non è che la nostra matrice ha dei campi "genere", "durata", etc. sennò staremmo parlando di content-based filtering. Quindi non possiamo dire a priori "ci sono 5 componenti latenti"! È per questo che dobbiamo studiare il numero ottimale da usare. Per farlo sfruttiamo la fattorizzazione di matrici.

Una matrice R può essere vista come il prodotto di due matrici U e V, in questo modo

![](https://static.wixstatic.com/media/4959fd_40b6050d01fc495cae327a01a69affd2~mv2.png/v1/fill/w_592,h_287,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_40b6050d01fc495cae327a01a69affd2~mv2.png)

Per questioni di copyright e per questioni di "non ho voglia di disegnare", ho preso l'immagine da questo [sito.](https://www.google.com/url?sa=i&url=https%3A%2F%2Ftowardsdatascience.com%2Frecommender-systems-matrix-factorization-using-pytorch-bd52f46aa199&psig=AOvVaw1N3bG_SCFzxSKnJ5r-7i0F&ust=1728983802465000&source=images&cd=vfe&opi=89978449&ved=0CBcQjhxqFwoTCICm0tzEjYkDFQAAAAAdAAAAABAJ) Invece di scrivere ogni volta "users" e "items", mi riferirò alle due matrici come U e V.

Una volta fattorizzata, dobbiamo andare a "indovinare" quale siano i valori migliori da attribuire alle celle di U e di V. Giustamente potreste chiedermi: come capiamo quali valori sono migliori? Usando una funzione di ottimizzazione, ossia una funzione come questa:

![](https://static.wixstatic.com/media/4959fd_362cdb62a51747d4b555552fdf6fcbb2~mv2.png/v1/fill/w_592,h_150,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_362cdb62a51747d4b555552fdf6fcbb2~mv2.png)

in cui semplicemente cerchiamo di minimizzare la differenza tra le celle della matrice R e della matrice R' ottenuta come il prodotto cartesiano delle due matrici U e V.

![](https://static.wixstatic.com/media/4959fd_4f57f104215947f98dad52a14272925a~mv2.png/v1/fill/w_592,h_117,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_4f57f104215947f98dad52a14272925a~mv2.png)

Per la cronaca: || · ||² indica la normalizzazione di Frobenius, ossia un'operazione che permette di calcolare la distanza tra due matrici.

Problema: le matrici U e V vengono costruite "black-box", non ci sono dei motivi espliciti che spiegano perchè abbiamo messo un valore x dentro la cella U[i,j] e perchè un valore y nella cella V[z,t]. La motivazione è semplicemente "perchè rendevano R' circa uguale a R". Piuttosto scarsa come spiegazione, infatti è un approccio eticamente rischioso da usare. Che succederebbe se il mio modello applicasse discriminazioni "razziste" o moralmente inaccettabili? Lo sviluppatore non ha il controllo totale sulle decisioni che prende il modello durante l'addestramento. A parte questo però, il metodo della fattorizzazione funziona piuttosto bene perchè riduce la dimensionalità delle matrici da dover manipolare.

Quindi per ricapitolare:

- la fattorizzazione non è esplicita nelle motivazioni
    
- la fattorizzazione permette di ridurre la dimensionalità dei dati
    
- si usano delle funzioni di ottimizzazione per capire quali valori assegnare alle matrici U e V

Quali sono i metodi per fattorizzare? Sono tutti uguali o si può scegliere come eseguirla?

Risposta: si può scegliere. Ma prima parliamo della basic matrix factorization, che è per l'appunto basic.

### Fattorizzazione con regularization

Nel caso ci siano poche valutazioni si rischia di incorrere in overfitting, per questo viene applicata la regolarizzazione, calcolata come

![](https://static.wixstatic.com/media/4959fd_2d452e5988544d2cb53dd5602d896ab5~mv2.png/v1/fill/w_286,h_74,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_2d452e5988544d2cb53dd5602d896ab5~mv2.png)

![](https://static.wixstatic.com/media/4959fd_1fce7f9f19514b2bb227e0ce365cc456~mv2.png/v1/fill/w_592,h_85,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1fce7f9f19514b2bb227e0ce365cc456~mv2.png)

Oltretutto potremmo voler aggiungere dei bias per gli utenti e gli items, trasformando la formula di ottimizzazione in questa roba qua, normalizzando anche questi due nuovi bias

![](https://static.wixstatic.com/media/4959fd_be0270927e644207bf315a1380200010~mv2.png/v1/fill/w_592,h_208,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_be0270927e644207bf315a1380200010~mv2.png)

  

### Singular Value Decomposition (SVD) & Non-negative Matrix Factorization

Questi due metodi sono intuitivamente molto semplici:

- il primo metodo richiede che la matrice U e V siano tra loro ortogonali
    
- il secondo che non ci siano valori negativi
    

  

Che senso ha fare queste richieste? Dipende dal contesto. Ci sono contesti in cui sappiamo che i valori di R non possono essere negativi, quindi con il vincolo di non negatività riduciamo lo spazio di ricerca. Inoltre se i vettori che compongono due matrici sono ortogonali, implica che i vettori sono indipendenti tra loro. Questo è comodo, perchè significa che non c'è correlazione tra fattori latenti.

  

  

### Possibili domande

1. What is the main issue with the Unconstrained Matrix Factorization method, and how is it addressed?

2. How does the Naive Bayes Collaborative Filtering approach estimate unobserved ratings for a user?

3. What are the roles of user factors and item factors in Basic Matrix Factorization?

4. What is the significance of incorporating user and item biases in a recommendation model?

5. What is the advantage of Non-negative Matrix Factorization in the context of implicit feedback?

6. Explain the concept of Singular Value Decomposition (SVD) and its significance in recommendation systems.

7. Discuss the differences between neighborhood-based methods and optimization models in collaborative filtering.

8. What are the implications of using latent models in collaborative filtering compared to classical machine learning models?

9. How does the introduction of user and item biases improve the performance of recommendation systems?

10. Describe the role of latent components and latent factors in Basic Matrix Factorization.

12. What is the primary advantage of using Singular Value Decomposition (SVD) in recommendation systems?

13. How do hybrid recommender systems benefit from integrating latent models with arbitrary models?

14. What is the significance of incorporating user and item biases in recommendation models?

15. Explain the concept of Naive Bayes Collaborative Filtering and its approach to estimating user ratings.

16. What are the roles of latent components and latent factors in Basic Matrix Factorization?

17. What is the role of truncated Singular Value Decomposition (SVD) in recommendation systems, and how does it facilitate out-of-sample recommendations?

18. Discuss the differences between neighborhood-based methods and optimization models in collaborative filtering. How can they be integrated?

19. Explain the significance of user and item biases in the context of recommendation systems. How do these biases affect the model's predictions?

20. What are the implications of using Naive Bayes Collaborative Filtering for estimating user ratings, and how does it differ from traditional collaborative filtering methods?

21. Describe the concept of latent factors in Basic Matrix Factorization and their importance in the recommendation process.