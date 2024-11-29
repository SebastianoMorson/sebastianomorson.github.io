---
layout: post
title: RECSYS Lecture 9 - Neural Networks for Recommender systems
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
---
<!--more-->

![](https://static.wixstatic.com/media/4959fd_a17b017a6ae84511ac56d00b34230941~mv2.png/v1/fill/w_280,h_190,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_a17b017a6ae84511ac56d00b34230941~mv2.png)

I metodi che abbiamo visto fin'ora andavano bene finchè i dataset erano piccoli, ma quando i dataset diventano grandi e si vogliono catturare legami non lineari è necessario chiamare la cavalleria: le reti neurali.

  

Facciamo un piccolo recap di come funziona una rete neurale:

- abbiamo dei nodi di input
    
- abbiamo dei nodi intermedi
    
- abbiamo dei nodi di output
    

  

  

In un sistema di raccomandazione, ci si aspetta che ci sia una relazione tra gli item valutati dagli utenti e gli item che saranno più propensi a considerare il futuro. In questo senso le reti ricorrenti sono particolarmente adatte per risolvere il problema di eseguire previsioni basate sulle sequenze di items con cui l'utente ha interagito.

  

## Recurrent Neural Networks (RNNs)

Le reti ricorrenti sono reti che hanno questa forma qua

![](https://static.wixstatic.com/media/4959fd_c2cf60f547394bda858f050cbccab52a~mv2.png/v1/fill/w_592,h_218,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c2cf60f547394bda858f050cbccab52a~mv2.png)

come si può osservare ciascun input nella sequenza x1,x2,...xn permette di generare una predizione che dipende dalla previsione precedente.

  

Il problema principale di questi modelli è che più la sequenza degli items è lunga, più l'effetto "memoria" del modello tende a svanire, perciò gli elementi iniziali della sequenza contribuiscono in maniera minore alle predizioni dei livelli successivi.

Per questo motivo sono stati pensati due ulteriori modelli che fanno uso di funzioni di gate per migliorare la propagazione delle informazioni dei layer precedenti su quelli successivi. Questi due modelli sono GRU e LSTM

###   

![](https://static.wixstatic.com/media/4959fd_5d9397f703b84e3bb31aa11a6f0283a3~mv2.png/v1/fill/w_592,h_171,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5d9397f703b84e3bb31aa11a6f0283a3~mv2.png)

### Gated Recurrent Unit (GRU)

![](https://static.wixstatic.com/media/4959fd_632ae74fac8a4fa09a1e529fcc72c6ab~mv2.png/v1/fill/w_314,h_195,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_632ae74fac8a4fa09a1e529fcc72c6ab~mv2.png)

### Long Short Term Memory (LSTM)

![](https://static.wixstatic.com/media/4959fd_bb5adf0154fc483c802b25cf3fe8ca6a~mv2.png/v1/fill/w_404,h_205,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_bb5adf0154fc483c802b25cf3fe8ca6a~mv2.png)

  

  

  

Il problema di queste reti è che sono complicate e costose.

Una GRU o una LSTM infatti ha bisogno di molti dati di training e di molte risorse di calcolo perchè

1. ogni input è relazionato con il precedente
    
2. a causa del punto 1 non siamo in grado di parallelizzare le computazioni
    

  

Tutti questi problemi sono risolti dai transformers.

  

## Transformer

I transformer si basano sul concetto di self-attention, ossia data una sequenza di input, ogni elemento della sequenza considera la sua relazione con gli altri elementi della sequenza in modo da individuare quali siano gli elementi chiave per rappresentare la sequenza.

La loro struttura è la seguente

![](https://static.wixstatic.com/media/4959fd_d27c32ec2897496287eb52a4e4fa722e~mv2.png/v1/fill/w_592,h_341,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_d27c32ec2897496287eb52a4e4fa722e~mv2.png)

  

Ma perchè li introduciamo nel contesto del collaborative filtering? Perchè funzionano meglio delle LSTM e di GRU. Il fatto è che riescono a catturare meglio le relazioni tra gli elementi di input nel lungo termine e contesti globali, oltre al fatto che permettono di paralellizzare la computazione, il che è utilissimo.

  

  

Tutto molto bello, ma rimangono alcuni problemi classicissimi:

1. cold start problem
    
2. gestire la sparsità delle matrici delle valutazioni
    
3. gestire le relazioni complesse tra gli items
    
      
    

Possibile che le reti neurali non possano aiutarci?

Eccerto che possono farlo.

  

## Neural Collaborative Filtering (NCF)

La prima proposta è il neural collaborative filtering.

Il problema che si cerca di risolvere è quello che avevamo visto quando abbiamo parlato di matrix factorization: l'inner product solitamente cattura correlazioni lineari tra le due matrici latenti, ma noi vogliamo cercare di catturare anche le relazioni profonde.

Per catturare relazioni profonde potremmo agire in due maniere:

- aumentiamo il numero di fattori latenti
    
    - ma questo può portare ad overfitting essendo che potrei non aver abbastanza dati per spiegare le features. Ad esempio assumiamo di avere 10 utenti e 5 cani da poter raccomandare. Potrei voler applicare la matrice di fattorizzazione e ottenere due matrici latenti per gli utenti e per i cani (che sono gli items). Quindi potrei scegliere di avere 3 fattori latenti:
        
        - lunghezza pelo
            
        - colore pelo
            
        - peso da adulto
            
        
        oppure ne protrei avere 10
        
        - lunghezza pelo
            
        - colore pelo
            
        - peso da adulto
            
        - lunghezza
            
        - altezza
            
        - aggressività
            
        - pelo riccio
            
        - pelo liscio
            
        - pelo crespo
            
        - pelo morbido
            
        
        Il problema nel primo caso è che potrei essere troppo generico con sole 3 caratteristiche latenti perchè i 5 cani potrebbero essere tutti molto simili tra loro in queste caratteristiche, mentre con 10 features potrei essere troppo specifico e ottenere una rappresentazione troppo poco generica di un elemento che piace al nostro utente.
        
- usiamo una deep neural network per catturare le correlazioni non lineari a partire dalle rappresentazioni latenti.
    

  

L'ultima idea è quella usata dal neural collaborative filtering. Intendiamo usare le deep neural networks per identificare comportamenti non lineari derivati dalla matrice di fattorizzazione generata a partire dalla matrice dei ratings.

In un colpo riusciamo a ridurre la dimensione della matrice delle valutazioni e ottenere un previsioni più accurate grazie allo studio degli aspetti non lineari nelle relazioni tra gli items.

Usiamo quindi un modello strutturato in questo modo

![](https://static.wixstatic.com/media/4959fd_c0b40db59e624364b3af97420d2ea9f2~mv2.png/v1/fill/w_512,h_298,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_c0b40db59e624364b3af97420d2ea9f2~mv2.png)

Di base cosa si fa: si prende il vettore dei fattori latenti dell'utente e il vettore dei fattori latenti degli items e si concatenano assieme prima di darli in pasto a una rete neurale profonda.

  

## Deep General Matrix Factorization (GMF)

La deep general matrix factorization è una sorta di matrix factorization sotto steroidi, infatti si è visto che performa meglio della matrix factorization tradizionale soprattutto quando si ha a che fare con matrici fortemente sparse e dimensionalità dei dati molto alte.

  

  

## NeuMF (Neural Matrix Factorization)

Il problema del Neural Collaborative Filtering è che tende a perdere il contributo dato dalle relazioni lineari tra le rappresentazioni latenti di user e items. Per questo in NeuMF l'idea è quella di usare in combo le reti neurali e la matrix factorization con l'obiettivo di catturare sia i fattori latenti lineari, sia i comportamenti non lineari. Il modello enseamble che si ottiene unendo GMF e MLP seguente

![](https://static.wixstatic.com/media/4959fd_ada68c9a9ff0473b9553b4419153e0f1~mv2.png/v1/fill/w_592,h_282,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_ada68c9a9ff0473b9553b4419153e0f1~mv2.png)

Di base cosa si fa: si prende il vettore dei fattori latenti dell'utente e il vettore dei fattori latenti degli items e si concatenano assieme. Dopodichè si fattorizza nuovamente la rappresentazione della matrice di valutazione e si considerano due nuove matrici di dimensionalità inferiore che vengono dati in pasto a una rete neurale profonda alla ricerca di legami non lineari.

  

Qualcuno potrebbe obiettare dicendo "eh, ma non bastava usare usa singola rappresentazione GMF o MLP invece che due rappresentazioni separate sia per gli items che per gli users?". Risposta: si è visto che usare un ebbending layer condiviso limita le capacità del modello.

  

  

## BERT4Rec

Ed ecco a voi l'ultimo, ma non ultimo modello presentato in questa lezione sulle reti neurali applicate ai recommender systems.

Si può immaginare la relazione tra RNN, NeuMF e Bert4Rec in questo modo:

![](https://static.wixstatic.com/media/4959fd_17f2350a3ae94feebb3c18e91e2cf626~mv2.png/v1/fill/w_343,h_213,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_17f2350a3ae94feebb3c18e91e2cf626~mv2.png)

  

La struttura di Bert4Rec è questa qua

![](https://static.wixstatic.com/media/4959fd_ed99bf0a2f0445f293188dedbb5dd726~mv2.png/v1/fill/w_592,h_498,al_c,lg_1,q_85,enc_auto/4959fd_ed99bf0a2f0445f293188dedbb5dd726~mv2.png)

Nella prima parte, abbiamo un layer di embedding che serve per creare una rappresentazione densa dei vettori di input.

Un buon compromesso di masking è tra il 10% e il 20% degli elementi. Questo parametro varia in base al dataset utilizzato.

  

Il layer fully connected è quello che permette al modello di catturare le relazioni non lineari nella sequenza di input e trasmettere la conoscenza passata ai transformers successivi.

L'immagine sottostante è un altro modo per vedere il layer dei transformers

![](https://static.wixstatic.com/media/4959fd_09e6cbcce0f343dca62ab6f0472126b0~mv2.png/v1/fill/w_425,h_442,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_09e6cbcce0f343dca62ab6f0472126b0~mv2.png)

  

Ricordiamoci che ogni transformer segue questo modello

![](https://static.wixstatic.com/media/4959fd_da542c67c2c842cbb9c1e88958f154f4~mv2.png/v1/fill/w_592,h_358,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_da542c67c2c842cbb9c1e88958f154f4~mv2.png)

### Possibili domande

1. What are the key components of a Long Short-Term Memory (LSTM) network and their functions?

  

2. How do neural networks enhance collaborative filtering in recommender systems?

  

3. What is the purpose of an autoencoder in unsupervised learning?

  

4. What is NeuMF and how does it function in recommender systems?

  

5. What are the main advantages of using neural networks for recommender systems?

  

6. Explain the concept of weight sharing in convolutional neural networks and its significance in reducing the number of parameters.

  

7. Discuss the role of regularization techniques in neural networks and provide examples of common methods used to prevent overfitting.

  

8. What are the main components of a neural network and how do they interact to perform tasks such as classification or regression?

  

9. Describe the architecture and functionality of the NeuMF model in the context of recommender systems.

  

10. What are the advantages of using Long Short-Term Memory (LSTM) networks over traditional recurrent neural networks (RNNs)?

  

11. What is the primary function of the pooling layer in a convolutional neural network?

A) To apply convolution operations

B) To reduce spatial dimensions

C) To connect neurons in a fully connected layer

D) To learn local patterns

  

12. What does the input gate in an LSTM control?

A) The flow of output from the memory cell

B) The retention of memory

C) The flow of input into the memory cell

D) The calculation of the hidden state

  

13. What is the main goal of an autoencoder in unsupervised learning?

A) To classify input data

B) To reconstruct input from a latent representation

C) To predict user preferences

D) To learn linear feature representations

  

