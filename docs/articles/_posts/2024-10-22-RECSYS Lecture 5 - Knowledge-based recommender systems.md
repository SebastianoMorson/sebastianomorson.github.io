---
layout: post
title: RECSYS Lecture 5 - Knowledge-based recommender systems
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_317f86d1fd42497d8edfda976e7da44a~mv2.png/v1/fill/w_328,h_441,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_317f86d1fd42497d8edfda976e7da44a~mv2.png)

Ebbene sì, vi ho mentito sapendo di mentire. Non esistono solo i sistemi content-based e collaborative, ma pure quelli Knowledge-based. Questi sistemi sembrano piuttosto interessanti, perchè non solo piombano a gamba tesa sulle povere caviglie dei due sistemi che abbiamo già visto (collaborative e content-based), ma perchè sono ancora più fighi cazzo.

  

Esistono 2 tipi di metodi knowledge-based:

- constraint based
    
- cause based
    

ma l'idea comune è questa:

**IDEA: Perchè basare i suggerimenti su quello che Bob ha già valutato, quando possiamo farci dire da lui quali sono le caratteristiche delle cose che gli piacciono?**

Cambia tutto! Con questo approccio il problema del cold start viene depotenziato, perchè di base non ci interessa avere tanti dati per poter fare dei suggerimenti, ma solo delle buone indicazioni da parte dell'utente. Ovviamente però se abbiamo pochi items è un fastidio, dal momento che dobbiamo proporre cose simili a quelle che ci indica Bob: se ci chiede qualcosa simile a un armadillo e nel nostro set di items abbiamo solo una sedia, un facocero e una pianta grassa, l'unica roba simile che potremo proporgli è il facocero, ma potrebbe non piacergli.

D'altro canto se siamo nella situazione in cui abbiamo pochi items valutati e dobbiamo capire cosa può piacere a Bob, questo approccio è ottimo! Pensiamo un attimo a dove potremmo aver trovato un sistema di raccomandazione simile:

- Amazon: se ci pensiamo, quante volte in un anno recensiamo un nuovo computer? Quasi mai. Una volta che ne prendiamo uno poi smettiamo di guardarne altri per almeno un paio di anni. Come farebbe Amazon a suggerirci nuovi elementi riguardanti i computer dopo più di un anno che non ne acquistiamo uno?
    
- Shein: se una volta ogni morte di Papa acquistiamo dei vestiti, magari su piattaforme diverse, come dovrebbe fare il sistema a capire quali ci piacciono e quali no? Non ci sono voci nella nostra cronologia, magari sono poche, e la moda cambia. Come fa a capire cosa ci piace?
    

Semplicemente quello che fa è chiederci una parola chiave da usare per cercare elementi con quella parola e ci fornisce dei filtri attraverso i quali imporre dei vincoli su quello che vorremmo ci fosse proposto.

  

  

## Cosa ci serve per implementare questi metodi

Quello di cui abbiamo bisogno per implementare questi metodi è:

1. la descrizione degli oggetti da poter suggerire
    
2. un modo per estrarre i feedback dell'utente
    
3. un modo per tradurre i requisiti dell'utente in attributi degli items (per poterli confrontare)
    
4. un modo per verificare la somiglianza tra gli items e i requisiti
    
5. un modo per ordinare gli oggetti da suggerire
    

  

Fondamentalmente quello che cambia tra il constraint approach e il case-based approach sta soltanto nel come vengono estratti i feedback dell'utente e come vengono verificate le somiglianze tra items e requisiti.

## Constraint-based approach

In questo approccio, come dice il nome, avremo a che fare con dei vincoli. Che significa? Che andremo a definire cosa vogliamo che ci venga suggerito chiedendo all'utente di descriversi e di descrivere le caratteristiche degli oggetti che gli interessano. Da qui il sistema capirà cosa potrebbe interessargli.

  

👁️ il nostro sistema potrebbe inferire alcuni vincoli sulla base delle informazioni fornite

  

Gli esempi che farò si baseranno su un sito di e-commerce per l'acquisto di carne. Un utente accede al sito che gli permette di ricevere suggerimenti sulla spesa da fare per organizzare una grigliata con gli amici in base alle informazioni fornite.

  

#### Quali tipi di input gestiremo?

- informazioni sull'utente (ad esempio quanti siamo alla festa e quanto pesiamo in media)
    
