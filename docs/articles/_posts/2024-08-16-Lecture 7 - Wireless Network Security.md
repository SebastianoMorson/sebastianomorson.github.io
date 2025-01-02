---
layout: post
title: Lecture 7 Network Security - Wireless Network Security
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
I fattori di rischio maggiori per le reti wireless sono:
- **channel**: la comunicazione è in genere broadcast quindi più suscettibile ad attacchi di sniffing e jamming 
- **mobility**: posso spostare facilmente un dispositivo wireless dove va meglio all'attaccante senza inficiare per questo la sua funzionalità
- **resources**: i dispositivi wireless come smartphone, etc hanno risorse limitate
- **accessibility**: i device wireless sono tipicamente posizionati in luoghi facilmente accessibili

I thread di sicurezza più significativi sono:
- accidental association
- malicious association
- ad hoc networks non hanno un punto centrale di controllo
- nontraditional networks possono essere più facili da manipolare
- MAC spoofing
- man-in-the-middle attacks
- denial of service
- network injection

Alcune tecniche per contrastare l'eavesdropping sono:
- signal-hiding techniques
	- posso nascondere l'SSID o usare SSID non standard
	- posso ridurre la potenza del segnale per diminuire la portata della comunicazione così da evitare che l'attaccante si trovi nello spazio di comunicazione
- encryption
	- posso cifrare la comunicazione così da rendere illeggibile la comunicazione a un malintenzionato

Lo standard IEEE 802.1x serve per prevenire l'accesso non autorizzato attraverso port-based network access control (praticamente assegno porte determinate per la comunicazione)

Poi chiaramente per mettere in sicurezza l'access point e la rete wireless ci sono politiche utili ma banali come quella di usare la cifratura, antivirus, antispyware, firewall, cambiare password, etc.


## Terminologia dello standard IEEE 802.11
![[1717281536_grim.png]]

## IEEE 802.11 Protocol Stack
![[Pasted image 20240602004209.png]]

![[1717281760_grim.png]]

![[1717281787_grim.png]]


### Tipi di transizione basati sulla mobilità:

1. **Nessuna transizione**:
   - **Descrizione**: Una stazione di questo tipo è o stazionaria (non si muove) o si muove solo all'interno dell'intervallo di comunicazione diretta delle stazioni che comunicano all'interno di un singolo Basic Service Set (BSS).
   - **Dettagli**: In questa situazione, la stazione rimane sempre connessa alla stessa rete e non ha bisogno di cambiare punto di accesso. Non ci sono complicazioni aggiuntive in termini di indirizzamento o gestione della connessione.

2. **Transizione BSS**:
   - **Descrizione**: Questo tipo di transizione avviene quando una stazione si sposta da un BSS a un altro BSS all'interno dello stesso Extended Service Set (ESS).
   - **Dettagli**: Un BSS è una rete wireless che comprende un punto di accesso (AP) e le stazioni (dispositivi) che comunicano con esso. Un ESS è una rete più ampia composta da più BSS collegati tra loro. Quando una stazione si sposta tra BSS all'interno dello stesso ESS, è necessario che il sistema di indirizzamento possa riconoscere la nuova posizione della stazione per garantire la consegna dei dati corretta. Questo comporta una certa gestione da parte della rete per mantenere la continuità della connessione.

3. **Transizione ESS**:
   - **Descrizione**: Questo tipo di transizione si verifica quando una stazione si sposta da un BSS in un ESS a un BSS in un altro ESS.
   - **Dettagli**: Quando una stazione attraversa i confini di un ESS e entra in un altro, mantenere le connessioni di livello superiore supportate dal protocollo 802.11 diventa problematico. La continuità del servizio non può essere garantita e è probabile che si verifichi un'interruzione del servizio. Questo è dovuto al fatto che gli ESS sono tipicamente gestiti da domini di rete differenti che potrebbero non avere le infrastrutture necessarie per gestire senza soluzione di continuità il trasferimento delle sessioni di rete della stazione.

### Servizi di Associazione:

#### Context
- **Scopo del servizio di distribuzione**:
  - Per consegnare un messaggio all'interno di un sistema di distribuzione (DS), il servizio di distribuzione deve conoscere l'identità del punto di accesso (AP) a cui il messaggio deve essere consegnato affinché raggiunga la stazione di destinazione. Questo è fondamentale per garantire che i dati inviati attraverso la rete raggiungano il corretto destinatario.