14. What is the purpose of the forget gate in an LSTM?

A) To control the flow of input

B) To manage memory retention

C) To calculate the hidden state

D) To shift the activation function

  

15. What does NeuMF combine to enhance recommender systems?

A) Matrix factorization and decision trees

B) Matrix factorization and deep neural networks

C) Traditional CF models and linear regression

D) Deep learning and clustering techniques

  

16. Convolutional neural networks (CNNs) are specialized for processing sequential data.

- True
    
- False
    

  

17. The main purpose of the pooling layer in a CNN is to increase the spatial dimensions of the input data.

- True
    
- False
    

  

18. NeuMF stands for Neural Matrix Factorization and combines matrix factorization with deep neural networks.

- True
    
- False
    

  

19. LSTMs are designed to address the vanishing gradient problem in traditional RNNs.

- True
    
- False
    

  

20. Autoencoders are used for supervised learning tasks such as classification.

- True
    
- False
    
      
    

21. What are the key components of a Long Short-Term Memory (LSTM) network and their functions?

  

22. Explain the concept of weight sharing in convolutional neural networks and its benefits.

  

23. Describe the role of regularization in neural networks and list some common techniques used.

  

24. What is the purpose of autoencoders in unsupervised learning, and what are their two main components?

  

25. How do neural networks enhance collaborative filtering in recommender systems?

  

