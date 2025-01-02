---
layout: post
title: Lecture 4 Network Security -Cryptographic Key Management and Distribution
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
# Key-distribution Center
Esistono varie modalità per la distribuzione delle chiavi tra due parti, A e B, in un contesto di crittografia:

1. A può selezionare una chiave e consegnarla fisicamente a B.
2. Una terza parte può selezionare la chiave e consegnarla fisicamente a A e B.
3. Se A e B hanno precedentemente utilizzato una chiave, una delle parti può trasmettere la nuova chiave all'altra, crittografata utilizzando la vecchia chiave.
4. Se A e B hanno ciascuno una connessione crittografata con una terza parte C, C può consegnare una chiave su collegamenti crittografati ad A e B.
5. Una variante di opzione 4 coinvolge un centro di distribuzione chiavi (KDC), che genera e distribuisce chiavi di sessione.

![[1715586000_grim.png]]
Le opzioni 1 e 2 richiedono la consegna manuale delle chiavi, una pratica ragionevole per la crittografia di collegamento ma scomoda per la crittografia end-to-end su una rete. La distribuzione manuale è problematica nei sistemi distribuiti, in cui ogni dispositivo può dover comunicare con molti altri dispositivi nel tempo.

La distribuzione delle chiavi a livello di rete richiede una quantità significativa di chiavi, che dipende dal numero di coppie di comunicazione supportate. Per la crittografia end-to-end a livello di applicazione, il numero di chiavi necessarie può essere ancora più elevato.

Inoltre, si discute l'utilizzo di un centro di distribuzione chiavi (KDC) per la crittografia end-to-end, dove il KDC genera e distribuisce chiavi di sessione tra le parti.

Si vede necessario quindi autenticare le parti che scambiano chiavi e l'uso di timestamp per limitare il tempo di scambio delle chiavi e/o la durata delle chiavi scambiate.
# Man-in-The-Middle attack
Per scegliere una chiave di sessione potrei pensare di usare un semplice schema come il seguente, dove A lascia che sia B a decidere la chiave di sessione K, per poi passarla attraverso la chiave pubblica di A ad A.
![[1715585974_grim 1.png]]
Il problema di questo schema è che è vulnerabile ad attacchi man-in-the-middle, difatti un soggetto C potrebbe banalmente fingersi A, intercettando la richiesta di B, inoltrando a B la chiave pubblica di C e l'ID di A, per poi intercettare la risposta di B, come si può vedere dal modello seguente 
![[1715585957_grim.png]]
Ovviamente questo approccio è sbagliato, ma mette in luce come sia essenziale prevenire questo genere di attacchi.
### Secured version
![[1715586033_grim.png]]
L'approccio in figura è migliore del precedente. Difatti in questo caso viene usata la chiave pubblica di B e 2 nonce, che permettono di evitare man-in-the-middle e replay attacks. Il problema in questo caso è che questa architettura non è facilmente scalabile.
Se invece di avere 2 entità ne avessimo 1000, ognuna di queste entità dovrebbe tener traccia di tutte le chiavi pubbliche degli altri 999 utenti. Chiaramente più cresce il numero più il numero di chiavi da memorizzare cresce e in generale saranno N*(N-1). Per questa ragione si è pensato di usare un'entità condivisa che gestisse i dati condivisi.
# Public-Key Directory
![[1715586171_grim.png]]
Una "public-key directory" è un servizio o una infrastruttura che memorizza e fornisce pubblicamente chiavi pubbliche associate a identità specifiche. Nel contesto della gestione e distribuzione delle chiavi (key management and distribution), una directory delle chiavi pubbliche è fondamentale per consentire la crittografia asimmetrica, in cui una coppia di chiavi (una pubblica e una privata) viene utilizzata per crittografare e decrittografare i dati.

1. **Registrazione delle chiavi pubbliche:** Gli utenti o le entità generano una coppia di chiavi (pubblica e privata) e registrano la loro chiave pubblica nella directory. Questo processo di registrazione può includere l'associazione della chiave pubblica con un'identità specifica, come un nome utente o un indirizzo email.
    
2. **Recupero delle chiavi pubbliche:** Quando un mittente ha bisogno di crittografare dati per un destinatario specifico, può recuperare la chiave pubblica di quel destinatario dalla directory utilizzando la sua identità. Questo consente al mittente di crittografare i dati con la chiave pubblica del destinatario, assicurando che solo il possessore della chiave privata associata possa decifrarli.
    
3. **Distribuzione delle chiavi pubbliche:** La directory delle chiavi pubbliche può essere accessibile tramite vari mezzi, come servizi online, protocolli di rete specifici o infrastrutture di sicurezza aziendale. È importante garantire che la directory sia affidabile e protetta da accessi non autorizzati, in modo che le chiavi pubbliche possano essere recuperate solo da chi ne ha effettivamente bisogno.
    
