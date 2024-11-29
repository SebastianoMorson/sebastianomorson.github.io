---
layout: post
title: RECSYS Lecture 2 - Neighborhood-based collaborative filtering
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_a6f89a18d92048f79c1e9b46bc573455~mv2.png/v1/fill/w_280,h_186,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a6f89a18d92048f79c1e9b46bc573455~mv2.png)
## I modelli Neighborhood-based collaborative filtering

Nel precedente articolo ho parlato di modelli di raccomandazione collaborativi, ossia quei modelli che sfruttano i feedback impliciti o espliciti di altri utenti, per suggerire a un utente le cose che più gli piacciono. L'idea è che se sono Bob e mi piacciono certe cose, magari altri utenti hanno gli stessi gusti e posso trovare interessante quello che a loro interessa.

  

A partire da questa idea possiamo introdurre il metodo Neighborhood-based (anche chiamato "memory-based", poi capiremo perchè), ossia una tecnica di collaborative filtering in cui l'idea è quella di tenere traccia di tutte le valutazioni che gli utenti hanno fatto di tutti gli elementi e poi analizzarle. È un metodo vecchio, quindi non iniziate a lamentarvi del fatto che le cose potrebbero essere fatte meglio. Lo so anche io, piuttosto provate a pensare a come migliorare le cose invece che lamentarvi. Per Dio.

Per prima cosa quindi il metodo Neighborhood-based costruisce una tabella come questa:

  

|   |   |   |   |   |
|---|---|---|---|---|
||Item 1|Item 2|Item 3|Item 4|
|Bob (user 1)|3|2|4|2|
|Alice (user 2)|4|7|-|2|
|Carl (user 3)|-|-|1|3|
|Markus (user 4)|3|6|6|2|
|Frank (user 5)|-|7|4|8|
|Dafne (user 6)|5|3|2|6|
|Flora (user 7)|1|1|1|1|

  

Analiziamo la tabella:

- alcuni utenti hanno votato usando range tra 2 e 7, altri utenti hanno range tra 4 e 8, e via così
    
- alcuni utenti sono particolarmente cattivi, come l'utente 7, che guarda caso è una donna ed è stata cattivissima in tutte le sue votazioni
    
- ci sono più utenti che items
    
- ci sono degli elementi che non sono stati recensiti
    

  

Teniamo a mente questi problemi, perchè tra un attimo capiremo perchè sono importanti da considerare.

  

Nel frattempo definiamo una rappresentazione generica per le tabelle come quella sopra.

