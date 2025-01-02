---
layout: post
title: Lecture 5 Network Security - User authentication
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
# Authentication Principles
**Identità digitale**: rappresentazione univoca di un soggetto coinvolto in una transazione online

**Identity proofing**: stabilisce che un soggetto è realmente chi dice di essere. Il processo di identity proofing prevede la collezione, la validazione e la verifica delle informazioni associate a una persona

**Digital authentication**: il processo per determinare la validità di uno o più authenticators usati per creare una identità digitale

![[1715593554_grim.png]]
- **Credential service provider (CSP):** A trusted entity that issues or registers subscriber authenticators. For this purpose, the CSP establishes a digital credential for each subscriber and issues electronic credentials to subscribers. A CSP may be an independent third party or may issue credentials for its own use.

- **Verifier**: An entity that verifies the claimant’s identity by verifying the claimant’s possession and control of one or two authenticators using an authentication protocol. To do this, the verifier may also need to validate credentials that link the authenticator(s) to the subscriber’s identifier and check their status. 

- **Relying party (RP):** An entity that relies upon the subscriber’s authenticator(s) and credentials or a verifier’s assertion of a claimant’s identity, typically to process a transaction or grant access to information or a system. 

- **Applicant**: A subject undergoing the processes of enrollment and identity proofing. 

- **Claimant**: A subject whose identity is to be verified using one or more authentication protocols. 

- **Subscriber**: A party who has received a credential or authenticator from a CSP


Una cosa importante sull'autenticazione è schematizzata in figura
![[1715593963_grim.png]]
Come si può vedere, l'autenticazione si basa sempre su una o più di queste 3 cose:
- qualcosa che conosci (password, pin, passphrases, la risposta a una domanda specifica, ...)
- qualcosa che possiedi (hardware token, ...)
- qualcosa che sei o fai (caratteristiche biometriche come retina, impronta digitale, voce, scrittura, ritmo di scrittura, ...)

![[1715594174_grim.png]]
![[1715594196_grim.png]]

# Replay Attacks


Per poter contrastare gli attacchi replay, si può:
- ***Timestamps***: Party A accepts a message as fresh only if the message contains a timestamp that, in A’s judgment, is close enough to A’s knowledge of current time. This approach requires that clocks among the various participants be synchronized. 

- ***Challenge/response:*** Party A, expecting a fresh message from B, **first sends B a nonce (challenge) and requires** that the **subsequent message (response) received from B contain the correct nonce value.**

**NOTA BENE!!**
1. l'approccio che usa timestamp non dovrebbe essere usato su applicazioni che sono connection-oriented, per via delle problematiche che questi sistemi presentano (ad esempio latenze di rete, sincronizzazione dei clocks, ...)
2. l'approccio challenge-response non andrebbe usato su tipi di connessioni connectionless, perchè richiede una fase di negoziazione che non è previsto in sistemi connectionless. Ad esempio UDP è connectionless. Se avessi una nonce non potrei garantire che i pacchetti vengano spediti nell'ordine atteso
- ***Sequence numbers***:  
# Remote User-Authentication Using Symmetric Encryption

# Needham-Schroeder Protocol
L'idea è quella di usare un'entità terza come mediatrice.
La terza parte fidata è rappresentata dal KDC (Key Distribution Center).

Uno schema del protocollo è il seguente:
![[1715599425_grim.png]]

Come si può vedere, le comunicazioni tra B e KDC non avvengono direttamente. 
![[1715599384_grim.png]]
La chiave di sessione viene distribuita correttamente, ma questo protocollo è comunque vulnerabile agli attacchi di tipo replay se una delle vecchie chiavi di sessione viene compromessa. In quel caso:
- il messaggio 3 può essere spedito di nuovo
- il messaggio 4 intercettato, decifrato con la vecchia chiave e risposto correttamente
- a quel punto si può convincere B di star comunicando con A 

Il trucco è usare un timestamp o usare una nonce extra.
# Denning-Sacco Protocol
È la versione del protocollo di Needham-Schroeder, ma con il timestamp al posto delle nonce per evitare replay attacks.

1. A -> S: A,B
2. S -> A: $E_{Kas}$ (B, Kab, T, $E_{Kbs}$ (A, Kab, T))
3. A -> B: $E_{Kbs}$(A, Kab, T)

##### Suppress-replay attack
Il problema è che se i clock non sono sincronizzati il protocollo smette di funzionare. In questo caso c'è un attacco chiamato *suppress-replay attack* che consiste nell'intercettare un messaggio dal sender e spedirlo di nuovo dopo quando il timestamp del messaggio diventa quello corrente dal lato del destinatario.

# Kerberos
Servizio di autenticazione che offre un centralized authentication server basato soltanto su cifratura simmetrica, la cui funzione è di autenticare gli utenti per i server e i server per gli utenti. 