4. **Revoca delle chiavi pubbliche:** In situazioni in cui una chiave pubblica non è più sicura o è stata compromessa, è necessario essere in grado di revocarla dalla directory in modo che non venga più utilizzata per crittografare i dati. Questo processo di revoca richiede una gestione accurata per garantire che le chiavi compromesse siano prontamente rimosse dalla directory.
# X.509 Certificates
X.509 definisce un framework per offrire servizi di autenticazione dalle directory X.500 ai suoi utenti

È basato su cifratura a chiave pubblica e firme digitali.

Ogni certificato contiene la chiave pubblica dell'utente ed è firmato con la chiave pubblica di un'autorità fidata di certificazione (CA).



Il cuore dello schema X.509 è il certificato a chiave pubblica associato a ciascun utente. 
![[1715586742_grim.png]]

## Certification Authority (CA)
L'entità che si occupa di garantire l'autenticità e della creazione dei certificati è la certification autority. Il server che contiene le associazioni utente-certificato non è responsabile per la creazione delle chiavi, ma solo del rendere disponibile una location per ottenere i certificati. 

Le caratteristiche di una CA sono:
- versione
	- 1, versione di default
	- 2, versione se sono presenti i campi *issuer unique identifier* o il *subject unique identifier*
	- 3, versione se una o più estensioni sono presenti
- numero seriale
	- un numero che identifica univocamente il certificato da parte della CA che l'ha generato
- identificatore dell'algoritmo di signature
	- non ha molta utilità, specifica l'algoritmo usato per la firma
- nome della CA Che ha creato e firmato il  certificato
- periodo di validità
- nome dell'utente a cui appartiene il certificato
- informazioni sulla chiave pubblica del soggetto
- identificatore univoco del generatore
- identificatore univoco del soggetto
- estensioni
	- insieme di una o più campi 
	- sono state aggiunte a partire dalla versione 3
- signature

![[1715587983_grim.png]]

# Certificate Revocation
servono per poter revocare un certificato prima della data di scadenza. Può essere utile revocare un certificato anticipatamente quando ad esempio la chiave privata viene compromessa, quando il certificato della CA si ritiene sia stato bucato, oppure quando l'utente non è più certificato dalla CA
# Public-Key Infrastructure (PKI)
![[1715590532_grim.png]]

Le componenti essenziali di una PKI sono:
- **End Entity**
- **Certification Authority (CA)**
	- autorità garantita da uno o più utenti che crea e assegna i certificati a chiave pubblica
	- responsabile anche per pubblicare i Certificate Revocation List (CRLs), che servono per identificare i certificati che sono stati revocati prima della data di scadenza
- **Registration Authority (RA)**
	- componenti opzionale
	- serve per delegare alcuni compity del CA
	- verificare l'identità e ottenere certificati per una chiave pubblica
- **Repository**
	- specifica ogni metodo per memorizzare e recuperare le informazioni associate a una PKI come ad esempio i certificati di chiave pubblica e i CRLs
	- può essere una X.500-based directory con un accesso client via LDAP (Lightweight Directory Access Protocol)
	
- **Relying party**
	- qualsiasi utente o agente che dipende dai dati di un certificato per poter prendere decisioni



## Domande:


**Che differenza c'è tra una public-key directory e una certificate autority (CA)?**
Una public-key directory e una certificate authority (CA) sono entrambe componenti cruciali nell'infrastruttura crittografica, ma svolgono ruoli leggermente diversi:

1. **Public-Key Directory:**
   - Una public-key directory memorizza e fornisce chiavi pubbliche associate a identità specifiche.
   - Le chiavi pubbliche nella directory possono essere utilizzate per crittografare dati in modo che solo il possessore della chiave privata associata possa decifrarli.
   - La directory fornisce un servizio di accesso pubblico per recuperare chiavi pubbliche, senza necessariamente garantire l'affidabilità o l'autenticità delle chiavi stesse.
   - La directory può essere utilizzata per scopi generici di crittografia e firma digitale, ma non fornisce garanzie sulla validità delle chiavi.

2. **Certificate Authority (CA):**
   - Una certificate authority è un'entità affidabile che emette certificati digitali che collegano una chiave pubblica a un'identità specifica (ad esempio, un'organizzazione o un sito web).
   - I certificati digitali sono firmati digitalmente dalla CA, fornendo una garanzia dell'autenticità delle informazioni contenute nel certificato.
   - Oltre alla chiave pubblica, il certificato contiene informazioni sull'entità a cui appartiene la chiave pubblica, come il nome dell'organizzazione e l'indirizzo email.
   - Le CA svolgono un ruolo critico nell'assicurare che le chiavi pubbliche siano autentiche e non siano state compromesse.
   - I certificati emessi dalle CA sono ampiamente utilizzati in contesti come HTTPS (protocollo di trasferimento sicuro ipertestuale) per garantire la sicurezza delle comunicazioni su Internet.

In sintesi, mentre entrambe le public-key directory e le certificate authority sono fondamentali per la crittografia e la sicurezza delle comunicazioni digitali, le CA forniscono un livello aggiuntivo di affidabilità e autenticità attraverso l'emissione di certificati digitali firmati digitalmente.



