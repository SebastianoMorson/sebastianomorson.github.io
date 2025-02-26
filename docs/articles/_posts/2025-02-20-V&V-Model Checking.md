---
layout: post
title: V&V-Model Checking
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->

# Model checking nella logica temporale
Il model checking 

## Model checking per LTL
Corrisponde a P-satisfiability.

>Dove vogliamo arrivare: 
>
>Se $B_{(P,\varphi)}$ contiene una componente adeguata allora la formula è soddisfabibile.

### 1. Definizioni di base
- state(A)
- consistenza
- tail
- componente adeguata
- behavior graph

- S just, compassionate, fair, fulfilling, adequate, 

### 2. Procedimento
L'idea è questa:
1. Costruisco il grafo delle computazioni per il programma P $G_P$
2. Costruisco il tableau $T_\varphi$ per la formula LTL $\varphi$
3. A partire da $G_P$ e $T_\varphi$ costruisco il behavior graph $B_{(P, \varphi)}$ 
4. Decompongo $B_{(P,\varphi)}$ nei suoi MSCS $S_1, S_2, \dots, S_n$
    1. richiamo la procedura ADEQUATE_SUB per ogni sottografo di $B_{(P,\varphi)}$
    2. La procedura termina con $S' \subseteq S$ e:
        - se $S' = \emptyset$ allora non esiste nessun sottografo adeguato e quindi il programma non soddisfa in nessun caso la formula LTL $\varphi$
        - se $S' \ne \emptyset$ allora $S'$ rappresenta proprio la componente adeguata che garantisce che esiste una computazione di P che soddisfa la formula LTL $\varphi$


La definizione di ADEQUATE_SUB è la seguente:

```python
def Adequate_Sub(S:MSCS)->SCS:
  if S.isNoTFulfilling(): 
    return null #se non è fulfilling non ci interessa
  if S.isNotJust():
    return null #se non è just non ci interessa

  #se è compassionate e ha superato i controlli precedenti significa che ho trovato la mia componente adeguata
  if S.isCompassionate():
    return S  

  # S è fulfilling e just ma non compassionate
  # Identifichiamo il sottoinsieme T delle transizioni compassionate non prese in S
  T = {t | t transizione and t abilitato in P and t non preso in S}

  # Rimuoviamo EN(T, S) da S per ottenere U
  U = S - EN(T,S) 

  #applico la procedura ricorsivamente su tutti i sottografi del sottografo
  for new_S in getStronglyConnectedSubgraphs(U):
    return Adequate_Sub(new_S)

```

Questo metodo permette di dimostrare la P-satisfiability, ovviamente per verificare la P-validity è sufficiente negare la formula LTL $\varphi$ e verificare la presenza di una componente adeguata per $B_{(P, \lnot \varphi)}$ 

### 3. Risultati
Ovviamente se $S'$ è un sottografo adequate, non è detto che S tale che $S' \subset S$ sia anch'esso adequate.
Questo è dato dal fatto che se S' per essere adequate dev'essere fair e fulfilling, quindi just, compassionate e fulfilling,
ma:
- se S' è just allora S è just 
- se S' è fulfilling allora S è fulfilling
- se S' è compassionate NON è detto che S sia compassionate




### Bounded Model Checking
L'idea è quella di non considerare tutta la computazione infinita, ma cercare possibili cicli. Se la computazione è del tipo A,B,C,D, A,B,C,D, ..., ovviamente la proprietà di "non compare mai E" deve valere all'interno del ciclo A,B,C,D

## Model checking per CTL
Ecco, per CTL le cose cambiano un po', perchè gli operatori di percorso aiutano a semplificare le cose. 

Prima di spiegare bene le cose, è doveroso ricordare qual è la differenza tra LTL e CTL:
- LTL permette di esprimere vincoli sull'ordine globale degli eventi in una singola computazione
  - MA NON VINCOLI DI POSSIBILITÀ 
  
  Ad esempio sarò in grado di definire la proprietà 
  >`Il forno non deve mai iniziare a scaldare prima che il pulsante di avvio sia stato premuto almeno una volta`
  
  ma non 

  >`Ogni volta che si preme il pulsante di avvio, esiste almeno un futuro in cui il forno si riscalderà`
  
- CTL permette di esprimere proprietà sulla struttura del modello
  - MA NON VINCOLI DI FAIRNESS
  
  Quindi ad esempio sarò in grado di definire la proprietà 
  >`È sempre possibile raggiungere uno stato in cui il forno si sta riscaldando` 
  
  ma non

  >`Se l'errore accade, allora in questa computazione il forno non si scalderà mai più`

### Microwave example
Prendiamo come esempio le seguenti specifiche per il funzionamento di un forno a microonde:

```
To cook food in the oven, open the door, put the food inside,
and close the door. Do not put metal containers in the oven.
Press the start button. The oven will warmup for 30 seconds,
and then it will start cooking. When the cooking is done, the
oven will stop. The oven will stop also whenever the door is
opened during cooking. If the oven is started while the door is
open, an error will occur, and the oven will not heat. In such a
case, the reset button may be used
```

Un modello di Kripke che rappresenta il seguente sistema potrebbe essere qualcosa del genere

![microwave_model](/docs/assets/images/microwave_model.png)

È evidente che le formule CTL che definiscono le proprietà per un modello corretto sono

1. If the oven heats, then the door is closed:

$$
AG(Heat \to Close)
$$

2. Whenever the start button is pushed, eventually the oven will
heat:

$$
AG(Start \to AFHeat)
$$

3. Whenever the oven is correctly started, eventually the oven will
heat:

$$
AG((Start \land \lnot Error) \to AFHeat)
$$

4. Whenever an error occur, it will be still possible to cook:

$$
AG(Error \to EFHeat)
$$

Possiamo notare come il modello proposto non garantisca tutte le formule richieste affinchè un microonde rispetti le specifiche.

In particolare è facile osservare che la proprietà numero 2 è violata in questo punto

![violation](/docs/assets/images/violation.png)

Si può vedere in modo analogo come anche le proprietà 3 e 4 non sono rispettate da questo modello.

Quello che sto per dire è molto importante per capire la differenza sostanziale tra LTL-MC e CTL-MC:
> in CTL-MC non ci interessa osservare una computazione infinita, ma solo la struttura del modello per poter dire se una proprietà è violata o meno.

##  Model checking LTL vs CTL
Un'importante osservazione da fare è che il model checking di CTL è più rapido del model checking di LTL.

LTL-MC é PSPACE mentre CTL-MC è polinomiale rispetto alla dimensione del modello.

La differenza sta nel fatto che per CTL le proprietà possono essere verificate sugli stati singoli, propagando l'informazione localmente (ricordati dell'esempio del forno a microonde).

Per LTL invece certe proprietà possono dipendere da una serire infinita di scelte. Anche per questo motivo abbiamo avuto bisogno di applicare il Bounded Model Checking nel caso LTL, perchè a volte il numero di stati "esplodeva" nel momento in cui generavamo il behaviour graph. 