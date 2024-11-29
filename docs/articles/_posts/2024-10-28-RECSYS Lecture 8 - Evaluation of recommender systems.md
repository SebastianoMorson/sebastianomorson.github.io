---
layout: post
title: RECSYS Lecture 8 - Evaluation of recommender systems
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
---
<!--more-->
![](https://static.wixstatic.com/media/4959fd_7d6a4813325a494c82a8213d7785a73c~mv2.png/v1/fill/w_380,h_214,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_7d6a4813325a494c82a8213d7785a73c~mv2.png)

Se fino adesso abbiamo cercato di capire come creare un sistema di raccomandazione, ora l'obiettivo è capire se il sistema di raccomandazione che abbiamo creato fa schifissimo oppure è perfetto per rendere dei perfetti imbecilli tre quarti della popolazione mondiale...*colpi di tosse* TikTok *colpi di tosse*...*colpi di tosse* Instagram *colpi di tosse*...

  

Ci sono due regole semplicissime da seguire:

**Prima regola dell'evaluation:** non si parla dell'evaluation.

**Seconda regola dell'evaluation**: se devi parlare dell'evaluation non puoi parlare di una singola metrica di evaluation.

La prima regola diciamo che è per nascondere il problema sotto il tappeto (vige la regola delle dimostrazioni matematiche, ossia "perchè chiedersi se e come una cosa funziona?")

  

La seconda regola invece è per chi vuole fare proprio le cose fatte bene.

In realtà non è una vera regola, ma piuttosto una cosa fondamentale da ricordare. Quando si vuole valutare un recommender system è sbagliatissimo pensare di usare una sola metrica per valutare se il sistema funziona bene o male, ma piuttosto dobbiamo considerare vari aspetti fondamentali che sono spiegati di seguito.

  

### Accuracy e Coverage

La metrica di accuracy la conosciamo un po' tutti, misura quanto i nostri risultati si discostano da quelli reali. Se di 10 items raccomandati solo 3 sono stati ritenuti interessanti dal nostro utente, direi che la accuracy è piuttosto bassa...

  

Il problema è che questa metrica non è sufficiente a dirci se il sistema funziona bene. Immaginiamo di avere un sistema che ha un'accuracy molto alta che effettua raccomandazioni sui film.

Immaginiamo la tabella dei ratings sia una cosa di questo tipo:

|   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|
||L'era glaciale|Hit|Una pallottola spuntata|Le pagine della nostra vita|Chiamami pasticcino alla crema|Forrest Gump|L'esorcista|Alien|
|Bob|-|8|-|2|2|-|-|9|
|Alice|8|3|-|9|9|8|2|4|
|Giangibimba|9|2|9|7|4|9|4|3|
|Mariolindo|8|5|9|2|5|9|6|2|
|Pierlinda|8|03|8|7|6|10|3|5|

Come possiamo vedere ci sono dei film valutati molto positivamente dai nostri utenti: L'era glaciale, Una pallottola spuntata, Forrest Gump. Sono dei film famosissimi e che hanno riscosso un successo generale, indipendentemente dai gusti degli utenti.

  

Immaginiamo ora che il nostro sistema debba fare una raccomandazione di 3 elementi a Bob, a cui sappiamo piacere il genere horror, difatti ha valutato molto positivamente Hit e Alien. Il nostro sistema applica un metodo collaborativo basato su utenti. Invece di suggerirgli il film "l'esorcista" che sarebbe perfetto con i suoi gusti, il sistema gli suggerisce i 3 film L'era glaciale, Una pallottola spuntata, Forrest Gump. L'accuracy del sistema è molto alta, difatti a Bob piaceranno questi film, ma non erano dei film che corrispondono ai suoi gusti. Non c'è "coverage", ossia

- una certa porzione di items non vengono suggeriti
    
- oppure non si riesce a dare suggerimenti a una certa porzione di utenti
    

La popolarità di certi film finisce quindi per "avvelenare" la metrica di accuracy portandola a valori più alti di quelli che invece sono i valori di coverage.

  

Perciò le cose da tenere a mente sono:

- l'accuracy misura soltanto se la predizione è corretta
    
- la coverage misura se tutti gli utenti ricevono suggerimenti consoni
    
- la coverage misura se tutti gli items sono suggeriti
    
- la popolarità di certi elementi può abbassare la coverage del sistema
    
- gusti particolari degli utenti possono abbassare la coverage del sistema
    

  

### Confidence e Trust

Andiamo spediti,

- la **confidence** misura quanto il **sistema** si fida delle proprie raccomandazioni.
    
- la **trust** misura quanto l'**utente** si fida delle raccomandazioni che riceve.
    

  

Se un sistema ha un'alta confidence significa che è sicuro che le proprie raccomandazioni piaceranno all'utente. Userà quindi la sua confidence per suggerire un certo elemento piuttosto che un altro.

Se un utente ha alta trust significa che darà più facilmente per scontato che l'item suggerito gli piacerà.

  

Ottenere un alto livello di trust è fondamentale, perchè permette di indurre l'utente a passare più tempo sulla piattaforma (ad esempio su Netflix se so che i film raccomandati mi piacciono sempre un sacco, quando mi capiterà un film che non mi ispira fiducia lo guarderò comunque perchè Netflix non sbaglia mai, e Netflix nel frattempo ci guadagna un sacco di grana con le pubblicità).

  

La trust è misurabile attraverso feedback dell'utente, mentre la confidence può essere comunicata all'utente ad esempio fornendo le motivazioni del perchè un certo elemento è stato suggerito.

  

### Robustness e stability

La robustezza di un sistema di raccomandazione e la sua stabilità indicano quanto il verificarsi di particolari situazioni può compromettere la qualità delle raccomandazioni. Da un buon sistema ci aspettiamo che attacchi come l'iniezione di valutazioni errate, non inficino significativamente le performance del modello usato.

  

### Scalability

In un sistema di raccomandazione ci aspettiamo che il numero di elementi o utenti cresca nel tempo. In particolare, se siamo i CEO di Spotify è auspicabile che il numero di utenti cresca ogni anno di un certo numero e che gli artisti non smettano di fare nuove canzoni.

Un buon sistema di raccomandazione deve quindi considerare aspetti come:

- tempo per ottenere una previsione
    
- tempo per addestrare il modello
    
- memoria richiesta dal modello
    

  

  

### Serendipity

La serendipity rappresenta il livello per cui un utente valuta positivamente un elemento che non si aspettava di ricevere come un suggerimento. La serendipity è una metrica fondamentale quando si parla di exploration. Un alto livello di serendipity indica che l'esplorazione di nuovi elementi ha successo.

### Novelty

La novelty è una metrica usata per misurare quanto un utente si aspetta di trovare un certo elemento tra le sue raccomandazioni. Un utente che guarda tanti film horror o di un particolare attore si aspetta che quando esce un nuovo film horror o un nuovo film in cui compare il suo attore preferito, lo riceva tra le raccomandazioni.

  

La differenza tra novelty e serendipity è quindi che nel primo caso l'utente rimane positivamente colpito da un elemento che si era perso, mentre nel secondo caso l'utente rimanere positivamente colpito da un elemento che non si aspettava potesse piacergli.

  

### Diversity

Una caratteristica auspicabile in un buon sistema di raccomandazione è la diversity, ossia la capacità del sistema di generare raccomandazioni diverse tra loro. Se un sistema si concentra su elementi molto simili tra loro è più a rischio di fornire tante raccomandazioni errate. Immaginiamo un sistema che suggerisce a Bob 10 elementi tutti molto simili. Se Bob valuta negativamente uno di questi elementi, è probabile che valuterà negativamente anche tutti gli altri, perchè troppo simili al primo valutato.

Al contrario, se i 10 elementi sono pertinenti, ma presentano delle differenze significative tra loro, è più probabile che tra questi 10 elementi si troverà un buon numero di items che otterranno una valutazione positiva (assumendo che i 10 elementi siano stati scelti in maniera intelligente e non casuale).

  

  

## Ranking Evaluation

Non è importante valutare solo la bontà delle valutazione offerte, ma anche se l'ordine di presentazione degli elementi è corretto. Solitamente gli utenti guardano le pagine dall'alto verso il basso e si stufano velocemente di scorrere una lista di tanti elementi, questo significa che un elemento in cima alla lista verrà probabilmente individuato e valutato per primo rispetto a uno che si trova in centesima posizione.

  

È quindi fondamentale verificare se l'ordine degli elementi è fatto in maniera consona.

Le metriche per questo tipo di valutazione sono:

  

### Evaluation via correlation

L'idea chiave di questa strategia è: controlliamo se c'è una correlazione forte tra la posizione nella classifica e la valutazione data dall'utente. Se questa correlazione è bassa significa che l'elemento non si trova nella giusta posizione della classifica, se invece la correlazione è alta significa che l'elemento è posizionato correttamente.

  

Per applicare questa idea usiamo la correlazione di Kendall.

  

#### Kendall correlation

Immaginiamo di avere una lista dei ground truth costruita così

|   |   |
|---|---|
|Predizione|Item|
|7|C|
|5|A|
|3|B|

e una lista delle predizioni che è così:

|   |   |
|---|---|
|Predizione|Item|
|5|A|
|3|B|
|7|C|

![](https://static.wixstatic.com/media/4959fd_8ecd4fd6e970435e999187010dc1b9f1~mv2.png/v1/fill/w_434,h_386,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_8ecd4fd6e970435e999187010dc1b9f1~mv2.png)

  

Qualcuno potrebbe storcere il naso e dire "ma dove sta scritta la posizione nella classifica"? Non è scritta esplicitamente, ma quella ideale viene rappresentata dalle ground truths.

  

La correlazione di kendall costruisce le coppie (C,A), (C,B), (A,B), (A,C), (B,A), (B,C) e valuta per ciascuna coppia se i valori di ciascuna coppia sono nel giusto ordine.

Ad esempio nella nostra predizione

|   |   |   |
|---|---|---|
|Predizione|Item||
|5|A||
|3|B|+|
|7|C|-|

|   |   |   |
|---|---|---|
|Predizione|Item||
|5|A||
|3|B||
|7|C|-|

Come si può vedere abbiamo ottenuto che 2 elementi sono discordanti rispetto alla loro valutazione, mentre solo 1 è concorde.

Quindi a questo punto calcoliamo il coefficiente di tau come

![](https://static.wixstatic.com/media/4959fd_1b46d07097874e4d8bca3194ddbb2362~mv2.png/v1/fill/w_592,h_91,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1b46d07097874e4d8bca3194ddbb2362~mv2.png)

  

  

### Evaluation via utility

Un'altra importante metrica è l'utilità. L'utilità misura quanto un metodo di ranking è ritenuto utile dall'utente.

  

Per calcolare questo valore di utilità viene valutata la combinazione di ground truths e ranking. Se un metodo di ranking ha ottima utilità significa che le previsioni sono ordinate coerentemente nella ranking list in modo da massimizzare l'utilità percepita dall'utente , perciò gli elementi valutati in maniera più positiva si trovano nella porzione superiore della ranking list.

  

Le metriche utility-based più usate sono:

- DCG (Discounted Cumulative Gain)
    
    - è uguale alla sommatoria delle utilità dei singoli elementi
        
    - gli elementi che si trovano più in basso nella ranking list partecipano alla valutazione in maniera minore rispetto agli elementi nelle posizioni superiori.
        
        Questo viene fatto per attribuire maggior peso a elementi teoricamente più affini ai gusti dell'utente
        
- NDCG (Normalized DCG)
    
    - è la versione normalizzata del DCG e viene usata quando si utilizzano dataset diversi (ad esempio, valutare diversi algoritmi di raccomandazione su utenti diversi o differenti set di dati)
        

La NDCG si vede molto spesso all'interno dei paper scientifici, quindi meglio approfondirla un attimo.

#### Normalized Discounted Cumulative Gain (NDCG)

La NDCG è una metrica usata per valutare l'utilità di una ranking list.

Per capire NDCG bisogna prima definire i concetti di

- CG
    
- DCG
    
- IDCG
    

La CG (cumulative gain) indica qual è la gain della nostra sequenza di rating. In poche parole equivale alla somma delle gain ottenute dagli elementi della nostra ranking list.

Il problema della cumulative gain è che non considera il fatto che i k-top elementi seguono un ordine ben preciso, posizionando in cima gli elementi di cui il modello ha più certezza possano piacere all'utente, e nelle ultime quegli elementi di cui il modello ha meno certezza.

Ovviamente se il modello sbaglia a suggerire il primo elemento, è più grave rispetto a sbagliare il suggerimento dell'ultimo. Per questo si utilizza la DCG che penalizza basse gain sugli elementi della ranking list di un fattore che dipende dalla posizione nella lista.

![](https://static.wixstatic.com/media/4959fd_c59da56ea987473e9171627e9fe61320~mv2.png/v1/fill/w_294,h_177,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c59da56ea987473e9171627e9fe61320~mv2.png)

Infine la IDCG serve per calcolare la gain ottenibile se adottassimo l'ordinamento ideale degli elementi.

![](https://static.wixstatic.com/media/4959fd_be709f8804e64b1d9971bdbc3eba21c2~mv2.png/v1/fill/w_422,h_134,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_be709f8804e64b1d9971bdbc3eba21c2~mv2.png)

Da questi parametri si deriva il valore di NDCG come il rapporto tra DCG e IDCG.

![](https://static.wixstatic.com/media/4959fd_fa8ba8b89caf4addb23a076760cc083e~mv2.png/v1/fill/w_458,h_215,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_fa8ba8b89caf4addb23a076760cc083e~mv2.png)

Se siamo interessati a calcolare la NDCG solo per i primi k elementi della ranking list, possiamo usare NDCG@k, che non fa altro che considerare nella formula soltanto i primi k elementi.

  

### Evaluation via ROC (Receiver Operating Characteristics)

Se in ascolto c'è qualcuno addetto ai lavori, saprà già cos'è una ROC curve e avrà fatto 2+2.

Per tutti gli altri, una ROC curve è una cosa di questo genere

![](https://static.wixstatic.com/media/4959fd_d1d9c5119e474fc297e08356fde72737~mv2.png/v1/fill/w_592,h_379,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_d1d9c5119e474fc297e08356fde72737~mv2.png)

La ROC curve permette di valutare il numero di corrette previsioni rispetto ai casi di falsi positivi. "Ma scusa, e in base a cosa diciamo che un elemento della ranking list è un falso positivo?". Ora ci arrivo.

  

Se abbiamo 10 items possiamo definire quali di questi elementi sono positivi stabilendo che lo sono quelli che hanno ricevuto, ad esempio, una valutazione maggiore a 7. Tutti questi elementi sono potenziali suggerimenti per il nostro utente. A questo punto, applicando la stessa idea agli elementi della ranking list, è possibile confrontare gli elementi del ground truth positivi con quelli della ranking list e contare quanti elementi pertinenti sono stati considerati e quanti no.

  

#### Quali sono le differenze tra ROC, Kendall correlation e NDCG?

La curva ROC e il calcolo della correlazione di Kendall sono due metriche che valutano equamente tutti gli elementi indipendentemente dalla posizione. Al contrario NDCG e la sua versione non normalizzata DCG danno più peso agli elementi in cima alla ranking list, basandosi sull'idea che una variazione su tali elementi influenza maggiormente l'utilità percepita dall'utente. Questa è la differenza più sostanziale. Tra ROC e Kendall c'è poi da dire che il primo metodo si interessa della posizione che occupa ciascun item suggerito, quindi si preoccupa dell'ordinamento della ranking list. Al contrario la ROC curve si preoccupa soltanto che non ci sia una forte presenza di elementi non pertinenti.

## Evaluation Gaming

Il problema dell'evaluation gaming è un problema che si capisce bene con un esempio (mi piacciono gli esempi):

facciamo finta che siamo un sistema di raccomandazione. Abbiamo tanti elementi da poter consigliare, ma ci accorgiamo che alcuni di questi hanno ricevuto tante interazioni da parte degli utenti. Potremmo quindi consigliare quelli perchè ritenuti i migliori, pur non essendo davvero rilevanti per l'utente a cui dobbiamo consigliare. Oppure potremmo ordinare la classifica delle raccomandazioni in base allo score ottenuto in fase di evaluation del ranking e quindi senza imparare davvero a ordinare correttamente gli items.

  

Questo è un problema, perchè è paragonabile all'overfitting di un classificatore: imparo talmente bene le metriche usate che finisco per sfruttare quelle invece che risolvere veramente il problema.

  

L'evaluation gaming è quindi la situazione in cui il sistema esegue delle raccomandazioni basandosi sulle metriche di valutazione utilizzate invece che dalle caratteristiche degli items suggeriti.

  

Per cercare di evitare di incorrere in questo problema, è necessario

- usare più metriche di valutazione contemporaneamente, così da rendere più complicato per il modello riuscire ad adattarsi a tutte le metriche.
    
- migliorare il numero di feedback impliciti ed espliciti, così da aiutare il modello a capire quali sono le reali preferenze dell'utente e quindi evitare che debba sfruttare nell'evaluation gaming
    
- considerare le valutazioni degli utenti a lungo termine, così da individuare se il consenso tende a diminuire o se si conforma su certi items
    
      
    

  

  

### Possibili domande

1. Explain how serendipity can be measured in recommendation systems and why it is important.
    

La serendipità è una metrica che misura la capacità di un sistema di proporre suggerimenti che riescono a stupire positivamente l'utente con items di cui potrebbe essere a conoscenza, ma che sicuramente non si aspettava potessero piacergli. La differenza rispetto alla novelty è che in quest'ultima metrica si misura quanto il sistema è in grado di suggerire items che si sà che piaceranno all'utente, ma di cui non sono a conoscenza (ad esempio l'ultimo film di Terminator a chi è piaciuto il primo Terminator)

La serendipità si può misurare come la frazione degli items che sono stati ritenuti utili e non ovvi.

  

2. Discuss the relationship between RMSE and the utility of items in a recommendation list.
    

Usare RMSE per misurare l'utility di un item in un sistema di raccomandazione non è una scelta saggia. RMSE non considera la posizione dell'item nella ranking list, di conseguenza non è una buona strategia di valutazione dell'utilità.

  

3. What are the key considerations in the experimental design of recommendation system evaluations?
    

  

4. Describe the differences between offline and online evaluation methods for recommendation systems.
    

La differenza tra questi due metodi è che l'offline evaluation viene effettuata su dati storici e può non modellare efficacemente il comportamento reale come invece fanno i metodi online di valutazione. Questi ultimi vengono svolti in contesti commerciali e "in esecuzione" e comprendono strategie come l'a/b testing. Le valutazioni online soffrono meno di bias rispetto ai metodi offline, ma richiedono un numero più grande di utenti. I motivi sono molteplici:

- perchè i comportamenti di utilizzo possono essere molto diversi tra gli utenti, di conseguenza se voglio misurare il tempo di utilizzo del mio sistema da parte dei miei utenti e ne ho 10, non posso aspettare che tutti e 10 abbiano usato l'applicazione. Se ne ho 1000 sarà molto più probabile che in una specifica giornata almeno 50/60 di loro stia usando la mia applicazione. Perciò un numero di utenti maggiore aiuta a ottimizzare i tempi
    
- perchè in un contesto reale non sono sicuro che i miei utenti interagiranno come voglio io con la mia applicazione. Se voglio misurare il numero di ricerche effettuate usando uno specifico form di ricerca, non ho modo di conoscere a priori quanti miei utenti effettueranno una ricerca, a meno che non dica loro di farlo (ma allora inficerei la bontà del mio esperimento)
    

  

5. What are the implications of the limitations of evaluation measures in recommendation systems?
    

  

6. Explain the relationship between an item's position in a recommended list and its utility to the user.
    

Se ho una lista di suggerimenti, la posizione degli items iniziale è la più importante perchè è quella a cui gli utenti prestano più attenzione. Un utente di fretta o che non vuole perdere troppo tempo nella ricerca, difficilmente esplorerà tutta la lista dei suggerimenti per cercare l'elemento desiderato, perchè si aspetta che nelle prime posizioni si trovino gli items più pertinenti. È quindi fondamentale valutare la ranking list considerando la loro posizione. Un metodo di valutazione basato su utility è NDCG, che penalizza maggiormente gli errori di raccomandazione effettuati in base alla loro posizione nella lista. Una ordinamento errato degli elementi nelle prime posizioni porterà ad ottenere una NDCG molto bassa rispetto a un errore di ordinamento degli elementi nelle ultime posizioni.

  

7. What is the role of accuracy metrics in the evaluation of recommendation systems, and what are some common types?
    
8. Discuss the importance of experimental design in the evaluation of recommendation systems.
    
9. What is the NDCG score, and how is it calculated using relevance scores of recommended items?
    
10. What is the purpose of using a temporal dimension in offline evaluation of recommendation systems?
    

Può servire a verificare se i gusti degli utenti cambiano frequentemente nel tempo e se il sistema si adatta ai cambiamenti.

11. How do multi-armed bandits and reinforcement learning relate to online evaluation of recommendation systems?
    
12. What challenges arise from the non-random nature of missing entries in a ratings matrix?
    
13. What is the impact of small changes in RMSE on the identities of top-rated items?
    
14. What is the difference between RMSE and MAE in terms of their sensitivity to outliers?
    
15. What is the significance of confidence and trust in the evaluation design of recommendation systems?
    
16. What is the basic idea behind evaluating ranking via utility in recommendation systems?
    
17. What are the general goals of evaluation design in recommendation systems?
    
18. How does the choice of training data affect the estimation of ratings in recommendation systems?
    
19. What are the limitations of evaluation measures in recommendation systems?
    
20. What is the basic idea behind evaluating ranking via utility in recommendation systems?
    
21. What are the general goals of evaluation design in recommendation systems?
    
22. What are the limitations of evaluation measures in recommendation systems?