---
layout: post
title: RECSYS Lecture 7 - Multi-armed bandit
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
categories: article

---
<!--more-->

![](https://static.wixstatic.com/media/4959fd_6f400693c6e6463893125e84fb252043~mv2.png/v1/fill/w_305,h_326,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_6f400693c6e6463893125e84fb252043~mv2.png)

Immaginiamo di essere in una sala slot. Abbiamo davanti un certo numero di slot machines (anche chiamate one-armed bandit).

Ogni slot machine ha una certa probabilità di farci vincere, ma chiaramente noi non sappiamo quale sia quella con le percentuali maggiori di vincita.

Per questo motivo iniziamo a provarle tutte, ma il gestore viene da noi e ci dice "guarda che non puoi mica metterti qua a studiare le mie macchinette, sennò ti caccio".

Il problema ora è: come faccio a capire quale delle macchinette è quella migliore in un numero massimo di tentativi?

E poi, una volta che sto usando una certa slot machine, mi conviene rimanere su quella e continuare a vincere bei soldoni, o spostarmi su un'altra che magari mi farà vincere ancora più soldi?

  

Questo problema descrive bene il problema del "exploration-exploitation", ossia il problema di capire se è più vantaggioso "exploitare" un metodo che sappiamo che ci soddisfa, oppure "explorare" e potenzialmente ottenre migliori benefici a lungo termine (se sei un fanatico dell'accademia della Crusca, lo so che è sbagliato e si dice "esplorare", continua pure a mangiare cibo per cavalli e vivere una vita triste ed insignificante).

  

Il problema exploration-exploitation è un fatto noto quando si parla di recommender systems. Un esempio pratico: siamo su Spotify (o Amazon, Netflix, ...) e ci viene suggerita una certa canzone rock, perchè ci piace quel genere li. Se continuiamo ad ascoltare rock, può essere che prima o poi venga meno la **serendipità,** ossia la capacità di sorprenderci, magari perchè non riceviamo più novità interessanti o perchè esploriamo tutti i brani di quel genere (magari siamo super fan di un genere prodotto da 5 persone di cui 4 sono morte).

  

Ecco quindi che ci serve un modo per inserire in qualche modo delle raccomandazioni che in qualche modo si discostino dai nostri gusti abituali o che non siano completamente attinenti, in modo da esplorare cose nuove.

Però c'è un però.

Chi ci dice che quello che stiamo suggerendo piaccia all'utente? Magari abbiamo a che fare con un utente anziano che non apprezza quando gli proponi cose nuove.

  

Ricordandoci dell'esempio iniziale e del fatto che la slot machine si dice "one-armed bandit", ecco a voi i metodi **Multi-armed bandit.**

  

Esistono 4 strategie di questo tipo che sono state sviluppate e hanno ottenuto buoni risultati:

- metodo _ε-Greedy_
    
- metodo Upper Confidence Bound (UCB)
    
- metodo di Thompson sampling
    
- metodo Upper Confidence Bound for Matrix Factorization (UCB-MF)
    

  

Queste strategie usano però dei concetti di base che ora vi spiego.

  

## Concetti di base base base

L'idea di base è quella di pensare a un'entità e fargli compiere delle azioni (ad esempio usare una slot machine piuttosto che un'altra, oppure scegliere un item da raccomandare piuttosto che un altro) che porteranno a ottenere un premio dall'ambiente.

Ad esempio se la nostra entità è un sistema di raccomandazione, l'ambiente potrebbe essere il nostro utente (o utenti) e i reward like, share, commenti, visualizzazioni, etc.

![](https://static.wixstatic.com/media/4959fd_2d6dd0ba944f4f8eaea5f8a3501fc4cb~mv2.png/v1/fill/w_333,h_122,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_2d6dd0ba944f4f8eaea5f8a3501fc4cb~mv2.png)

L'obiettivo è chiaramente quello di ottenere il maggior numero di rewards.

  

  

Bene.

Modelliamo questa cosa sull'idea di "bandit".

Un bandit non è un bandito, ma una tupla **< A,** **_R_** **>** dove

- **A** è un insieme di azioni (ad esempio "scegli la slot machine 7")
    
- **_R_** è una funzione di rewarding (che però è sconosciuta, altrimenti sarebbe troppo facile)
    

  

Ci serve poi un insieme di probabilità di rewarding per ogni azione, chiamiamo questo insieme **_K = {_** θ₁,θ₂, ... , θₖ }**_._**

A questo punto possiamo definire il concetto di "qualità" come una funzione **_Q_**

![](https://static.wixstatic.com/media/4959fd_a9f3a19ae1d142869ee77a23b08cb3e0~mv2.png/v1/fill/w_332,h_68,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a9f3a19ae1d142869ee77a23b08cb3e0~mv2.png)

  

  

  

  

  

![](https://static.wixstatic.com/media/4959fd_c554d99156cd46ecb01bfac9011f1b4a~mv2.png/v1/fill/w_355,h_56,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c554d99156cd46ecb01bfac9011f1b4a~mv2.png)

  

![](https://static.wixstatic.com/media/4959fd_d2884fe9ec3a4afe87cf9c262304f6b8~mv2.png/v1/fill/w_174,h_65,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_d2884fe9ec3a4afe87cf9c262304f6b8~mv2.png)

  

  

  

  

  

  

  

Ora sappiamo capire quanto vale la nostra scelta, ma non se all'utente piace o meno. Per questo dobbiamo definire il concetto di "regret", che non è altro che la differenza tra il valore massimo di reward associato a una certa azione e il reward ottenuto con quelle che abbiamo scelto noi.

La formula per il calcolo del massimo reward è la seguente:

![](https://static.wixstatic.com/media/4959fd_1bf5eea5d65b42719b661e0252c511f2~mv2.png/v1/fill/w_488,h_74,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_1bf5eea5d65b42719b661e0252c511f2~mv2.png)

La formula per il calcolo del valore di regret diventa quindi

![](https://static.wixstatic.com/media/4959fd_ba23a43c2d184ef1ae00215a04d25b80~mv2.png/v1/fill/w_372,h_116,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_ba23a43c2d184ef1ae00215a04d25b80~mv2.png)

In poche parole prendiamo la differenza tra il valore di "apprezzabilità massimo" che l'utente poteva provare dandogli qualcosa che gli piaceva, e quello che gli abbiamo proposto. In termini ancora più semplici misuriamo quanto all'utente piace che gli diamo delle feci invece che una torta sacher.

  

Questo è in sintesi e in generale, cosa fa il metodo Multi-armed bandit.

Ora passiamo a vedere le strategie nel dettaglio

  

  

|   |   |   |   |
|---|---|---|---|
||_ε-Greedy_|UCB|Thompson|
|Vantaggi|semplice|

|   |   |   |
|---|---|---|
|veloce|funziona bene con probabilità di rewards equamente distribuite|funziona bene con probabilità di rewards che non seguono una gaussiana|
|Svantaggi|non esplora completamente le soluzioni|

|   |   |
|---|---|
|non raggiunge la soluzione ottimale|più complesso da implementare|

|   |   |
|---|---|
|se le probabilità non sono distribuite non si può applicare|richiede molti samples|

|   |
|---|
|computazionalmente molto dispendioso|

  

## _ε-Greedy strategy_

Questo metodo si basa su un'idea piuttosto semplice, ossia scegliere una percentuale _ε_ di items di esplorazione e una percentuale 1-_ε_ di items di exploit.

  

Cosa significa:

se abbiamo dobbiamo consigliare 10 items e consideriamo un _ε=0.3_ , la strategia greedy farà in modo che il 70% delle raccomandazione siano items che sappiamo piaceranno, mentre il restante 30% saranno items di esplorazione.

  

Per ottenere questa condizione, usiamo seguiamo questi passaggi:

1. scegliamo un valore p che può essere 0 o 1, con probabilità _ε_ che sia 0, e probabilità 1-_ε_ che sia 1
    
2. se p=1 allora suggerisco un item di exploitation
    
3. se p=0 allora suggerisco un item di exploration
    

  

Chiaramente vogliamo capire se i suggerimenti offerti hanno migliorato o peggiorato il regret dell'utente, perciò dopo ogni suggerimento calcolo la media delle valutazioni con la formula seguente

![](https://static.wixstatic.com/media/4959fd_50939ee47869461bbb00a69da38507df~mv2.png/v1/fill/w_432,h_90,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_50939ee47869461bbb00a69da38507df~mv2.png)

dove Nₜ(a) è definito come

![](https://static.wixstatic.com/media/4959fd_39a3763cc221409d8689ebfb6937aaec~mv2.png/v1/fill/w_435,h_44,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_39a3763cc221409d8689ebfb6937aaec~mv2.png)

Ovviamente l'obiettivo è scegliere l'azione a migliore, ossia

![](https://static.wixstatic.com/media/4959fd_99a52427ab0a4abc862c72db05d2e640~mv2.png/v1/fill/w_494,h_140,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_99a52427ab0a4abc862c72db05d2e640~mv2.png)

  

(argmax non è altro che il massimo ottenuto cambiando il valore del parametro a)

  

## Upper Confidence Bound

L'idea di questa strategia è quella di giocare sulla potenzialità di un'azione di essere ottenere un buon reward. Cioè, non spariamo a caso come facevamo prima, ma decidiamo di suggerire gli elementi che hanno più potenzialità di piacere all'utente.

![](https://static.wixstatic.com/media/4959fd_ffbd12b007ef48e5a9446868075cc49d~mv2.png/v1/fill/w_592,h_218,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_ffbd12b007ef48e5a9446868075cc49d~mv2.png)

"Eh, ma grazie al topocazzo, se sapessi già quanto può piacere un elemento a Bob glielo suggerirei e basta". Replica: eh grazie al gattocazzo, basta capire come fare e farlo.

  

Prepariamoci perchè ora sarà un sussegguirsi di calci sulle gengive per quelli che non amano la matematica.

Consideriamo un'azione a. Noi sappiamo che quell'azione può ricevere una certa dose di apprezzamento Q(a) che però non conosciamo. Però sappiamo che fino a quel momento l'azione a ha ricevuto una certa media di rewards. L'algoritmo inoltre deve favorire quelle azioni che sono state proposte poche volte e che quindi hanno un alto tasso di potenziale di apprezzamento (o rewarding).

Perciò

  

  

![](https://static.wixstatic.com/media/4959fd_db34b2a9f4074f649f685343d7063aa7~mv2.png/v1/fill/w_592,h_174,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_db34b2a9f4074f649f685343d7063aa7~mv2.png)

ossia l'apprezzamento finale, sarà più basso dell'apprezzamento medio sommato all'apprezzamento potenziale.

Guardiamola in questi termini: più un criminale rimane un criminale, più non ci aspetteremo che diventi un cittadino modello. Perciò il valore di U(a) sarà legato al numero di volte che l'elemento **_a_** è stato proposto. Più grande è il numero di volte che è stato suggerito **_a_**, più basso sarà il potenziale che **_a_** migliori il reward medio.

  

Ma come calcolo quindi questo benedetto potenziale? (oltrettutto ci ho appena fatto caso che U viene usato anche come simbolo per esprimere l'energia potenziale in fisica).

  

Usiamo la disequazione di Hoeffding che non ho voglia di trattare, quindi dirò semplicemente che ci permette di dire che U è uguale a questa cosa qua

![](https://static.wixstatic.com/media/4959fd_5d6e5ec628144225afe1b415b84860e0~mv2.png/v1/fill/w_275,h_94,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5d6e5ec628144225afe1b415b84860e0~mv2.png)

Come si vede, compare proprio il termine Nₜ(a) a denominatore.

Da qui possiamo calcolare l'azione migliore da applicare che è

![](https://static.wixstatic.com/media/4959fd_075c7e710bf142b9aa254e55d3b90da0~mv2.png/v1/fill/w_405,h_90,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_075c7e710bf142b9aa254e55d3b90da0~mv2.png)

Ma, ma, ma. Avevamo detto che questo metodo va bene se ci sono distribuzioni di probabilità dei rewarding gaussiane. Ma che vuol dire? E che succede se non sono gaussiane? Semplice, ho omesso di spiegare la disuguaglianza di Hoeffding che ha come premessa l'utilizzo di variabili casuali indipendenti e con distribuzione uniforme. Se non ho una distribuzione normale semplicemente non posso applicare la disuguaglianza, che sta alla base del nostro bel metodo. Magari più avanti spiegherò nel dettaglio anche questo particolare, ma per il momento mi scoccia.

  

## Thompson sampling

Infine ci sarebbe da parlare del metodo di Thompson. Come dice il nome, avrà a che fare con cosiderare dei sample, no?

L'idea di questo metodo è quella di considerare le conoscenze maturate fino a un certo punto t su ciascuna macchina (o previsione) per capire quali azioni intraprendere all'istante t+1.

  

Mettiamo caso di avere 3 azioni:

a₁ -> è piaciuta 3 volte su 5 tentativi -> ossia è piaciuta 3/5 = 60% delle volte

a₂ -> è piaciuta 2 volte su 7 tentativi  -> ossia è piaciuta 2/7 = 28 % delle volte

a₃ -> è piaciuta 5 volte su 8  tentativi -> ossia è piaciuta 5/8 = 62% delle volte

  

Rullo di tamburi... Doppio carpiato e ... ginocchiata sui denti...

**A questo punto cosa faccio: uso una distribuzione Beta per campionare un valore random da ciascuna macchina.**  

...Momento di stordimento generale...

  

In parole povere non faccio altro che usare una funzione di distribuzione che genera valori distribuiti in questo modo

![](https://static.wixstatic.com/media/4959fd_3294209fc1c84112b474fa3df5d5e75e~mv2.png/v1/fill/w_312,h_57,al_c,lg_1,q_85,enc_auto/4959fd_3294209fc1c84112b474fa3df5d5e75e~mv2.png)

con la funzione Beta definita come

![](https://static.wixstatic.com/media/4959fd_1f5cb6561cad4495a974fe64af0061fe~mv2.png/v1/fill/w_475,h_95,al_c,lg_1,q_85,enc_auto/4959fd_1f5cb6561cad4495a974fe64af0061fe~mv2.png)

Ad esempio la Beta distribution su 2 vittorie e 10 sconfitte mi produce un grafico del genere

![](https://static.wixstatic.com/media/4959fd_3770753d6686424cab6ea534e62a9458~mv2.png/v1/fill/w_310,h_306,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_3770753d6686424cab6ea534e62a9458~mv2.png)

Perciò se campionassi usando questa distribuzione avrei tanti valori compresi tra 0.1 e 0.3 e pochi negli altri casi. In altre parole, se avessi un'urna e dovessi estrarre dei valori tra 0 e 1, è molto più probabile che peschi tanti 0.1, 0.22, 0.33, ... ma pochi 0.8, 0.9, etc.

  

Cosa facciamo quindi:

1. prendiamo la macchina A e spariamo un numero a caso di quelli della distribuzione. Vuol dire che prendiamo a caso un numero tra 0 e 1 seguendo la probabilità di uscita data dalla beta distribuzione corrispondente ad A. Facciamo finta che sia uscito 0.6
    
2. rifacciamo la stessa cosa con B. Facciamo finta che sia uscito 0.5
    
3. rifacciamo la stessa cosa con C. Facciamo finta che sia uscito 0.58
    

  

In questo caso ad essere suggerita è la macchina A perchè il numero uscito è più grande degli altri. Aggiorno quindi le frequenze in maniera corrispondente.

  

Non è detto che il numero uscito sia più alto se la macchina A ci fa vincere spesso. Se la macchina A ha un grande tasso di vincita significa soltanto che è più probabile che il numero campionato sia grande, ma non significa che uscirà per forza un numero grande. Potrebbe benissimo capitare di beccare un numero piccolissimo, perchè il numero estratto è casuale.

  

È evidente che a lungo andare le scelte più proposte verranno penalizzate dal fatto che il numero di tentativi aumenta, ma in ogni caso c'è sempre la possibilità che una qualsiasi delle azioni/macchine venga scelta, indipendentemente se è una macchina fruttuosa o meno.

  

### Possibili domande

1. What is the main goal of a multi-armed bandit problem?
    
2. Explain the exploration-exploitation dilemma in the context of recommender systems.
    
3. What are the advantages and disadvantages of the epsilon-greedy algorithm?
    
4. What is regret in the context of recommendation systems, and why is it important?
    
5. Describe Hoeffding’s Inequality and its application in the context of multi-armed bandits.
    
6. What is the significance of the Thompson Sampling method in multi-armed bandit problems, and how does it utilize Bayesian inference?
    
7. Discuss the role of Upper Confidence Bounds (UCB) in the context of multi-armed bandits and how it helps in decision-making.
    
8. What are the implications of using a fixed exploration rate in the epsilon-greedy algorithm, and how can it affect the overall performance of a recommendation system?
    
9. Qual è la differenza tra un approccio statico e uno dinamico nel risolvere problemi di multi-armed bandit?
    
10. Come si calcolano le probabilità posteriori nel metodo Thompson Sampling e come influenzano la scelta del braccio?
    
11. In che modo la formulazione di UCB bilancia esplorazione ed exploitazione? Scrivi la formula e spiegane i termini.
    
12. Quali strategie si possono adottare per minimizzare il regret in un problema multi-armed bandit?
    
13. Come può un algoritmo multi-armed bandit essere utilizzato per migliorare le raccomandazioni personalizzate in presenza di utenti nuovi?
    
14. Quali sono i vantaggi di un tasso di esplorazione decrescente (ϵ\epsilonϵ) rispetto a un tasso fisso nell'epsilon-greedy algorithm?
    
15. Cos’è un contextual bandit e in che modo si differenzia da un multi-armed bandit standard? Fornisci un esempio.
    
16. Qual è il ruolo di Hoeffding’s Inequality nella definizione dei bound di confidenza per le ricompense?
    
17. Confronta gli approcci Bayesiani (es. Thompson Sampling) e frequentisti (es. UCB) nella risoluzione di problemi di multi-armed bandit.
    
18. Perché gli algoritmi greedy possono soffrire di esplorazione insufficiente nei problemi di multi-armed bandit? Come si possono mitigare questi limiti?