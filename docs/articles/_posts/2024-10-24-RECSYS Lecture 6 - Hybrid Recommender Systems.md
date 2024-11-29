---
layout: post
title: RECSYS Lecture 6 - Hybrid Recommender Systems
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
categories: article

---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_cc9aea2b7c994da9a586f1e9944ace85~mv2.png/v1/fill/w_322,h_182,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_cc9aea2b7c994da9a586f1e9944ace85~mv2.png)

Ecco a voi l'argomento per nulla atteso, la fusione delle fusioni: i sistemi di raccomandazione ibridi.

  

Ovviamente l'idea di base è quella di sfruttare la combinazione di sistemi già visti, per creare raccomandazioni più accurate.

  

Qui bisogna fare un'importante distinzione, ossia quella tra sistemi ensemble e sistemi ibridi:

- i sistemi ensemble sono sistemi che utilizzano più modelli di machine learning, simili o diversi tra loro, per creare delle previsioni su cui poi viene definita una votazione. Il fatto chiave è che però i modelli lavorano in maniera indipendente.
    
    - **voting**: considero la somma pesata dei risultati dei singoli modelli
        
    - **stacking**: considero i risultati dei singoli modelli che vengono dati in pasto a un ulteriore modello che effettuerà la predizione finale
        
    - **bagging**: gli input forniti ai singoli modelli (tutti dello stesso tipo) è un sottoinsieme del set di dati e il risultato finale è la somma pesata dei valori degli output dei singoli modelli
        
    - **boosting**: vengono forniti dei dati di input a un modello, il cui input è poi fornito a un altro modello che migliora gli errori del modello precedente. La somma pesata dei risultati funge poi da risultato finale
        