I requisiti principali di Kerberos sono: 
- **sicurezza**: un ascoltatore non deve essere in grado di ottenere le informazioni necessarie a impersonare un utente
- **affidabilità**: il server deve essere affidabile, in quanto gli utenti avranno bisogno di esso per poter applicare il protocollo  
- **scalabilità**: il sistema dev'essere in grado di supportare un numero anche molto grande di utenti
- **trasparenza**: gli utenti non dovrebbero essere a conoscenza del fatto che l'autenticazione va oltre al semplice inserire una password

### Version 4
La versione 4 di Kerberos fa uso di DES per il servizio di autenticazione, oltre a un Authentication Server (AS) che conosce le password di tutti gli utenti e le memorizza all'interno di un database centralizzato. L'AS inoltre memorizza una segreto univoco per ogni server.

Una volta che l'AS verifica l'autenticità di un utente, genera un ticket contenente lo user ID, un indirizzo di rete e il server ID. Questo ticket è cifrato usando la stessa chiave segreta condivisa tra l'AS e il server.

Infine il Ticket-granting server (TGS) serve per richiedere i ticket agli utente che sono stati autenticati dall'AS.
Ogni volta che l'utente richiede l'accesso a un nuovo servizio il client si invia al TGS il ticket per autenticare sè stesso. Il client memorizza ogni service-granting ticket e lo usa per autenticare i suoi utenti a un server ogni volta che un particolare servizio viene richiesto.


Ci sono alcune cose da tenere in considerazione:
- il **periodo di validità del ticket-granting ticket** non può essere nè troppo corto nè troppo lungo. Nel primo caso l'utente dovrebbe inserire troppo frequentemente la password. Nel secondo caso l'attaccante avrebbe più opportunità di eseguire un replay attack
- il **servizio di rete** deve essere in grado di provare che la persona che usa il ticket sia la stessa persone che ha richiesto quel ticket
- **i server devono autenticarsi** a loro volta agli utenti

![[1715602102_grim.png]]



![[1715602147_grim.png]]

![[1715602171_grim.png]]


Un sistema Kerberos è composto da un server Kerberos, un certo numero di clients e un numero di application servers che richiedono che:
- il server Kerberos possieda gli user ID e gli hash delle password di tutti gli utenti del database 

- il server Kerberos deve avere una chiave segreta condivisa con ognuno dei server

- il server Kerberos in ogni reame condivide una chiave segreta con il server di ogni altro reame. I due server Kerberos si sono registrati tra loro.
![[1715602223_grim.png]]

### Differences between Kerberos v4 and v5
- Kerberos v4 non era pensato per essere general purpose, mentre v5 si.
- Kerberos v4 usa DES, mentre v5 usa AES

| Kerberos v4                                              | Kerberos v5                                                                                                                                                                     |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| non era pensato per essere general purpose               | è pensato per essere general purporse                                                                                                                                           |
| usa DES come algoritmo di cifratura                      | usa AES come algoritmo di cifratura                                                                                                                                             |
| richiede l'uso di indirizzi IP                           | possono essere usati tutti i tipi di indirizzi di rete                                                                                                                          |
| il mittente usa un ordinamento dei byte a sua scelta     | tutti i messaggi sono definiti usando una Abstract Syntax Notation One (ASN.1) e Basic Encoding Rules (BER) che permette di evitare le ambiguità nel byte ordering              |
| i valori di lifetime erano codificati usando 4-bit       | il lifetime è indefinitamente lungo essendo che viene specificato lo start time e l'end time                                                                                    |
| viene usato DES in modalità PCBC che è vulnerabile       | viene usato un meccanismo di integrità esplicito permettendo l'uso di CBC mode. Inoltre viene usato un checksum o un hash code aggiunto al messaggio prima dell'encryption      |
| non ci sono meccanismi per ostacolare i password attacks | viene usato un meccanismo di preautenticazione che rende più difficile applicare un attacco alle password, anche se non lo previene                                             |
| è vulnerabile ai replay attack                           | è possibile che il client e il server negozino una chiave di sub-sessione da usare per una sola connessione. Un nuovo accesso del client richiederebbe una nuova subsession key |


![[1715602261_grim.png]]

# Mutual Authentication
![[1715609635_grim.png]]

![[1715609724_grim.png]]
# One-Way Authentication
È possibile che non mi interessi che entrambi gli agenti della comunicazione vengano autenticati o proteggano la confidenzialità. 

In questo caso è sufficiente usare uno schema one-way come il seguente:
$$
A \rightarrow B:\; M ||E(PR_{a}, H(M))
$$
Il problema è che se un utente terzo intercetta il messaggio (per esempio se M è un progetto segreto che Bob vuole inviare al centro dei brevetti), potrebbe rimuovere la firma, inserire la sua, e inviarlo prima di Bob rubandogli l'idea.