![](https://static.wixstatic.com/media/4959fd_1c072e594094479398bc8129ed48e036~mv2.png/v1/fill/w_592,h_176,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1c072e594094479398bc8129ed48e036~mv2.png)

  

  

## User-based vs Item-based neighborhood

Ebbene i metodi neighborhood possono essere di due tipi:

- USER BASED: quelli che si concentrano sugli utenti simili a quello target
    
- ITEM BASED: quelli che si concentrano sugli items simili a quelli che piacciono al nostro target
    

La differenza è notevole, perchè il primo metodo metterà più enfasi sugli utenti simili, ma gli anni di indottrinamento scolastico ci hanno insegnato che le persone sono tutte diverse e quindi anche gli utenti potrebbero avere gusti super diversi, ma che in parte coincidono con quello del nostro utente target. A Bob potrebbero piacere i film horror, ma non i cartoni animati horror. Magari tra i nostri utenti quelli a cui piacciono gli horror amano anche i cartoni animati, quindi a Bob potrebbero venir suggeriti cartoni animati horror che gli fanno schifo.

  

Viceversa, il secondo metodo è più personalizzato, perchè pone l'enfasi sui gusti del nostro utente target, cercando elementi simili a quelli che piacciono al nostro target.

  

Facciamo che da ora in avanti Bob sarà il nostro utente target. Salutate tutti Bob.

  

### User-based Neighborhood collaborative filtering

Come abbiamo detto, nel modello user-based poniamo l'enfasi sul trovare utenti simili e poi suggeriamo elementi che piacciono ai k user più simili a Bob.

  

Per prima cosa quindi abbiamo bisogno di calcolare un valore di somiglianza tra utenti. Per farlo useremo la mitica correlazione di Pearson, una formula spesso usata in intelligenza artificiale che permette di ottenere il valore di correlazione tra due elementi, considerando

![](https://static.wixstatic.com/media/4959fd_7fea3395b71e40d88ba04b20a93056e2~mv2.png/v1/fill/w_592,h_382,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_7fea3395b71e40d88ba04b20a93056e2~mv2.png)

Consideriamo con x e come y due nostri utenti t (l'utente target) e q.

Assumiamo inoltre che la media dei valori sia indicata da µ come

  

![](https://static.wixstatic.com/media/4959fd_2bfb0ed13dcf4798808c86a16e550293~mv2.png/v1/fill/w_281,h_120,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_2bfb0ed13dcf4798808c86a16e550293~mv2.png)

Praticamente faccio la media di ciascuna riga e per ciascun utente.

A questo punto la nostra correlazione di Pearson diventa:

![](https://static.wixstatic.com/media/4959fd_cb8bb5d3c9b340caab55b3306f420e67~mv2.png/v1/fill/w_592,h_138,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_cb8bb5d3c9b340caab55b3306f420e67~mv2.png)

Quindi cosa facciamo ora con questa correlazione? Semplicemente la calcoliamo per ciascuna coppia (t, user). In questo modo so quale utente ha valutato in modo simile a t.

  

Quindi non ci resta che individuare i k utenti più simili a t, che non saranno altro che gli utenti con più alta correlazione.

  

Indichiamo con P(t,j) l'insieme dei k utenti più simili all'utente t nelle valutazioni dell'elemento j.

  

L'obiettivo adesso è prendere P(t,j) e per ciascuno dei suoi utenti individuare qual è la media pesata delle valutazioni. In questo modo si può capire quale valutazione mediamente otterrebbe quel particolare elemento j.

  

Il problema è che, analizzando la tabella iniziale, avevamo scoperto che le scale dei valori cambiano da utente a utente... accidenti.

Ci viene in soccorso la normalizzazione. Se normalizziamo la tabella prima di lavorarci sopra, i valori possiamo prenderli già belli puliti come piacciono a noi.

Utilizziamo quindi la mean-normalization per normalizzare i valori.

La formula è semplicissima

![](https://static.wixstatic.com/media/4959fd_cb548ac0348042188881a6ac02f0ef66~mv2.png/v1/fill/w_302,h_81,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_cb548ac0348042188881a6ac02f0ef66~mv2.png)

Perciò per calcolare la media pesata e rimuovere la normalizzazione è sufficiente calcolare la formula

![](https://static.wixstatic.com/media/4959fd_8be0c27ff96a4b5e8fdd16ca470efed4~mv2.png/v1/fill/w_592,h_139,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_8be0c27ff96a4b5e8fdd16ca470efed4~mv2.png)

Ora abbiamo un altro problema. Quando avevamo parlato di long-tail frequencies avevamo detto che nel caso ci siano elementi più recensiti di altri, questi saranno generalmente più apprezzati o più disprezzati. Ciò significa che in generale le persone, anche con gusti completamente diversi, tenderanno a schierarsi tutti da una parte. Perciò se io voglio trovare la somiglianza tra utenti, non posso dare troppo peso a questi items.

Ecco che ci viene in soccorso la cosiddetta "inverse-user frequency", ossia un termine che inseriremo dentro la nostra formula della correlazione di Pearsonn per bilanciare il peso di questi fastidiosi elementi.

La inverse user frequency è calcolata come:

![](https://static.wixstatic.com/media/4959fd_0e90b39cab2c40059305fedcb77e1672~mv2.png/v1/fill/w_297,h_81,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_0e90b39cab2c40059305fedcb77e1672~mv2.png)

La formula finale per le correlazione diventa quindi:

![](https://static.wixstatic.com/media/4959fd_998da2535fda4a58bfaaeb4aee5edc71~mv2.png/v1/fill/w_592,h_134,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_998da2535fda4a58bfaaeb4aee5edc71~mv2.png)

That's it.

  

### Item-based Neighborhood collaborative filtering

Questo metodo, al contrario del precedente (user-based neighborhood collaborative filtering) che cercava di identificare utenti simili, si focalizza sul trovare items simili già valutati.

  

Per cominciare bisogna capire cosa vuol dire "item simile", ma ancora prima bisogna capire come rappresentare quindi un item in modo da confrontarlo.

  

Ebbene per descrivere un item useremo un vettore costruito a partire dalle valutazioni degli utenti. Ad esempio un item j verrà rappresentato dalle valutazioni fatte dagli utenti su quell'elemento. Il problema è che gli utenti potrebbero aver usato scale di valutazione diverse (magari c'è qualcuno che è più generoso con i valori, mentre altri più stitici). Perciò è necessario applicare la normalizzazione delle valutazioni degli utenti centrando sullo zero la media di tutte le righe.

  

Una volta ottenuta la rappresentazione, possiamo usare una metrica di correlazione come Pearson (nel caso siamo interessati alla correlazione di dati normalizzati), oppure come la cosine-correlation (che invece si concentra sulla direzione dei vettori rappresentanti gli elementi) per ottenere dei valori di correlazione tra l'elemento target j e gli elementi valutati da parte del nostro utente target t.

![](https://static.wixstatic.com/media/4959fd_561b8231c7cb40e99d92e56adfe60555~mv2.png/v1/fill/w_592,h_139,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_561b8231c7cb40e99d92e56adfe60555~mv2.png)

dove

![](https://static.wixstatic.com/media/4959fd_72273aa86c6a42169e8f1f9db49340db~mv2.png/v1/fill/w_415,h_72,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_72273aa86c6a42169e8f1f9db49340db~mv2.png)

  

  

  

  

  

Applichiamo questo procedimento a tutti gli elementi j che non sono ancora stati valutati da t e creiamo quindi un secondo insieme definito come

![](https://static.wixstatic.com/media/4959fd_c42c78840f7e4e238d99e9667a87d198~mv2.png/v1/fill/w_408,h_92,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c42c78840f7e4e238d99e9667a87d198~mv2.png)

  

  

  

  

  

  

Fatto ciò prendiamo i k items più correlati all'item j. Ma che valutazione diamo quindi all'item j? Consideriamo l'utente target t e l'elemento target t e ne calcoliamo lo score come:

  

![](https://static.wixstatic.com/media/4959fd_fccc9765f0a2431598d55c4397150c96~mv2.png/v1/fill/w_592,h_206,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_fccc9765f0a2431598d55c4397150c96~mv2.png)

  

### Considerazioni sulla complessità dei due metodi

Entrambi i metodi sono chiaramente piuttosto dispendiosi in termini di tempo. Nel caso dell'user-based model abbiamo una complessità O(m² · n) solo per calcolare la correlazione di Pearson, e cambia poco anche per l'item-based, dove abbiamo una complessità di O(n² · m) per calcolare la similarità tra tutti gli oggetti. Se poi vogliamo calcolare le predizioni, nel primo caso dobbiamo considerare tutti i k utenti simili, e individuare quale elemento è il migliore da consigliare, quindi abbiamo O(k·m) controlli da fare. Nel secondo metodo ne abbiamo O(k·n) perchè dobbiamo confrontare tutti i k items simili agli n elementi valutati dall'utente. La complessità in spazio almeno è per il primo metodo è un po' meglio, perchè abbiamo O(m) per l'user-based neighborhood e O(n) per l'item-based.

Tranquilli, metto una tabella così ci capiamo meglio.

  

|   |   |   |
|---|---|---|
|Complessità/Metodo|User based|item based|
|Tempo per calcolare le correlazioni|O(m² · n)|O(n² · m)|
|Tempo per calcolare le predizioni|O(k·m)|O(k·n)|
|Spazio|O(m)|O(n)|

Ma allora perchè dico che l'item based è meglio dello user based? Bè, perchè innanzitutto lo user-based è più rischioso a causa degli overlapping sulle valutazioni degli utenti, e poi perchè certe operazioni possono essere svolte in real-time e altre offline. In particolare se demandiamo le operazioni più costose alla fase offline, ci accorgiamo che nell'item-based method il numero di volte in cui dobbiamo ricalcolare la correlazione tra gli elementi non è poi così alto. Se immaginiamo di avere 1 milione di utenti e se ne aggiunge un altro, non serve ricalcolare le similitudini tra items, perchè quel singolo utente non farà mai la differenza, cosa che invece poteva accadere nello user-based method.

Facciamo un altro esempio ancora più pratico: siamo in mezzo a un gruppo di amici (ne abbiamo tanti, tipo tutti gli abitanti di Milano) e chiediamo "Titanic è simile a Naruto?", tutti mi diranno di no e anche se arriva un povero stronzo a dirmi "sì sono uguali", non è che gli credo (a meno che anche io sia un povero stronzo). Se invece nel gruppo di amici cerco un utente simile a me, cavolo ogni volta che si aggiunge un nuovo amico devo controllare se ha i gusti simili ai miei, cosa che faranno anche tutti i miei amici. Praticamente diventa un puttanaio a cercare di capire chi è simile a chi. Capito adesso?

  

  

## Possibili domande

1. What is the process of computing the Pearson coefficient in user-based neighborhood models?

  

2. What are the advantages of item-based methods compared to user-based methods in recommendation systems?

  

3. How does dimensionality reduction address issues in large sparse matrices?

4. What is the impact of the long tail distribution on rating frequencies in recommendation systems?

  

5. What is the role of clustering in improving the efficiency of neighborhood methods?

##   

6. Explain the process of obtaining mean-centered ratings in user-based neighborhood models and its significance.

  

7. Discuss the challenges faced by user-based collaborative filtering methods compared to item-based methods.

  

8. What are the implications of the long tail distribution on the profitability and prediction accuracy of items in recommendation systems?

  

9. Describe how dimensionality reduction techniques like PCA can help in addressing issues in collaborative filtering.

  

10. Analyze the computational complexity of user-based and item-based recommendation models and their implications for real-world applications.

  

11. User-based neighborhood models compute the Pearson coefficient only once for each user.

- True
    
- False
    

  

12. Item-based methods are more difficult to compute than user-based methods.

- True
    
- False
    

  

13. The long tail distribution indicates that most items are rated frequently.

- True
    
- False
    

  

14. Clustering can help improve the efficiency of neighborhood methods by reducing the number of peer groups.

- True
    
- False
    
      
    

15. Dimensionality reduction techniques like PCA do not address biases in large sparse matrices.

- True
    
- False
    

  

16. What is the primary purpose of computing the Pearson coefficient in user-based neighborhood models?

A) To find the closest users for each item

B) To categorize data into nominal scales

C) To rank items based on popularity

D) To reduce dimensionality of data

  

17. Which of the following is a characteristic of item-based methods compared to user-based methods?

A) More difficult to generate explanations

B) Less stable over time

C) Easier to compute a 'like factor'

D) More reliant on user anonymity

  

18. What does the long tail distribution imply about item ratings?

A) Most items are rated frequently

B) A small fraction of items are rated frequently

C) All items have equal ratings

D) Items in the long tail are less profitable

  

19. What is the effect of clustering on the efficiency of neighborhood methods?

A) Increases accuracy significantly

B) Reduces the number of peer groups

C) Eliminates the need for ratings

D) Makes computations more complex

  

20. What is a key benefit of dimensionality reduction techniques like PCA in collaborative filtering?

A) They increase the number of ratings

B) They help reduce bias in large sparse matrices

C) They eliminate the need for user ratings

D) They simplify the user-item matrix