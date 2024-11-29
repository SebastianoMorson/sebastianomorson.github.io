---
layout: post
title: RECSYS Lecture 4 - Content-based filtering
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
categories: article

---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_8eab47fc0e164a3c819a993bef52701d~mv2.png/v1/fill/w_386,h_248,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_8eab47fc0e164a3c819a993bef52701d~mv2.png)

Eccoci qua, sembrava volessi fare un sacco di lezioni su metodi di collective filtering, invece sono già finiti. Ora si parla del secondo metodo di filtering: il content-based filtering.

  

  

  

  

Idea del metodo:

Usiamo le caratteristiche degli item valutati da un utente per prevedere le valutazioni che farebbe su nuovi items.

  

La cosa interessante di questo approccio è proprio il fatto che pone al centro del problema un singolo utente e l'analisi degli item che ha valutati, ma senza preoccuparsi di cosa e come hanno valutato gli altri utenti.

In parole povere, se Bob ha valutato 3 film, il sistema di raccomandazione content-based userà queste valutazioni per suggerire nuovi film, ma è ininfluente se Alice ha visto film simili o se Carlos è il migliore amico di Bob. Quindi studierà ad esempio di che tipo di film si trattano, il loro titolo, la loro durata, le recensioni fatte dall'utente e magari i commenti fatti dagli utenti.

  

Il metodo content-based si basa sui seguenti passaggi chiave:

- preprocessing e feature extraction
    
- feature selection
    
- addestramento del modello
    
- testing usando dati di training e di test
    

e utilizza due fonti di dati principali:

- la descrizione degli oggetti
    
- la descrizione del profilo degli utenti (ad esempio tempi di visualizzazione, numero di click, elementi osservati,...)
    

  

## Preprocessing

Un passaggio chiave del metodo content-based è quello che prevede di manipolare le informazioni che si hanno sugli items. Questo passaggio è necessario per ottimizzare la fase di addestramento. Ma perche?

- perchè le informazioni sugli items possono essere tante di diversa natura (categoriali, testuali, binarie, continue, ...). Ad esempio l'utente potrebbe aver lasciato delle recensioin ai film che ha guardato oppure potremmo avere la durata del film, che è espressa in secondi o in minuti, etc.
    
- perchè possono esserci informazioni ridondanti che aumentano la complessità spaziale e temporale (sinonimi, parole prive di significato, errori ortografici, ...)
    
- perchè non tutte le caratteristiche possono essere rilevanti allo stesso modo (il titolo del film potrebbe non essere rilevante tanto quanto la sua durata. Tendenzialmente se scegli di vedere o non vedere un film lo fai in base al titolo oppure alla durata del film? Se scarti un film è perchè si chiama in modo poco accattivante o perchè dura 5 ore?)
    

  

In generale il preprocessing serve a garantire la bontà dei dati che poi serviranno per addestrare il modello che calcolerà le previsioni.

  

Userò ora questo esempio di recensione per mostrare quali sono i passaggi più comuni quando si manipolano dati non strutturati e ricchi:

  

1. #### Togliamo le stop words
    

Avere un testo lungo e complesso non è bello. I simboli di punteggiatura e gli articoli non contengono informazioni utili, quindi possiamo eliminarli.

  

2. ### Stemmatization
    

Nella recensione, che senso ha tenere tutta la parola "repellente" quando posso tenere semplicemente "repel"? Non è importante la parola in sè, ma piuttosto il significato che gli attribuiamo e la posizione nella frase. Se un magrebino appena arrivato in Italia cerca di venderci i suoi tappeti di lana mentre siamo sulla spiaggia ad agosto, non è che non capiamo quello che ci vuole dire. Sbaglierà le parole, ma il significato lo capiamo comunque. Lo stesso vale in questo caso. Creare, per così dire, un nuovo vocabolario più corto, composto dai prefissi comuni di parole semanticamente simili semplifica la struttura sintattica del nostro testo. Ad esempio se il nostro testo sta parlando di giochi, invece di tenere in memoria parole singole come giocare, giochiamo, giochino, giochetto, giocattolo, è meglio sostituirle tutte con il loro prefisso comune "gioc".