- informazioni sull'item (ad esempio se preferiamo che sia di vitello, manzo, bovino o suino)
    

#### Come mappiamo gli input sugli elementi?

- mapping diretto: non viene applicata nessuna derivazione
    
- mapping knowledge-based: uso gli input forniti come base per derivare altri input. Ad esempio se Bob mi dice che la festa è per 10 persone da 100kg ciascuno, non gli propongo 10 bistecchine di lonza, ma assumo che in input venga specificato (in modo non esplicito) che spenderemo almeno 25euro a persona
    

  

#### Il sistema può conoscere dei vincoli da applicare in relazione a un certo input?

Sì, il sistema potrebbe già conoscere i vincoli da applicare in aggiunta agli input forniti. L'approccio può quindi essere

- diretto: se Bob dice che sono in 10, impongo che la spesa minima sia di 25 euro
    
- indiretto: se Bob dice che sono in 100, allora è ragionevole assumere che la carne sia già stata aromatizzata perchè sennò Bob dovrebbe farlo a casa, oppure che la data di consegna sia come minimo 3 giorni dopo la data dell'ordine
    

  

### Come decido cosa ritornare all'utente?

Semplicemente traduco il problema in un constraint satisfiable problem e cerco di risolverlo. Quindi posso applicare metodi di automated-reasoning per gestire lo spazio di ricerca e ottimizzare il mio problema.

Ovviamente ci possono essere delle difficoltà come vincoli inconsistenti.

  

### Come decido cosa proporre in cima alla pagina?

Uso un metodo di ranking. Easy.

Qualcosa del genere potrebbe andare bene

