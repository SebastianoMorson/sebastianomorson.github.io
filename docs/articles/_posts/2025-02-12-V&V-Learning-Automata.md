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

## Learning Strategy

L'idea dell'algoritmo che porta alla costruzione del DFA che riconosce il linguaggio del learner è il seguente:

1. Inizializzazione
    
    - Sia $S=\{\epsilon \}$ e $T=\{\epsilon\}$, ovvero iniziamo con solo la stringa vuota.
   
    - $S$ rappresenta gli stati ipotetici del DFA in costruzione.
   
    - $T$ contiene suffissi usati per distinguere gli stati.

2. Loop principale
   
    - L'algoritmo termina al massimo dopo index($\sim L_0$) iterazioni, ovvero dopo aver trovato il numero minimo di stati del DFA cercato.

3. Verifica della T-completezza
    - Un insieme S è T-completo se, per ogni stato $s$ in $S$, il suo comportamento sulle stringhe in $T$ è unico.
    
    - Se $S$ non è T-completo, allora si aggiunge una nuova stringa a $S$, estendendolo con una nuova lettera $a$.

4. Costruzione del DFA candidato
    - Si costruisce un DFA con:
      - Stati $S$
      - Transizioni definite da $\delta(s,a)=s_a$ (concatenazione)
      - Stato iniziale $\epsilon$
      - Stati finali determinati da $Membership(s')$, cioè quelli che appartengono a $L_0$.

5. Verifica di Equivalenza
    - Se il DFA costruito è corretto ($Equivalence(A)$ è vera), allora l'algoritmo restituisce il DFA.
    
    - Altrimenti, se il DFA non è corretto, viene fornito un controesempio $w$.

6. Aggiornamento dell'insieme $T$
    - Si aggiungono i suffissi del controesempio $w$ a $T$ per affinare la distinzione tra stati.

    - Ritorna al punto 3 e ripete il processo con il DFA aggiornato.



**PSEUDOCODICE:** 

```javascript
S = T = {ε} // S is T-minimal, possibly not T-complete
loop // this will loop at most index(∼L0) times
  while S NOT T-complete
    let s ∈ S and a ∈ Σ such that
      ∀s’ ∈ S ∃t∈T Membership(s a t) ≠ Membership(s’ t)
    S = S ∪ {s a}
  A = DFA with state set S, transitions δ(s,a) = sa
  initial state ε, final states s’ s.t. Membership(s’)
  if Equivalence(A) // this surely happens
    return A // when |S| = index(∼L0)
  else
    let w be the counter-example of equivalence 
    T = T ∪ {suffixes of w} // S becomes T-incomplete and will grow at next iteration…
```

## Matrici di Hankel
È possibile sfruttare le matrici di Hankel per avere un modo più intuitivo per applicare la procedura.

In pratica si costruisce una matrice in cui le righe sono le classi S e le colonne invece gli elementi di T.

Ciascuna cella può contenere il valore 0 o 1 a seconda che $Membership(s\cdot t)$ restituisca yes o no.

Dopodichè il procedimento è identico all'algoritmo visto in precedenza.

Se due righe sono uguali significa che lo stato è il medesimo.

📌 Esempio concreto di costruzione di un DFA

Supponiamo che il learner parta con:
$S=\{\epsilon,a,b\},T=\{\epsilon,a\}$

La matrice iniziale:

| H[S,T] | ε	| a | 
| :--: | :--: | :--:|
|ε | 0 | 0 | 
|a | 0 | 0 | 
|b | 0 | 0 | 

Il learner scopre che tutti i prefissi attuali sembrano uguali (tutte le righe sono uguali), quindi per ora assume un solo stato.

Ora il learner prova nuove parole:
- Se chiede per abab, scopre che $H(ab,\epsilon)=1$ (perché "ab" appartiene a L).
- Aggiunge ab a S e aggiorna la matrice.

Ora la matrice è:

| H[S,T] | ε	| a | 
| :--: | :--: | :--:|
|ε | 0 | 0 | 
|a | 0 | 0 | 
|b | 0 | 0 | 
| ab | 1 | 0 |

Il learner nota che la riga di ab è diversa dalle altre, quindi introduce un nuovo stato per ab.

Ora può costruire il DFA:

- Stato iniziale $q_0$ (per ε).
- Stato $q_1$​ per $a$.
- Stato $q_2$​ per $ab$ (stato finale).

Aggiunge le transizioni:

  - $q_0 \to_a q_1$
  - $q_1 \to_b q_2$ (perché abab è accettato)
  - q2​ è finale.

Alla fine, ripete il controllo con un’Equivalence Query.

## Funzioni su parole 
>L'approccio black-box mi permette di creare il trasduttore sequenziale che permette, dato un programma scritto in python, di calcolare il corrispettivo codice in javascript non conoscendo l'alfabeto di output? 



## Implicazioni del Learning Automata

Il modello di learning basato su membership ed equivalence query, inizialmente proposto per DFA (ad esempio con l'algoritmo $\mathcal{L}^*$ di Angluin), ha ispirato estensioni nel contesto degli ω-automata e delle logiche S1S/WS1S. L'idea centrale è sempre quella di avere un teacher che possiede una descrizione completa del linguaggio target e un learner che costruisce progressivamente un'ipotesi tramite feedback, anche se in questo caso il "gioco" si svolge in un ambiente in cui le proprietà $\omega$ (come la visita infinita di stati finali) devono essere verificate.


## Conclusioni

- Automata Learning offre un quadro in cui il learner, partendo solo dall'alfabeto, interagisce con un teacher tramite membership ed equivalence query per identificare un automa (sia per linguaggi finiti che per ω-linguaggi) che riconosca il linguaggio target.
- Le membership query forniscono informazioni locali, mentre le equivalence query consentono di verificare globalmente la correttezza dell'ipotesi.
- Nel dominio degli ω-linguaggi, la complessità aumenta a causa delle condizioni di accettazione (ad esempio, in automi di Büchi o Müller) e delle corrispondenti rappresentazioni logiche (S1S, WS1S), ma il modello di interazione tra teacher e learner rimane un potente strumento.
- Le connessioni con omega-regolarità e le logiche S1S/WS1S garantiscono che il potere espressivo dei modelli di automi utilizzati nel learning sia sufficiente a rappresentare le proprietà temporali e infinite dei sistemi reali, come quelli modellati in verifica formale.
- Infine, l'analisi comparativa tra automi di Büchi, Müller e Rabin fornisce ulteriori strumenti per affrontare problemi di learning in contesti dove il complementare, la determinizzazione e altre operazioni chiave giocano un ruolo fondamentale.