Per fare in modo che questo non sia possibile è sufficiente cifrare tutto con la chiave pubblica di B in modo che il destinatario sia l'unico in grado di leggere il messaggio
$$
A \rightarrow B:\;E(PU_{b}[M||E(PR_{a},H(M))])
$$
E se volessi fare in modo che non si possa eseguire un attacco replay, potrei aggiungere un timestamp in questo modo
$$
A \rightarrow B: M||E(PR_{a}, H(M)||E(PR_{as},[T||ID_{a}||PU_{a}]))
$$
# Federated Identity Management
Il Federated Identity Management, o Gestione delle Identità Federata, è un approccio alla gestione delle identità digitali che consente a più organizzazioni o sistemi di condividere in modo sicuro le informazioni di autenticazione e autorizzazione tra di loro. In pratica, questo significa che un utente può utilizzare le proprie credenziali di accesso (come nome utente e password) presso diverse piattaforme o servizi, senza la necessità di creare e memorizzare un set separato di credenziali per ciascun sistema.

Il concetto fondamentale del Federated Identity Management è quello di creare un ambiente di trust tra le organizzazioni partecipanti, in modo che possano condividere in modo sicuro le informazioni di identità degli utenti. Ciò viene solitamente realizzato attraverso l'uso di standard e protocolli di autenticazione e autorizzazione, come SAML (Security Assertion Markup Language), OAuth e OpenID Connect.

Le principali caratteristiche e vantaggi del Federated Identity Management includono:

1. **Interoperabilità:** Le organizzazioni possono collaborare e integrare in modo più efficiente i loro servizi e sistemi, consentendo agli utenti di accedere a risorse distribuite su più piattaforme.
    
2. **Migliorata esperienza utente:** Gli utenti possono accedere a diversi servizi utilizzando un'unica coppia di credenziali, semplificando il processo di accesso e riducendo il numero di password da ricordare.
    
3. **Riduzione del rischio di sicurezza:** L'utilizzo di protocolli standardizzati e sicuri per lo scambio di informazioni di identità riduce il rischio di accessi non autorizzati e frodi.
    
4. **Miglior controllo dell'accesso:** Le organizzazioni possono mantenere il controllo sull'accesso alle proprie risorse, anche quando queste sono condivise con altre organizzazioni, attraverso politiche di autorizzazione e regole di accesso.
    

Complessivamente, il Federated Identity Management è un approccio fondamentale per supportare la collaborazione tra organizzazioni e fornire agli utenti un'esperienza di accesso più semplice e sicura attraverso diverse piattaforme e servizi online.
# OAuth2 
https://itnext.io/an-oauth-2-0-introduction-for-beginners-6e386b19f7a9

OAuth2 è un meccanismo di autenticazione che permette a siti web e web apps di richiedere un restricted access a un account di un altra applicazione

OAuth2 è quello che viene usato quando ad esempio accedo a un'applicazione X usando il mio account Google. L'applicazione non viene a conoscenza delle credenziali del mio account Google, ma solo dell'autorizzazione all'accesso.
-> questo è ottenuto ottenendo un **access_token** da un server di autorizzazione.

Le componenti di OAuth2 sono:
- client application
- resource owner 
- OAuth service provider

Ci sono diverse varianti di OAuth2

| Nome                     | Descrizione                                                                                                                                                                        |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authorization Code Grant | viene ottenuto l'access_token, il codice per richiedere l'access token è rilasciato da un front-end dopo che l'utente fa il login. L'access_token viene richiesto server-side      |
| Implicit Grant           | dopo l'accesso il token è comunicato immediatamente                                                                                                                                |
| Client Credential Grant  | l'access_token è comunicato sul server, autenticando il client e non l'utente                                                                                                      |
| Password Grant           | l'access_token è comunicato immediatamente con una singola richiesta contenente tutte le informazioni di login. Può essere più semplice da implementare ma ha alcune complicazioni |
![[1715775767_grim.png]]

Quale flow dovrei usare?
Posso memorizzare il client secret in un posto sicuro?
- sì, il client è una web application? 
	- sì, l'utente si fida a usare le sue "Oauth credentials"? 
		- sì, **PASSWORD GRANT**
		- no, **AUTHORIZATION CODE GRANT**
	- no, **CLIENT CREDENTIAL GRANT**
- no, **IMPLICIT GRANT**


Il processo di autorizzazione si divide in 2 fasi dove gli attori sono:
- User: the person who wants to be authenticated, to access protected information. 
- Client App: the client is usually a web application. The application must have both a front-end and a back-end, later we’ll see why. This means a pure Front-end application (Javascript, React…) cannot implement this flow but can use the Implicit grant one. 
- Authorization Server: is the component that performs the authentication and the authorization, it handles login requests, user authentication, token generation, and security validations. 
- Resource Server: it exposes resources, as they could be REST API. After the Client App obtains the access_token, it will use it to call the Resource Server.

![[1715776069_grim.png]]
![[1715776092_grim.png]]