![](https://static.wixstatic.com/media/4959fd_70386299bddc4ae5b94ab22f0d2c9c59~mv2.png/v1/fill/w_592,h_269,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_70386299bddc4ae5b94ab22f0d2c9c59~mv2.png)

L'idea è semplicemente quella di considerare un elemento **_V_** che soddisfa i miei vincoli ed un insieme di pesi **_W_** associato a tutti gli attributi dell'elemento **_V_**. Dopodichè, banalmente, eseguo la somma pesata dei valori degli attributi dell'elemento **_V_**. La funzione **_f_** serve per gestire i tipi di input forniti per ciascun attributo (potrebbero essere di varia natura).

  

👁️ Se i vincoli sono troppo stretti non otterremo i risultati desiderati (magari addirittura nessuno) quindi bisognerà rilassare un attimo i vincoli. Se i vincoli sono troppo generici invece riceveremo un numero troppo grande di suggerimenti, perciò bisognerà aggiungere nuovi input.

  

## Case-based approach

In questo approccio invece di creare dei vincoli usiamo un item di esempio e poi eseguiamo un raffinamento dei risultati.

Ad esempio, Bob ha visto il film Deadpool e gli è piaciuto un sacco. Non sa di preciso cosa gli è piaciuto, ma sa che vorrebbe vedere un film simile. Con il metodo constraint-based dovrebbe vincolare l'attore a Ryan Reynolds, imporre il genere comico e di azione, etc etc.

Un'operazione che implica che Bob conosca il dominio di ciò che vuole.

Problema, Bob è un signore di 85 anni che vuole acquistare un computer. L'ultima volta che l'ha fatto era nel 1998 e si ricorda solo che il suo ultimo pc aveva 100MB di RAM. Non conosce le novità uscite sul mercato, ma sà che si era trovato molto bene con quel pc. Bob vorrebbe qualcosa di simile a quel prodotto, ma non conosce tutti i dettagli tecnici. In questo caso il case-based approach è perfetto, è sufficiente che Bob specifichi il prodotto desiderato e il modello troverà gli oggetti simili a quello.

  

  

È chiaro che per implementare una cosa del genere abbiamo bisogno di:

1. un modo per ottenere le informazioni sul prodotto target di Bob
    
2. una descrizione di tutti gli elementi
    
3. un modo per confrontare l'elemento target di Bob con gli items a disposizione
    
4. un modo per affinare la ricerca di Bob
    

  

### Risolviamo i nostri problemi

**Soluzione al problema 1:** Le informazioni sui prodotti potrebbero essere già salvate in un database. Ad esempio potrei avere una pagina di Amazon in cui seleziono un prodotto già acquistato e lui mi trova elementi simili. Non ho bisogno di indicare le informazioni sul prodotto, sono già registrate a sistema quando il venditore ha messo in vendita l'articolo.

  

**Soluzione al problema 2:** stessa soluzione di prima

  

**Soluzione al problema 3:**

qui le cose diventano un attimo più articolate, iniziamo a parlare di funzione di similarità (come al solito).

Assumiamo che Bob voglia trovare qualcosa di simile a un oggetto **_T._** T avrà un numero **_d_** di attributi. Non è detto che ci interessi trovare una similarità su tutti gli attributi, quindi assumiamo che quelli che ci interessino siano contenuti in un insieme **_S._**

Per calcolare la similarità tra due oggetti T e X useremo la formula seguente, che non è altro che la media pesata degli attributi dei due items

![](https://static.wixstatic.com/media/4959fd_04946cec97664012b208eba9bd08d9c6~mv2.png/v1/fill/w_592,h_174,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_04946cec97664012b208eba9bd08d9c6~mv2.png)

Il sistema dovrà capire quali oggetti sono simili a T, ma ora il dettaglio a cui fare attenzione è il seguente:

- vogliamo qualcosa di proprio uguale uguale,
    
- qualcosa di simile agli articoli che ho già acquistato o
    
- qualcosa che abbia certe caratteristiche uguali e altre migliori?
    

  

Nel primo caso useremo una funzione di similarità simmetrica

![](https://static.wixstatic.com/media/4959fd_fa97dbdac51f4e148fb2a3e3a6403672~mv2.png/v1/fill/w_525,h_177,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_fa97dbdac51f4e148fb2a3e3a6403672~mv2.png)

Nel secondo caso useremo una funzione di similarità storica (il denominatore corrisponde alla deviazione standard dei valori dell'attributo **_i_** )

![](https://static.wixstatic.com/media/4959fd_0cd9ebebf37d423eab1272c9f7a87bb2~mv2.png/v1/fill/w_592,h_116,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_0cd9ebebf37d423eab1272c9f7a87bb2~mv2.png)

Nel terzo caso useremo una funzione di similarità asimmetrica (in questo caso assumiamo che l'attributo **_i_** dell'elemento **_x_** sia più grande del nostro elemento target)

![](https://static.wixstatic.com/media/4959fd_a0c2863a62d34c459dda6f96eea94e6d~mv2.png/v1/fill/w_592,h_114,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a0c2863a62d34c459dda6f96eea94e6d~mv2.png)

  

**Soluzione al problema 4:**

Una volta capito quali elementi sono simili al nostro elemento target, dobbiamo fare in modo di affinare la ricerca. Per farlo usiamo il concetto di "**critiques**".

Una critica non è altro che un feedback dell'utente sulla risposta fornita dal sistema. Se gli piace non dirà nulla, ma se non è esattamente quello che cercava potrebbe voler cercare elementi simili a quello trovato, ma con attributi leggermente diversi.

Esistono 3 tipi di critiques:

- simple critique: offro all'utente la possibilità di variare un attributo del nostro item suggerito
    
    ![](https://static.wixstatic.com/media/4959fd_edd451a6b61d4db8a844d40c14fd68d7~mv2.png/v1/fill/w_407,h_294,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_edd451a6b61d4db8a844d40c14fd68d7~mv2.png)
    
- compounded critique: offro all'utente la possibilità di variare un numero multiplo di attributi del nostro item. Possiamo creare delle combo che automaticamente applicano più variazioni sugli attributi
    
    ![](https://static.wixstatic.com/media/4959fd_1d2c6432cf4246bca645a4c6859dac61~mv2.png/v1/fill/w_405,h_292,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1d2c6432cf4246bca645a4c6859dac61~mv2.png)
    
- dynamic critique: offro all'utente la possibilità di variare più attributi contemporaneamente, ma specificando quali sono le opzioni migliori di cambiamento delle variabili basandosi sugli items presentati
    
    ![](https://static.wixstatic.com/media/4959fd_1c73fe91e41246269a0dc0a71c1bf8b3~mv2.png/v1/fill/w_409,h_293,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1c73fe91e41246269a0dc0a71c1bf8b3~mv2.png)
    

  

|   |   |   |
|---|---|---|
|SIMPLE CRITIQUE|COMPOUNDED CRITIQUE|DYNAMIC CRITIQUE|
|molto semplici|permettono di ridurre il tempo per|permettono di ridurre il carico cognitivo dell'utente|
|se le critiche sono tante, l'utente spreca tanto tempo a correggere il suggerimento|si rischia di non capire la correlazione tra le modifiche alle features e i risultati ottenuti|più complesse da implementare|
|rischio di non capire la correlazione tra le features|non si basano sugli items suggeriti|si basano sugli items suggeriti|

  

## Bounded Greedy Selection Strategy

I problemi non mancano mai. Abbiamo detto che usiamo la similarity per individuare items simili a quello che abbiamo usato fornito al sistema come esempio. Però c'è un però: che succede se tutti gli items suggeriti sono troppo simili? Non ci piace questa casistica, perchè se il primo elemento raccomandato non piace all'utente, è molto probabile che anche quelli dopo non piaceranno. È quindi necessario definire una funzione che calcoli la qualità degli item suggeriti sulla base della diversità degli item stessi.

  

La bounded greedy selection strategy è proprio un metodo che cerca di risolvere il problema descritto. L'idea è selezionare b*k elementi tra quelli da suggerire e calcolare la qualità della raccomandazione sulla base della diversità rispetto agli elementi di un insieme R (che all'inizio è vuoto). Di volta in volta viene inserito l'elemento che ha ottenuto lo score migliore e che è il più diverso rispetto a quelli già contenuti all'interno dell'insieme R. That's it. Questa è l'idea.

![](https://static.wixstatic.com/media/4959fd_53f7d71182404a198863ec6bd15a2e3c~mv2.png/v1/fill/w_592,h_180,al_c,lg_1,q_85,enc_auto/4959fd_53f7d71182404a198863ec6bd15a2e3c~mv2.png)

  

## Constraint vs case-based: pro e contro

  

|   |   |
|---|---|
|Constraint PRO|Constraint CONTRO|
|efficaci quando l'utente conosce il dominio|se l'utente non conosce il dominio perdono di efficacia|
|l'utente ha un maggior controllo|filtri sbagliati possono fare perdere tempo|
||approccio black-box|

  

|   |   |
|---|---|
|case-based PRO|case-based CONTRO|
|più user-friendly del metodo constraint-based|l'utente può ricevere più facilmente suggerimenti non consoni|
|non serve che l'utente conosca il dominio||
|all'utente può essere fornita una motivazione sui suggerimenti||

### Possibili domande

1. What are the two crucial aspects for Case-Based Recommenders to work effectively?
    

  

2. How do users specify their requirements in Constraint-Based Recommender Systems?
    

  

3. What is the role of similarity metrics in retrieving meaningful items?
    

  

4. What is the purpose of compound critiques in recommendation systems?
    

  

5. In what types of domains do Knowledge-Based Recommender Systems work well?
    

  

6. What is the process involved in returning relevant results in a constraint satisfaction problem according to the document?
    

  

7. What are the three different types of critiques mentioned in the document?
    

  

8. How do users often realize their requirements in the context of critiquing methods?
    

  

9. What is the significance of a knowledge base in Knowledge-Based Recommender Systems?
    

  

10. What is the main challenge addressed by Knowledge-Based Recommender Systems?
    

  

11. What is the role of additional rules in Constraint-Based Recommender Systems?
    

  

12. What is the importance of the proper design of similarity metrics in recommendation systems?
    

  

13. How do critique methods set the right exploration goal in Case-Based Recommenders?
    

  

14. What are the implications of users not being able to state their requirements exactly in the initial query when using critiquing methods?
    

  

15. Explain the role of similarity metrics in the context of Case-Based Recommenders and why they are crucial for effective recommendations.
    

  

16. Discuss the significance of compound critiques in the recommendation process and how they differ from simple critiques.
    

  

17. What challenges do Knowledge-Based Recommender Systems address, and how do they function in domains with limited data?
    

  

18. Describe how the process of returning relevant results is structured as a constraint satisfaction problem and the steps involved.
    

  

Ringrazio il libro "Recommender Systems: The Textbook" di [Charu C. Aggarwal](https://www.amazon.it/Charu-C-Aggarwal/e/B00E6PGCPM/ref=dp_byline_cont_book_1) per le 3 immagini sulle critiques. Non avevo proprio voglia di ricrearle da zero.