![](https://static.wixstatic.com/media/4959fd_730f8e912edb4108b21bf12f2d142b1e~mv2.png/v1/fill/w_538,h_1024,al_c,q_90,usm_0.66_1.00_0.01,enc_auto/4959fd_730f8e912edb4108b21bf12f2d142b1e~mv2.png)

  

- i sistemi ibridi invece sono sistemi che usano più modelli anche molto diversi tra loro che vengono eseguiti al fine di ottenere un singolo risultato. È possibile che un modello X manipoli risultati di un altro modello Y. I singoli modelli non lavorano in modo indipendente per raggiungere più risultati che devono essere votati come invece i sistemi ensemble.
    
    Possiamo classificare questi sistemi ibridi nelle seguenti categorie
    
    - **weighted:** uso più modelli e sommo i risultati
        
    - **switching**: in base al contesto decido se usare un modello piuttosto che un altro
        
    - **feature augmentation:** uso l'output di un modello per accrescere il numero di features dei dati passati al modello successivo
        
    - **feature combination:** raccolgo dati da più sorgenti e fondo assieme le loro features per poi darle in pasto a un singolo modello
        
    - **cascade:** uilizzo più modelli concatenati tra loro, è l'equivalente del boosting
        
    - **mixed:** propongo i risultati generati da più modelli (non è visto come un vero e proprio modello ibrido)
        
    - **meta-level:** utilizzo un modello per setacciare i dati da passare al modello successivo
        

  

Piccolo recap:

![](https://static.wixstatic.com/media/4959fd_a1bbb6dd83334648a4bbfe420767e573~mv2.png/v1/fill/w_488,h_254,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a1bbb6dd83334648a4bbfe420767e573~mv2.png)

  

Come al solito, le domande che ci dobbiamo porre quando abbiamo più metodi da poter applicare, sono:

- che differenza c'è tra ciascun modello?
    
    Dipende dai modelli che vogliamo paragonare, ma in genere uno quando viene applicato in un certo produce un buon risultato, quando viene usato in un altro contesto produce un risultato peggiore, mentre un altro ne produrrà uno migliore.
    
- in base a cosa posso valutare se un modello è migliore di un altro?
    
    Attraverso i risultati che produce
    
- eh, grazie al cazzo, ma quindi come vengono prodotti i risultati?
    
    ora lo vediamo, stai calmo.
    

  

Se vogliamo capire come vengono prodotti i risultati dobbiamo prima parlare di alcune cose

- come capire se i risultati sono buoni oppure no
    
    - varianza
        
    - bias
        
    - noise
        
- capire come vengono prodotti i risultati
    
    - gradient descent
        

  

#### Varianza

La varianza serve a capire quanto le nostre previsioni si discostano dai dati reali , ovverosia quanto sono concentrati o dispersi tra loro.

  

### Bias

Il bias serve a capire se le nostre previsioni si discostano dai dati reali secondo un errore sistematico o meno.

![](https://static.wixstatic.com/media/4959fd_13e3327e708849d7a437284718517cf4~mv2.png/v1/fill/w_592,h_466,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_13e3327e708849d7a437284718517cf4~mv2.png)

  

### Noise

La noise (o rumore) serve per capire se quanto le nostre previsioni sono soggette a un errore casuale.

  

### Gradient descent

Il gradient descent viene usato per aggiornare i parametri del modello in base a una funzione di costo. Se con certi parametri il valore della funzione di costo che stiamo usando si alza, aggiorneremo i parametri per cercare di abbassarla. L'obiettivo è quello di minimizzare la funzione di costo.

Prendiamo ad esempio una funzione di regressione lineare semplice, come questa qua

![](https://static.wixstatic.com/media/4959fd_5eac638075cf4b8d90ca9a6110a071eb~mv2.png/v1/fill/w_322,h_114,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5eac638075cf4b8d90ca9a6110a071eb~mv2.png)

Per capire quanto i nostri risultati sono belli, usiamo una funzione di costo definita come

![](https://static.wixstatic.com/media/4959fd_97272b2d639b483294f9ad2e974330fa~mv2.png/v1/fill/w_424,h_88,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_97272b2d639b483294f9ad2e974330fa~mv2.png)

Che produrrà un certo grafico, con delle concavità, delle curve etc etc.

Comunque questa funzione di costo sarà di questo tipo

![](https://static.wixstatic.com/media/4959fd_6dc1b9c8f15c47fe880b9ac07e8c2797~mv2.png/v1/fill/w_351,h_210,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_6dc1b9c8f15c47fe880b9ac07e8c2797~mv2.png)

Per certi valori dei parametri (nell'immagine corrisponde a "weight") la funzione di costo crescerà, per altri valori decrescerà. Noi vogliamo capire per quale valore la funzione di costo è minima.

Ora facciamo caso a quella sigla chiamata "gradient" e a quel pallino nero che è il valore iniziale del nostro parametro. Il gradiente è rappresentato da quella linea tratteggiata e non è altro che la derivata della funzione in quel punto, ossia in parole povere la pendenza della curva. Secondo quella linea, se mi sposto a destra mi allontano dal minimo, se mi sposto a sinistra mi avvicino.

Eh, ma come facciamo quindi a fare in modo che il gradiente si sposti?

Semplice, aggiorniamo il valore del parametro in base al valore del gradiente. Se il gradiente è positivo (pendenza positiva) allora decremento il valore del parametro (mi sposto a sinistra). Viceversa se il gradiente è positivo incremento il valore del parametro (mi sposto a destra).

In sostanza faccio questo

![](https://static.wixstatic.com/media/4959fd_b1518072a9734619a53b2af016453c23~mv2.png/v1/fill/w_592,h_286,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_b1518072a9734619a53b2af016453c23~mv2.png)

È importante notare che esitono più modi per aggiornare i parametri.

Possiamo infatti

- aggiornare i parametri una volta che tutti i sample sono stati predetti -> stochastic gradient descent
    
- aggiornare i parametri ogni volta che viene predetto un sample -> mini-batch gradient descent
    
- aggiornare i parametri quando un certo numero di sample è stato predetto -> batch gradient descent
    

![](https://static.wixstatic.com/media/4959fd_c0413dbfe201494eac1a509b977da89b~mv2.png/v1/fill/w_592,h_208,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c0413dbfe201494eac1a509b977da89b~mv2.png)

  

## Weighted model

Questo tipo di modello è piuttosto semplice, non si fa altro che sommare i valori delle predizioni date dai singoli modelli.

![](https://static.wixstatic.com/media/4959fd_2ab6f1eed3b24aed80cc1b0623e99121~mv2.png/v1/fill/w_345,h_134,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_2ab6f1eed3b24aed80cc1b0623e99121~mv2.png)

Come prima, ora bisogna trovare una funzione di costo opportuna, tipo questa

![](https://static.wixstatic.com/media/4959fd_a89455979ae646efa62916ab00696ea2~mv2.png/v1/fill/w_378,h_154,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a89455979ae646efa62916ab00696ea2~mv2.png)

  

E per finire si aggiornano i pesi

![](https://static.wixstatic.com/media/4959fd_a3d5dbae71564ea681bdb5ec9c1ff9f4~mv2.png/v1/fill/w_315,h_162,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a3d5dbae71564ea681bdb5ec9c1ff9f4~mv2.png)

Tutto molto semplice.

  

  

## Switching

Questo tipo di sistema è basato sul cambiare modello di raccomandazione in base alle necessità del contesto. Ad esempio in caso ci siano problemi di cold-start il sistema potrebbe optare per usare un modello knowledge based per poi usare un modello content-based non appena l'utente abbia valutato un numero sufficiente di items.

  

I pro sono chiaramente questa adattabilità alle situazioni, ma di contro non sono visti propriamente come dei sistemi ibridi.

  

## Cascade

I sistemi ibridi cascade sono modelli in cui si esegue un refinement degli output dei modelli prima di passarli ai modelli successivi della catena. Questo permette di diminuire l'errore della predizione finale.

  

## Feature combination

L'idae non è altro che considerare più sorgenti di dati, combinarli e usare le features estratte come input per un modello.

Ad esempio si può usare dei dati collaborativi (numero di utenti che hanno valutato positivamente un certo items, etc. ) per poi addestrare un modello che invece è di tipo content-based.

  

Come funzione di costo può essere usata qualcosa del tipo

J = collaborative object (θ) + ß·content-based object(θ) + termine di regolarizzazione