Questa cosa non vale sempre, a volte è meglio tenere la forma estesa, ma in molti contesti è più comodo raggruppare e abbreviare.

  

3. ### Phrase extraction
    

Non è detto che le parole separate da uno spazio siano da considerare come parole singole. Può essere che due parole spaziate siano in realtà un'unica parola. Ad esempio "buona sera" (a volte si scrive così, a volte "buonasera") non è da leggere come "buona" "sera", ma come "buonasera". Capito il concetto?

  

### Feature extraction

Una volta rimosse le cose che non ci piacciono e compresso quelle che si presentano spesso, bisogna fare in modo di estrarre ciò che ci interessa. Per farlo è possibile sfruttare informazioni implicite o esplicite del testo. Ad esempio attraverso tecniche di sentiment analysis potremo capire dalla recensione se è di tipo negativo o positivo. Se l'autore sta imprecando contro una spazzola per le bambole che non ha la funzione di messa in piega potremo categorizzare la recensione come "negativa", mentre al contrario se sta elogiando il nuovo modello di Iphone potremo assegnare alla recensione un valore "positivo". Quindi poi, in fase di addestramento saremo in grado di capire attraverso questa informazione se consigliare o meno una certa categoria di prodotti all'utente. Tutto fa brodo.

  

Potremo anche estrarre informazioni esplicite, come la presenza di parole chiave. Ad esempio "mi piace" o "bello" potrebbero indicare che l'utente ha apprezzato un certo prodotto, quindi la presenza all'interno della recensione di queste parole potrebbe essere usata per discriminare un prodotto piuttosto che un altro.

  

Al termine della feature extraction avremo quindi un insieme di features associate ai prodotti, che saranno molto probabilmente di tipo diverso.

### Feature selection

Come anticipavo, non è detto che tutte le features siano necessarie. Alcuni attributi dell'oggetto infatti potrebbero essere ridondanti con altri. Ad esempio se dobbiamo consigliare dei prodotti legati alla tecnologia, è ovvio che se un prodotto è più nuovo costerà anche di più. Quando due attributi sono correlati non è necessario tenere entrambi, ma solo uno dei due. Applicando questa idea è possibile ridurre il numero di features da analizzare, migliorando previsioni e performance. Il problema è: anche se ho ridotto il numero di features, chi mi garantisce che siano veramente informative?

  

#### Gini Index

Se cercate l'indice di Gini online, probabilmente vi comparirà una formula statistica usata in economia, quindi seguite il mio ragionamento per avere la strada un po' più spianata. Altrimenti se volete fare le cose fatte bene seguite il link e studiatevelo, perchè non so se scriverò un articolo su quello.

  

Alloraaa, partiamo con fare un attimo di brainstorming: avevamo delle informazioni testuali, le abbiamo filtrate e abbiamo estratto delle features. Ora ci interessa capire quali sono interessanti e quali no.

  

Immaginiamo di avere una tabella come questa, che rappresenta i film visti e piaciuti al nostro Bob:

|   |   |   |   |   |
|---|---|---|---|---|
|Film|Genere|Anno di uscita|Regista|Votazione|
|Item 1|Horror|2022|Beppe Locapo|1(piace)|
|Item 2|Action|2020|Beppe Locapo|0(non piace)|
|Item 3|Horror|2000|Raffaella Gnigni|1|
|Item 4|Horror|1995|Durov Patchok|1|

Quello che possiamo subito notare è che ci sono tanti film Horror , gli autori e le date di uscita sono diverse, i voti sono tre 1 e uno 0.

A rigor di logica direi che a Bob piacciono i film horror, ma non gliene frega nulla di quando sono usciti e di chi è il regista.

  

L'idea dell'indice di Gini è la stessa. Consideriamo una features w e calcoliamo la frazione degli items che sulla parola w hanno ricevuto una votazione i. Assumiamo questa frazione di items come pi(w).

L'indice di Gini si calcola con la formula seguente

