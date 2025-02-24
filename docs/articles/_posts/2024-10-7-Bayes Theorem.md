---
layout: post
title: Bayes Theorem
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
Il teorema di Bayes è un teorema fondamentale nel campo della probabilità. È così importante che quando diedi l'esame di Complexity me lo dimenticai e giustamente presi una mazzata sulle gengive e mi ritirai per la vergogna, pur sapendo tutto il resto.

Questo articolo è perciò un promemoria per il me stesso del futuro, in modo che, cascasse il mondo, il teorema di Bayes rimarrà impresso a fuoco nella mia memoria.

  

Prima di enunciare il teorema, bisogna però ricordarsi di alcune definizione:

- spazio campionario = è l'insieme dei possibili risultati di un esperimento casuale
    
- evento casuale = un evento è un elemento dell'insieme delle parti dello spazio campionario. Quindi se lancio una moneta, un evento può essere "esce testa" oppure "esce croce"
    
- probabilità = è uguale al numero di risultati che ci piacciono, diviso il numero totale di possibili risultati
    
- eventi dipendenti ed eventi indipendenti = due eventi si dicono indipendenti, se il verificarsi di uno dei due, non influisce in nessun modo nel verificarsi del secondo. Ad esempio se lancio un dado, non è che se compare 1 allora i numeri successivi posso in qualche modo prevederli. Viceversa, due eventi sono dipendenti se il verificarsi di uno, influisce sul verificarsi del secondo. Ad esempio se tiro un pugno a un mio amico, posso prevedere che me ne arrivi un altro indietro.
    

Ebbene, cosa afferma il teorema di Bayes? Afferma che dati due eventi indipendenti A e B, la probabilità che si verifichi A, sapendo che si è verificato B, è uguale a:

- la probabilità che si verifichi A
    
- moltiplicata per la probabilità che si verifichi B sapendo che si è verificato A
    
- tutto diviso per la probabilità che si verifichi B
    

  

In matematichese:

![](https://static.wixstatic.com/media/4959fd_60819ad19a6d4e4c99a819f19e62919a~mv2.png/v1/fill/w_333,h_77,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_60819ad19a6d4e4c99a819f19e62919a~mv2.png)

Come si dimostra una cosa del genere? Ricordiamo che un evento condizionato è uguale alla probabilità dell'intersezione dei due eventi A e B, diviso la probabilità che si verifichi l'evento B. Ossia

![](https://static.wixstatic.com/media/4959fd_65a153e5309c481a9b2db48aacb7c663~mv2.png/v1/fill/w_286,h_76,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_65a153e5309c481a9b2db48aacb7c663~mv2.png)

Bello, ma quindi potremmo dimostrare il nostro teorema in questo modo:

1. guardo P(A**∩B) come:**
    
    ![](https://static.wixstatic.com/media/4959fd_ca44991f027f4b0ab4cd67eaa833c65a~mv2.png/v1/fill/w_563,h_73,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_ca44991f027f4b0ab4cd67eaa833c65a~mv2.png)
    
2. ma sappiamo che P(A**∩B) =** P(B**∩A)**
    
    ![](https://static.wixstatic.com/media/4959fd_e40eeb405db646fcbd0a2379c974f51b~mv2.png/v1/fill/w_490,h_52,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_e40eeb405db646fcbd0a2379c974f51b~mv2.png)
    
3. ma allora
    
    ![](https://static.wixstatic.com/media/4959fd_5c0826c30fca41319d80525d1072bcd6~mv2.png/v1/fill/w_513,h_70,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/4959fd_5c0826c30fca41319d80525d1072bcd6~mv2.png)
    

Tutto molto bello, ma a cosa ci serve scrivere sta roba se sapevamo già come calcolare P(A∩B)? Semplicemente perchè magari non abbiamo la probabilità P(A∩B) o non sappiamo P(A\|B) quindi questo teorema può tornare utile nel caso sia difficile calcolare una delle probabilità della formula che già conoscevamo. In particolare, vedremo che questo teorema torna utile nell'articolo sul Naive Bayes Method nel contesto dei recommender systems e nel calcolo dell'entropia per eventi indipendenti.