26. What are the main advantages of using neural networks for recommender systems compared to traditional methods?

  

27. What is the function of the fully connected layer in a convolutional neural network?

  

28. What is the significance of activation functions in neural networks?

  

29. Explain the concept of memory cells in Long Short-Term Memory (LSTM) networks.

  

30. What is the role of the output layer in the NeuMF model for recommender systems?

  

31. What are the key components of a Long Short-Term Memory (LSTM) network, and how do they contribute to its ability to learn long-term dependencies?

  

32. Explain the role of regularization in neural networks and describe some common techniques used to prevent overfitting.

  

33. Describe the architecture of NeuMF and how it integrates matrix factorization with neural networks.

  

34. What are the advantages of using neural networks in recommender systems compared to traditional collaborative filtering methods?

  

35. What is the purpose of the encoder and decoder in an autoencoder, and how do they work together to achieve the goal of reconstruction?

  

36. What is the significance of weight sharing in convolutional neural networks, and how does it impact the model's performance?

  

37. How do neural networks enhance collaborative filtering in recommender systems compared to traditional methods?

  

38. Explain the role of activation functions in neural networks and why they are essential for model performance.

  

39. What are the main components of a Long Short-Term Memory (LSTM) network, and how do they work together to address the vanishing gradient problem?

  

40. Describe the optimization objective of autoencoders and the significance of minimizing reconstruction error.

- [](https://sebastianomorson.wixsite.com/sebastiano-morson/blog/tags/artificial-intelligence)