![](https://static.wixstatic.com/media/4959fd_73b29e694b6341eba407b6ecef9dcfa7~mv2.png/v1/fill/w_360,h_93,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_73b29e694b6341eba407b6ecef9dcfa7~mv2.png)

e se assume un valore prossimo a 0 significa che la feature è molto significativa, altrimenti no. Questo perchè se ci sono tanti film valutati "horror" allora avremo un grande valore di pi(w) e quindi avremo un basso numero una volta sottratto a 1. "Genere" avrà indice di Gini molto basso, difatti è molto importante per capire cosa piace a Bob. Al contrario "data di uscita" avrà indice di Gini molto alto essendo che è tanto variabile.

  

Nell'esempio precedente si può osservare che:

Gini("horror") = 1- (3/4)**2 = 0.43

Gini("action") = 1- (1/4)**2 = 0.93

Gini("2000") = 1-(1/4)**2 = 0.93

Gini("Beppe Locapo") = 1 - (1/2)**2 - (1/2)**2 = 0.5

  

Chiaramente sono valori calcolati su una scoring matrix di piccole dimensioni. La voce "horror" si è rilevata la più rilevante, e quindi il genere è una feature particolarmente significativa.

  

#### Entropy

Se vogliamo capire se un valore è informativo oppure no, la teoria dell'informazione ci suggerisce alcune nozioni utili. Non ne conosco tante, ma sicuramente l'entropia è una di queste. La formula è piuttosto semplice. Se vuoi imparare più cose sull'entropia dovrei pubblicare qualcosa a breve.

![](https://static.wixstatic.com/media/4959fd_8aa6492511a74073803301e92a66e8f1~mv2.png/v1/fill/w_488,h_86,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_8aa6492511a74073803301e92a66e8f1~mv2.png)

  

## Nearest Neighborhood method

Il metodo Nearest Neighborhood serve per determinare quali elementi sono più simili a quello target.

Per prima cosa vengono creati due insiemi di dati:

- training set: contiene le valutazioni già note
    
- test set: contiene gli elementi non valutati
    

Per chi già mastica qualcosa di AI appena ha letto "training" e "test" set avrà pensato "okay, quindi ci sarà una fase di training in cui addestro il modello e una fase di training in cui verifico che non stia overfittando". NO. Non c'è nessun tipo di apprendimento. Il training set contiene soltanto le valutazioni già note e le predizioni dei valori del test set verrà calcolato misurando la similarità degli elementi del test set rispetto a quelli del training set. Non aggiorno nessun parametro, nessun peso, niente di niente! Non abbiamo a che fare con una neural network! NNo NNo NNoo!

  

#### Cosine similarity

Per capire se due oggetti sono simili oppure no, utilizziamo la cosine similarity. La formula non è complicata

![](https://static.wixstatic.com/media/4959fd_43eb6655967949819a9fd2be90f3d617~mv2.png/v1/fill/w_506,h_270,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_43eb6655967949819a9fd2be90f3d617~mv2.png)

Viene applicato questo procedimento per ogni coppia di elementi, così da ottenere una tabella delle similarità. Una volta ottenuta si considerano i k elementi più simili, che rappresenteranno le previsioni dell'utente.

  

### Problema della complessità

Il problema di questo metodo è la complessità. Difatti per ogni elemento del training set è necessario calcolare la somiglianza a ciascun item del test set. Questo metodo è piuttosto oneroso, perchè se i film sconosciuti sono tanti, la complessità aumenta notevolmente.

![](https://static.wixstatic.com/media/4959fd_6b4a976fafa04a7aac614198dda70520~mv2.png/v1/fill/w_296,h_49,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_6b4a976fafa04a7aac614198dda70520~mv2.png)

  

  

#### Abbassiamo la complessità con il clustering

Usando il clustering possiamo ridurre la complessità del nostro problema. Quello che facciamo è raggruppare gli elementi del test set in cluster di elementi simili, per poi prendere il centroide del cluster e confrontare quello con i dati di training. In questo modo riduco notevolmente l'insieme dei dati di test.

  

### Collaborative filtering simulato da un modello content-based

È possibile fare in modo che un modello content-based simuli un modello collaborative filtering.

Per farlo è sufficiente incrementare il numero di attributi associato a un certo item inserendo delle voci come "Alice-like" oppure "Alice-unlike".

terminator: John-Like, Alice-Like, Tom-Dislike, ... 
alien: John-Like, Peter-Dislike, Alice-Dislike, Sayani-Like, ... ...

In questo modo quando il sistema apprenderà quali sono gli item che piacciono a Bob non utilizzerà soltanto le features dell'item o le caratteristiche del profilo di Bob, bensì sfrutterà anche le features aggiuntive che indicano quello che è piaciuto agli altri utenti. In questo senso il modello potrebbe apprendere che Bob ama gli items che piacciono ad Alice (o che non le piacciono), pur non avendo usato esplicitamente un modello collaborativo.

  

### Possibili domande

1. What are the two main sources of data used in content-based recommender systems, and how do they contribute to the recommendation process?

Le due fonti principali di dati sono:

- descrizioni degli items (ad esempio descrizioni date dai produttori, recensioni degli utenti, trama, ... )
    
- descrizioni dei profili degli utenti (click rate, visualizzazioni, valutazioni, ...)
    

  

2. How does the Gini Index work in feature selection?

L'indice di Gini serve per misurare l'importanza delle features. Viene calcolato come la differenza tra 1 e la sommatoria del quadrato delle frazioni di elementi in cui compare una parola w su ogni valutazione i. Se il Gini index per una parola w è circa 0 significa che tale parola è fortemente informativa, viceversa se è circa 1 indica che la parola non lo è.

  

L'indice di Gini lavora con valutazioni binarie, ordinate e intervalli finiti, quindi non con valutazioni continue. Questo è motivato dal fatto che la sommatoria nella formula ha bisogno di un valore di terminazione.

  

3. What is the role of user feedback in Content-Based Recommender Systems?

Il feedback dell'utente serve come sorgente di dati, in quanto feedback impliciti (numero di click, numero di visualizzazioni, etc.) e feedback espliciti (like, commenti, recensioni,...) accrescono il profilo utente e quindi le features che possono essere usate dai modelli content-based per offrire raccomandazioni più precise.

4. What is the significance of feature representation and cleaning in the context of recommender systems?

Diminuire la complessità

5. How do content-based systems provide explanations for their recommendations?

  

8. Describe the process of nearest neighbor classification and how it predicts ratings for items in the test set.

Il nearest neighborhood classification è un metodo che calcola le previsioni di scoring sugli elementi sfruttando le rappresentazioni vettoriali dei dati ottenuti dalle due sorgenti di dati citate in precedenza. Una volta calcolata la rappresentazione vettoriale il metodo usa la cosine similarity per predirre uno score. Gli elementi del training set vengono usati per prevedere gli score dei test set e alla fine vengono consigliati all'utente i k elementi che hanno ottenuto lo score più alto.

  

9. What is the cold-start problem in recommender systems, and how do content-based models help address it?

Il cold-start nei sistemi content-based è mitigato dal fatto che chi produce gli items generalmente produce anche una descrizione dell'item stesso, quindi tali dati possono essere usati per eseguire previsioni

  

11. What role does domain-specific knowledge play in movie recommendations, and which features are considered more important?

  

12. How do content-based models utilize user ratings in collaborative filtering?

I modelli content-based possono simulare i metodi collaborativi usando le valutazioni fatte dagli altri utenti come features degli items. Ad esempio se Bob ha valutato Titanic e l'ha fatto anche Alice, il profilo di Bob potrebbe contenere l'item <Titanic, drammatico, DiCaprio, "#navi che affondano", "piace ad Alice">. A questo punto a Bob potrebbero essere consigliati altri film che hanno la voce "piace ad Alice".

  

13. What is the significance of the Cosine similarity function in nearest neighbor classification?

Misura la distanza tra vettori in uno spazio.

  

14. What are the implications of not using ratings from other users in content-based recommender systems?

  

15. What is the primary focus of the black composition in web page recommendations?

  

16. What is the purpose of preprocessing and feature extraction in recommender systems?

  

17. How do content-based recommender systems provide interpretable predictions to users?

  

19. What are the challenges associated with feature representation and cleaning in recommender systems?

  

  

21. Discuss the implications of using content-based models for collaborative filtering in terms of user preferences and item attributes.

  

23. Describe the process and significance of feature representation and cleaning in the context of recommender systems.

  

24. What are the main components of a content-based recommender system, and how do they interact to provide recommendations?

  

25. Analyze the role of user profiles in content-based recommender systems and their impact on addressing the cold-start problem.