#### Servizi correlati all'associazione con l'AP nel BSS corrente:
1. **Associazione**:
   - **Descrizione**: Stabilisce un'associazione iniziale tra una stazione e un AP.
   - **Dettagli**: Questo è il processo iniziale tramite il quale una stazione (come un computer portatile o uno smartphone) si connette a un punto di accesso specifico all'interno di un BSS. L'associazione consente alla stazione di essere riconosciuta e di comunicare con la rete attraverso l'AP.

2. **Riassegnazione**:
   - **Descrizione**: Permette di trasferire un'associazione stabilita da un AP a un altro, consentendo a una stazione mobile di spostarsi da un BSS a un altro.
   - **Dettagli**: Quando una stazione si sposta fisicamente e lascia il raggio di copertura di un AP, deve stabilire una nuova associazione con un altro AP che si trova nel nuovo BSS in cui si trova ora. La riassegnazione assicura che la connessione di rete rimanga continua e che le sessioni attive non vengano interrotte.

3. **Disassociazione**:
   - **Descrizione**: Una notifica da parte di una stazione o di un AP che un'associazione esistente è terminata.
   - **Dettagli**: Questo servizio notifica la fine di un'associazione tra una stazione e un AP. La disassociazione può essere iniziata sia dalla stazione che dall'AP. Una volta disassociata, la stazione non può più comunicare attraverso quell'AP finché non viene stabilita una nuova associazione.
### Sicurezza delle LAN Wireless IEEE 802.11i


#### Esigenza di Servizi e Meccanismi di Sicurezza Robusti
Con l'aumento dell'uso delle LAN wireless, è cresciuta la necessità di implementare servizi e meccanismi di sicurezza robusti per proteggere i dati trasmessi e garantire la privacy e l'integrità delle comunicazioni.

#### Wired Equivalent Privacy (WEP)

- **Descrizione**: WEP è la porzione del protocollo 802.11 dedicata alla privacy.
- **Dettagli**: Questo standard iniziale mirava a fornire un livello di sicurezza comparabile a quello delle reti cablate, ma conteneva gravi vulnerabilità che ne compromettevano l'efficacia. Le principali debolezze includevano problemi con la gestione delle chiavi di cifratura e l'algoritmo di cifratura stesso, rendendo WEP facilmente attaccabile.

#### Wi-Fi Protected Access (WPA)

- **Descrizione**: WPA è un insieme di meccanismi di sicurezza progettati per eliminare la maggior parte dei problemi di sicurezza presenti nello standard 802.11 originale.
- **Dettagli**: Basato sullo stato attuale dello standard 802.11i, WPA ha introdotto miglioramenti significativi rispetto a WEP, tra cui l'uso di chiavi dinamiche e una migliore autenticazione. WPA include il Temporal Key Integrity Protocol (TKIP) per migliorare la cifratura e l'integrità dei messaggi.

#### Robust Security Network (RSN)

- **Descrizione**: RSN rappresenta la forma finale e più sicura dello standard di sicurezza per reti wireless.
- **Dettagli**: RSN, parte integrante dello standard 802.11i, include il supporto per il protocollo Advanced Encryption Standard (AES) per una cifratura forte e una gestione avanzata delle chiavi. Fornisce un livello di sicurezza molto più elevato rispetto a WEP e WPA, affrontando tutte le vulnerabilità note e implementando meccanismi di sicurezza avanzati per garantire la protezione delle comunicazioni wireless.


Il problema di WEP era dato dal fatto che era vulnerabile all'attacco del compleanno, per cui si riusciva a forgiare nuovi messaggi o addirittura a scoprire la chiave di cifratura.

### Correzione del WEP: Wi-Fi Protected Access (WPA & WPA2, 802.11i)

#### Profili WPA e WPA2

WPA e WPA2 introducono diversi profili di sicurezza per adattarsi a diverse esigenze di rete:

- **WPA-Enterprise**: 
    - **Descrizione**: Progettato per essere utilizzato con il protocollo 802.1X per l'autenticazione.
    - **Dettagli**: Questo profilo prevede l'uso di un server di autenticazione, come un server Radius, per verificare l'identità degli utenti e definire una chiave condivisa. È adatto per ambienti aziendali e reti che richiedono un livello elevato di sicurezza.

