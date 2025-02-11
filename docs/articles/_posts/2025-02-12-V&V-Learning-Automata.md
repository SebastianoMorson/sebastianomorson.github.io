---
layout: post
title: V&V-Learning Automata
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->

# Learning Automata: Imparare Automata a Stati Finiti e $\omega$-Automata

Gli automi sono modelli computazionali che riconoscono linguaggi (insiemi di parole) e, in particolare, i linguaggi regolari (per parole finite) o gli ω-linguaggi regolari (per parole infinite). Un aspetto fondamentale nella teoria degli automi è la possibilità di apprendere (o “learning”) l'automa corretto che riconosca un dato linguaggio, partendo da informazioni limitate e facendo interagire un learner (studente) e un teacher (insegnante) esperto del linguaggio target.

Quindi la prima cosa fondamentale da ricordare quando si parla di Learning Automata è che: esiste un **learner** ed esiste un **teacher** che **interagiscono** tra loro.

## Il Modello di Apprendimento

Nel contesto del learning degli automi, il <u>teacher conosce il linguaggio target $\mathcal{L}$</u> (che può essere, ad esempio, un linguaggio regolare o $\omega$-regolare) mentre il <u>learner inizia conoscendo solo l'alfabeto di base</u> e deve costruire progressivamente una ipotesi $\mathcal{A}$ (un automa, che si tratti di un DFA per linguaggi finiti o di un automa di Büchi, Müller o Rabin per $\omega$-linguaggi).

Il modello di apprendimento tipico si basa su due tipi di query:

>- ***Membership Query (MQ)***:
    Il learner propone una parola $w$ e chiede al teacher: "È $w$ appartenente al linguaggio $\mathcal{L}$?"
    Il teacher risponde "sì" o "no".
    - Nel caso di risposta negativa, il teacher fornisce anche un controesempio minimo $w'$ che dimostri la discrepanza, ossia un elemento di $(\mathcal{L}_0∖\mathcal{L})\cup(\mathcal{L}∖\mathcal{L}_0)$, dove $\mathcal{L}_0$​ è il linguaggio ipotizzato dal learner.
>
>- ***Equivalence Query (EQ)***:
    Il learner propone l'automa $\mathcal{A}$ (o, equivalentemente, il linguaggio $\mathcal{L}_0$​ riconosciuto da $\mathcal{A}$) e chiede: "È $\mathcal{L}_0=\mathcal{L}$?"
    Il teacher risponde "sì", in tal caso il processo di apprendimento termina, oppure "no" fornendo un controesempio che evidenzi una parola in cui $\mathcal{L}_0$​ e $\mathcal{L}$ divergono.

Questa interazione iterativa permette al learner di aggiornare la propria ipotesi fino a quando non converge sull'automa corretto.



### Perché non bastano le Membership Query da sole?

Le membership query (MQ) da sole non sono sufficienti per garantire che il learner “vinci”, ovvero che riesca a identificare completamente il linguaggio target. Ciò è dovuto al fatto che, senza un controllo globale, <u>il learner potrebbe dover verificare un numero potenzialmente infinito di parole </u>per essere sicuro dell'esattezza della propria ipotesi. Infatti, per i linguaggi regolari (e ancor più per quelli ω-regolari) l'insieme delle possibili parole è infinito, e controllare ogni possibile membership senza una query globale (equivalence query) sarebbe impraticabile.

### Il Ruolo delle Equivalence Query

Le equivalence query (EQ) forniscono un controllo globale: quando il learner propone l'automa $\mathcal{A}$ ipotizzato, il teacher verifica se il linguaggio $\mathcal{L}(\mathcal{A})$ coincide con $\mathcal{L}$. In caso contrario, un controesempio guida il learner a correggere l'ipotesi. Anche se le EQ possono, da sole, richiedere tempo esponenziale nella verifica (per via della complessità della struttura del linguaggio), esse sono essenziali perché permettono una convergenza nel processo di apprendimento.

## Black-box learning

>! Nell'effettivo questo approccio permette di evitare di eseguire equivalence query perchè l'idea di base è che "se il 99% degli elementi appartiene al linguaggio reale ed è riconosciuto dal mio automa, perchè dare troppa importanza a quell'insignificante 1% che non è detto nemmeno che esista?"



## Funzioni su parole 
> Quindi l'approccio black-box in teoria mi permette di creare il trasduttore sequenziale che permette, dato un programma scritto in python, di calcolare il corrispettivo codice in javascript non conoscendo l'alfabeto di output? 



## Implicazioni del Learning Automata

Il modello di learning basato su membership ed equivalence query, inizialmente proposto per DFA (ad esempio con l'algoritmo $\mathcal{L}^*$ di Angluin), ha ispirato estensioni nel contesto degli ω-automata e delle logiche S1S/WS1S. L'idea centrale è sempre quella di avere un teacher che possiede una descrizione completa del linguaggio target e un learner che costruisce progressivamente un'ipotesi tramite feedback, anche se in questo caso il "gioco" si svolge in un ambiente in cui le proprietà $\omega$ (come la visita infinita di stati finali) devono essere verificate.


## Conclusioni

- Automata Learning offre un quadro in cui il learner, partendo solo dall'alfabeto, interagisce con un teacher tramite membership ed equivalence query per identificare un automa (sia per linguaggi finiti che per ω-linguaggi) che riconosca il linguaggio target.
- Le membership query forniscono informazioni locali, mentre le equivalence query consentono di verificare globalmente la correttezza dell'ipotesi.
- Nel dominio degli ω-linguaggi, la complessità aumenta a causa delle condizioni di accettazione (ad esempio, in automi di Büchi o Müller) e delle corrispondenti rappresentazioni logiche (S1S, WS1S), ma il modello di interazione tra teacher e learner rimane un potente strumento.
- Le connessioni con omega-regolarità e le logiche S1S/WS1S garantiscono che il potere espressivo dei modelli di automi utilizzati nel learning sia sufficiente a rappresentare le proprietà temporali e infinite dei sistemi reali, come quelli modellati in verifica formale.
- Infine, l'analisi comparativa tra automi di Büchi, Müller e Rabin fornisce ulteriori strumenti per affrontare problemi di learning in contesti dove il complementare, la determinizzazione e altre operazioni chiave giocano un ruolo fondamentale.

Questa visione integrata non solo approfondisce il metodo di learning degli automi ma ne evidenzia anche il collegamento con la teoria degli ω-linguaggi e la logica S1S/WS1S, argomenti cruciali nella verifica di sistemi reattivi e nella teoria dei linguaggi formali.