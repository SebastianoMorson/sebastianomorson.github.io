---
layout: post
title: RECSYS Lecture 10 - Transformers
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - artificial
categories: article

---
<!--more-->

![](https://static.wixstatic.com/media/4959fd_692118617a1b496e98b02a7a61862376~mv2.png/v1/fill/w_286,h_463,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_692118617a1b496e98b02a7a61862376~mv2.png)

E finalmente possiamo parlare di una delle tecnologie più interessanti degli ultimi 20mila anni: i transformers.

Come nel film, anche questa tecnologia è comparsa sulla Terra per combattere i malvagi; per fare il culo a tutti quei modelli obsoleti e insulsi che riescono si e no a predirre i prezzi delle case.

Perciò bando alle ciance, iniziamo a parlare di LSTM.

  

Esatto. Avete sentito bene, sti cazzi dei transformers, noi vogliamo parlare di LSTM.

  

## Long Short-Term Memory

LSTM è un modello ricorsivo che serve per mitigare l'effetto della scomparsa del gradiente in modo da permettere alla rete di apprendere da sequenze di dati. L'idea quindi è di trasferire le conoscenze pregresse ai layer successivi, così da modellare situazioni di consequenzialità o relazioni tra elementi che appaiono uno di seguito all'altro.

Lo schema è questo

![](https://static.wixstatic.com/media/4959fd_2ac4bae987e845b8b69c823a6e0fb394~mv2.png/v1/fill/w_592,h_422,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_2ac4bae987e845b8b69c823a6e0fb394~mv2.png)

Devo ricordarmi di disegnare da zero le immagini, sennò mi fanno il culo a stelle e strisce quelli dei siti da cui le ho copiate.

  

Tralasciando questo dettaglio: come si può vedere abbiamo 3 gate e una cosa strana chiamata "cell state", che fondamentalmente è la memoria del nostro modello.

  

##### A cosa servono i gate?

Dipende dal tipo di gate, ma in generale i gate servono a fare in modo che l'informazione sia propagata più a lungo nei layer. In questo caso ne abbiamo di 3 tipi:

- input gate: serve per capire quali informazioni devono essere aggiornate o modificate
    
- forget gate: serve a decidere quali informazioni devono essere scartate
    
- output gate: servono a capire quali informazioni devono essere propagate al layer successivo
    

  

Potete notare subito una cosa: se ho 3 layer lstm A, B e C, non potrò calcolare C prima di aver calcolato B, che a sua volta non posso calcolare prima di essere passato per A.

In breve: non posso parallellizzare la computazione.

  

Si può notare anche un'altra cosa: una volta computato l'hidden state A, la sua informazione sarà molto rilevante per l'hidden state B, che a sua volta sarà molto rilevante per l'hidden state C, e così via. Più hidden state ci sono, più l'informazione di A si perde in mezzo alle informazioni degli hidden state che seguono A.

Questo è un problema, perchè significa che se le sequenze sono molto lunghe finirò per perdere quasi del tutto molta dell'informazione che viene offerta dagli hidden state iniziali.

  

Ma per fortuna in mezzo a questa valle di lacrime, un asteroide squarciò il cielo e il suo calore asciugo le nostre lacrime: ecco a voi i Transformers.

  

## Transformers

Nel 2017 alcuni ricercatori di Google presentarono questo modello all'interno del paper "[Attention is all you need](https://arxiv.org/abs/1706.03762)". L'obiettivo non lo conosco, ma sicuramente quello che hanno tirato fuori risolve il problema che avevamo con LSTM (e di conseguenza con GRU e i modelli RNN in generale).

  

Ora copierò e incollerò l'immagine più copiata del web:

![](https://static.wixstatic.com/media/4959fd_2d85ce37d5374aaa91d907cf0d8902eb~mv2.png/v1/fill/w_592,h_720,al_c,q_90,usm_0.66_1.00_0.01,enc_auto/4959fd_2d85ce37d5374aaa91d907cf0d8902eb~mv2.png)

Direi che per capire quello che fa bisogna procedere a piccoli passi.

Iniziamo con l'idea.

  

#### Idea

l'idea dei transformers è quella di usare una rete neurale profonda e il concetto di "self-attention" per catturare il contesto per ogni parola passata in input. In questo modo il modello è in grado di gestire sequenze testuali molto lunghe. Inoltre la self-attention viene calcolata contemporaneamente per tutte gli elementi della sequenza di input, cosa non possibile con le RNN.

  

## Architettura

I Transformer seguono questa architettura:

![](https://static.wixstatic.com/media/4959fd_202bdf9e4087448c9ac1e7408f9e5145~mv2.png/v1/fill/w_592,h_401,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_202bdf9e4087448c9ac1e7408f9e5145~mv2.png)

In realtà è una versione semplificata, ma il concetto è che viene usato un blocco di encoding e uno di decoding.

L'encoder serve per catturare le informazioni dell'input fornito. Se gli passo la frase "l'uomo è per l'uomo un lupo", l'encoder cerca di creare una rappresentazione della frase che sia altamente informativa. La self-attention viene usata per catturare il contesto di ogni token.

Il decoder serve per creare del contenuto a partire dall'output dell'encoder. Una volta che l'encoder mi fornisce una rappresentazione del mio input, il decoder è in grado di estrarre tutte le informazioni necessarie per creare del nuovo contenuto coerente con le relazioni delle parole dell'input. Il decoder a differenza dell'encoder non usa solo un input, bensì usa sia l'input dell'encoder che una sequenza di input parziale.

  

Quindi:

- encoder = catturare le informazioni dell'input
    
- decoder = creare del nuovo contenuto
    

  

## Encoder

  

### Input embedding

![](https://static.wixstatic.com/media/4959fd_4690da29e7be49b581afaaf0ab4a029e~mv2.png/v1/fill/w_280,h_223,al_c,lg_1,q_85,enc_auto/4959fd_4690da29e7be49b581afaaf0ab4a029e~mv2.png)

Non ci perdo troppo tempo perchè è relativamente importante. Questo layer serve per dare una forma più maneggevole al nostro input. I metodi per creare una rappresentazione dell'input dipendono dai dati di partenza che forniamo al modello.

  

### Positional Encoding

Il positional encoding serve per capire qual è l'ordine degli elementi nella sequenza.

Ma perchè è così importante? Perchè se a un mio amico dico "se mangio la torta poi faccio la cacca" non voglio che capisca "se mangio la cacca poi faccio la torta". Quindi con il positional encoding è come gli dicessi "(1)se (2)mangio (3)la (4)torta (5)poi (6)faccio (7)la (8)cacca"

  

### Multi-head attention

![](https://static.wixstatic.com/media/4959fd_f50cd28b44a64571958a1cfd77b39f0e~mv2.png/v1/fill/w_166,h_202,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_f50cd28b44a64571958a1cfd77b39f0e~mv2.png)

Questo è un passaggio fondamentale. Una volta che ricevo l'embedding dei miei tokens, mi serve un modo per capire quali siano le relazioni tra i token. Per farlo uso il concetto di "attention".

  

  

  

  

  

  

  

##### Attention

Lo schema sottostante rappresenta il passaggio successivo all'embedding.

![](https://static.wixstatic.com/media/4959fd_24a4264331da4cc88609a62c4f80a322~mv2.png/v1/fill/w_559,h_428,al_c,lg_1,q_85,enc_auto/4959fd_24a4264331da4cc88609a62c4f80a322~mv2.png)

Come si può vedere, l'input X viene condiviso a 3 matrici dei pesi. Queste matrici sono:

- WQUERY
    
- WKEY
    
- WVALUE
    

Queste 3 matrici vengono addestrate dalla rete per creare una rappresentazione del mio input che sia molto informativa sulle relazioni tra le i tokens.

Le righe della matrice X sono parole della frase di input.

Perciò l'attention è calcolata come

![](https://static.wixstatic.com/media/4959fd_2ff1838dfc0c4f9ca34b2ceda58110a2~mv2.png/v1/fill/w_399,h_73,al_c,lg_1,q_85,enc_auto/4959fd_2ff1838dfc0c4f9ca34b2ceda58110a2~mv2.png)

Il denominatore dentro la softmax serve come termine di normalizzazione per evitare argomenti troppo grandi della softmax che porterebbero a gradienti instabili.

![](https://static.wixstatic.com/media/4959fd_ebb7a178e10547f78c78194584c3f016~mv2.png/v1/fill/w_565,h_382,al_c,lg_1,q_85,enc_auto/4959fd_ebb7a178e10547f78c78194584c3f016~mv2.png)

  

  

  

  

Il problema è che come abbiamo visto, la matrice dei tokens è l'unione di tutti gli embeddings dei tokens, quindi la matrice Z che otteniamo è una rappresentazione delle relazioni calcolate su tutti i tokens. Ma ciò significa che potrebbe dare più peso alle relazioni tra il primo token e tutti gli altri, ma meno rispetto al secondo token e tutti gli altri, mentre noi vogliamo che racchiuda le informazioni tra tutti i tokens, con tutti i tokens. Per questo usiamo la multi-head attention. Questo passaggio considera l'attenzione per ciascuna head così calcolata

![](https://static.wixstatic.com/media/4959fd_91444ec96ea744d382185b2fbf51ac40~mv2.png/v1/fill/w_458,h_96,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_91444ec96ea744d382185b2fbf51ac40~mv2.png)

Quindi ad esempio se abbiamo 8 heads, useremo 8 matrici dei pesi WQUERY WKEY WVALUE sugli input X e calcoleremo head1 head2 ... head8  . Infine concateniamo assieme le heads perchè il feed-forward layer si aspetta una singola matrice e non 8 heads.

  

  

### Residual/Skip connection

Nel modello che abbiamo visto all'inizio erano presenti delle connessioni come questa a lato.

![](https://static.wixstatic.com/media/4959fd_df9851c024fe46c799eaea6ac8957ef4~mv2.png/v1/fill/w_151,h_305,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_df9851c024fe46c799eaea6ac8957ef4~mv2.png)

Queste connessioni servono per "skippare" passaggi della rete in fase di training e per propagare l'informazione dei layer precedenti.

Nell'esempio sotto, l'input embedding viene propagato alla multi-head attention e viene skippato invariato anche al layer "add & norm".

![](https://static.wixstatic.com/media/4959fd_de5343e1d9314c758a2d7c6b6fe13176~mv2.png/v1/fill/w_280,h_187,al_c,lg_1,q_85,enc_auto/4959fd_de5343e1d9314c758a2d7c6b6fe13176~mv2.png)

  

  

  

  

  

  

  

  

  

  

Durante la fase di addestramento il modello deciderà quanto peso dare a questa skip connection. Se viene assegnato un basso peso alla skip connection avremo che l'informazione dell'input embedding originale viene depotenziato o addirittura annullato (quindi il modello può anche eliminare la skip connection se gli fa comodo), oppure può dargli un forte peso, quindi eliminando il contributo della multi-head attention.

Il senso di tutto ciò? Migliorare le performance. Ce la fa? A quanto pare sì.

  

  

La cosa divertente è che questa è solo la parte dell'encoder.

  

  

## Decoder

Il decoder sfrutta l'informazione ricavata dall'encoder per la generazione di nuovo contenuto.

  

L'idea alla base è quella di calcolare p(Z | X) per ogni X, ossia trovare il token che ha la probabilità più alta di presentarsi sapendo che prima c'è stato il token X.

All'inizio X corrisponde all'output del dell'encoder.

  

Immaginiamo che si inizi sapendo x. In seguito viene prodotto dal decoder il token Z1 , quindi il passaggio successivo è calcolare il valore di Z2 sapendo che sono già comparsi Z1 e X e così via finchè non si raggiunge un simbolo finale.

  

Il processo è ben esemplificato da queste due gif.

  

  

![](https://static.wixstatic.com/media/4959fd_0b17161082a0486db4e403ec35cff925~mv2.gif)

All'inizio il decoder ha bisogno dell'encoding di K e di V per poter generare il primo elemento di partenza.

  

![](https://static.wixstatic.com/media/4959fd_42b651fcbf354021ae15cb44cc2d5a3f~mv2.gif)

in seguito il decoder utilizza gli output precedenti (a cui è stato aggiunto il positional encoding) per prevedere gli output successivi creando la propria Query matrix e usando la Key matrix e la Value matrix dell'encoder. L'addestramento avviene tramite Masking, ossia viene mascherato il token successivo a quello che abbiamo raggiunto, così che non possa essere "sbirciato".

Supponiamo che il decoder stia generando una traduzione parola per parola:

- Se ha già generato "La casa è", al prossimo step il decoder dovrà prevedere la parola successiva senza sapere quale sarà la parola seguente a "è".
    
- Il look-ahead masking blocca qualsiasi accesso a parole successive a "è", consentendo una generazione causale, dove ogni parola si basa solo sul contesto passato.
    

Come si può vedere, all'inizio all'inizio l'unica cosa che si conosce è l'encoding di K e di V provenienti dall'encoder.

  

### Domande teoriche

1. **Storia e contesto:**
    
    - Quali sono le principali applicazioni dei modelli sequence-to-sequence prima dei Transformers?
        
    - Cosa si intende per attenzione nella frase "Attention is all you need"?
        
2. **Encoder-Decoder Architecture:**
    
    - Spiega la differenza tra l’encoder e il decoder in un Transformer.
        
    - Perché l’encoder produce una rappresentazione contestualizzata del testo in input?
        
3. **Attention:**
    
    - Quali sono i tre componenti principali della self-attention e quale ruolo hanno (Query, Key, Value)?
        
    - Cosa significa che l’attenzione è **permutation invariant**? Perché questo è un problema nel linguaggio naturale?
        
4. **Multi-Head Self-Attention:**
    
    - Qual è il vantaggio di avere più heads nella multi-head self-attention?
        
    - Spiega come vengono calcolati i pesi di attenzione utilizzando la softmax.
        
5. **Positional Encoding:**
    
    - Perché il positional encoding è essenziale nei Transformers?
        
    - Descrivi come funzionano i positional encodings sinusoidali.
        
6. **Residual Connections:**
    
    - Perché i Transformers usano skip connections nei loro layer? Quali problemi risolvono?
        
7. **Decoder:**
    
    - Cosa si intende per masked self-attention e perché è usata nel decoder?
        
    - Quali strategie di decodifica si possono utilizzare nei Transformers e in quali casi sono utili?
        
8. **Cross-Attention:**
    
    - Come funziona il cross-attention e quale differenza ha rispetto alla self-attention?
        
9. **Transformers per il NLP:**
    
    - Qual è l’impatto dei Transformers nel campo del Natural Language Processing?
        
    - Perché i Transformers hanno sostituito gli RNN in molte applicazioni?
        
10. **Scalabilità:**
    
11. Quali sono i principali problemi computazionali dei Transformers e come possono essere affrontati?
    
      
    

### Domande di ragionamento

11. **Confronto con i modelli pre-Transformers:**
    
    - Quali sono i limiti dei modelli basati su RNN o LSTM che i Transformers hanno risolto?
        
12. **Multi-Head Attention:**
    
    - Cosa succede se riduciamo il numero di heads in un Transformer? Quali aspetti del modello potrebbero soffrirne?
        
13. **Masked Self-Attention:**
    
    - Spiega come il masking nel decoder aiuta nella generazione di sequenze e cosa accadrebbe senza questa tecnica.
        
14. **Beam Search vs Greedy Search:**
    
    - Confronta beam search e greedy search per la generazione del testo nei Transformers. Quali sono i pro e contro di ciascuna strategia?
        
15. **Positional Encoding:**
    
    - Se il positional encoding non fosse utilizzato, cosa perderebbe il modello? Prova a fare un esempio pratico.
        
16. **Applicazioni:**
    
    - Come viene adattata l’architettura dei Transformers per immagini e audio? Quali trasformazioni sono necessarie?
        
17. **Generazione di contenuti:**
    
    - In che modo il contrastive search può incoraggiare la diversità nel testo generato rispetto al greedy search?
        
18. **Unificazione dei domini:**
    
    - Spiega il concetto "if you can tokenize it, you can feed it into a transformer". Quali sono i limiti di questa affermazione?
        
19. **Reinforcement Learning:**
    
    - In che modo i Transformers vengono utilizzati nel reinforcement learning? Fai un esempio pratico.
        
20. **Adattabilità:**
    
    - Cosa succede se il modello Transformer è applicato a sequenze molto lunghe? Quali strategie possono essere utilizzate per gestire queste sequenze?
        
    -   
        

### Domande su algoritmi e implementazioni

21. **Softmax nella Self-Attention:**
    
    - Spiega passo passo come viene calcolata la matrice di attenzione usando softmax
        
22. **Cross-Attention:**
    
    - Qual è l’impatto del cross-attention nelle prestazioni complessive del modello?
        
23. **Scaling Factors:**
    
    - Perché il prodotto Q⋅KTQ \cdot K^TQ⋅KT viene scalato dividendo per dk\sqrt{d_k}dk​
        

  

  

#### References

[https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)

[https://jalammar.github.io/illustrated-transformer/](https://jalammar.github.io/illustrated-transformer/)

[https://ai.stackexchange.com/questions/20075/why-does-the-transformer-do-better-than-rnn-and-lstm-in-long-range-context-depen](https://ai.stackexchange.com/questions/20075/why-does-the-transformer-do-better-than-rnn-and-lstm-in-long-range-context-depen)

[https://lilianweng.github.io/posts/2018-06-24-attention/](https://lilianweng.github.io/posts/2018-06-24-attention/)

[https://www.humai.it/la-self-attention-delle-reti-transformer/](https://www.humai.it/la-self-attention-delle-reti-transformer/)

[https://medium.com/analytics-vidhya/attention-mechanism-and-softmax-65d8d50f7786](https://medium.com/analytics-vidhya/attention-mechanism-and-softmax-65d8d50f7786)

[https://stats.stackexchange.com/questions/421935/what-exactly-are-keys-queries-and-values-in-attention-mechanisms](https://stats.stackexchange.com/questions/421935/what-exactly-are-keys-queries-and-values-in-attention-mechanisms) (molto interessante un esempio presente in fondo)

- [](https://sebastianomorson.wixsite.com/sebastiano-morson/blog/tags/artificial-intelligence)