- **WPA-Personal**:    
    - **Descrizione**: Utilizza chiavi pre-condivise (PSK).
    - **Dettagli**: Questo profilo è più semplice e utilizza una chiave pre-condivisa che è generalmente la stessa per tutti i nodi della rete, simile a quanto avveniva con WEP. È adatto per reti domestiche o piccole reti aziendali.

#### Autenticazione e Handshake

In entrambi i profili WPA-Enterprise e WPA-Personal, dopo la fase di autenticazione iniziale, viene utilizzato un **4-way handshake** per generare una chiave di sessione (PTK) tra ciascun client (supplicant) e il punto di accesso (AP). Questo processo assicura che ogni comunicazione tra un client e l'AP sia protetta da una chiave unica.

#### Caratteristiche delle Chiavi e degli IV

- **Initialization Vector (IV)**:
    - **Dimensione**: 48 bit.
    - **Dettagli**: Gli IV sono utilizzati in combinazione con le chiavi di cifratura per garantire la sicurezza dei dati trasmessi. Tuttavia, non tutti gli IV sono accettabili, ad esempio, le lunghe ripetizioni degli IV non sono ammesse perché possono compromettere la sicurezza.

- **Chiavi di Cifratura**: 
    - **Dimensione**: 128 bit per WPA e 256 bit per WPA2.
    - **Dettagli**: Le chiavi di cifratura più lunghe in WPA2 offrono una sicurezza maggiore rispetto a WPA.

#### Capacità di Trasmissione Sicura

- **$n(0.5) = 1.18\cdot2^{24} = 20\cdot10^6$ pacchetti**:
    - **Dettagli**: Questo calcolo indica la quantità di dati (fino a 28 GB) che possono essere trasmessi in modo sicuro prima che si possa iniziare a ripetere gli IV. La ripetizione degli IV può ridurre la sicurezza, quindi è importante monitorare la quantità di dati trasmessi.


![[1717282546_grim.png]]

![[1717282564_grim.png]]


### Fasi del RSN
![[1717282583_grim.png]]
1. **Discovery (Scoperta)**:
    
    - **Descrizione**: Un punto di accesso (AP) utilizza messaggi chiamati Beacon e Probe Responses per pubblicizzare la sua politica di sicurezza IEEE 802.11i.
    - **Dettagli**: La stazione (STA) utilizza questi messaggi per identificare un AP di una WLAN con cui desidera comunicare. La STA si associa all'AP, utilizzando le informazioni contenute nei Beacon e Probe Responses per selezionare il suite di cifratura e il meccanismo di autenticazione appropriato quando vengono presentate delle opzioni.
2. **Authentication (Autenticazione)**:
    
    - **Descrizione**: Durante questa fase, la STA e il server di autenticazione (AS) provano reciprocamente le loro identità.
    - **Dettagli**: L'AP blocca il traffico non relativo all'autenticazione tra la STA e l'AS finché l'autenticazione non ha successo. L'AP non partecipa attivamente alla transazione di autenticazione, limitandosi a inoltrare il traffico tra la STA e l'AS.
3. **Key Generation and Distribution (Generazione e Distribuzione delle Chiavi)**:
    
    - **Descrizione**: L'AP e la STA eseguono diverse operazioni che portano alla generazione di chiavi crittografiche e alla loro distribuzione tra l'AP e la STA.
    - **Dettagli**: I frame vengono scambiati solo tra l'AP e la STA, garantendo che le chiavi siano generate e memorizzate correttamente su entrambi i dispositivi.
4. **Protected Data Transfer (Trasferimento di Dati Protetti)**:
    
    - **Descrizione**: I frame vengono scambiati tra la STA e la stazione finale attraverso l'AP.
    - **Dettagli**: Come indicato dall'ombreggiatura e dall'icona del modulo di cifratura, il trasferimento sicuro dei dati avviene solo tra la STA e l'AP. La sicurezza non è fornita end-to-end, cioè non copre l'intero percorso fino alla stazione finale.
5. **Connection Termination (Terminazione della Connessione)**:
    
    - **Descrizione**: L'AP e la STA scambiano frame per terminare la connessione.
    - **Dettagli**: Durante questa fase, vengono eseguite le operazioni necessarie per chiudere la connessione in modo sicuro e garantire che le chiavi e le sessioni siano terminate correttamente.