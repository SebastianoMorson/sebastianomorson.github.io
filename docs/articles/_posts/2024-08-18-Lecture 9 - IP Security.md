---
layout: post
title: Lecture 9 Network Security - IP Security
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
![[1717511358_grim.png]]

IPSec è obbligatorio in IPv6 e opzionale in IPv4, inoltre può operare in modalità **transport** o **tunnel**

## IPSec in Transport Mode
Fornisce la protezione per i payload dei protocolli di trasporto, inserendo i security headers dopo l'header IP originale e prima del payload IP originale.

L'**IP header originale NON è protetto** (questo significa che tutte le informazioni associate, come ad esempio ip di destinazione e mittente non sono nascosti)
![[1717511661_grim.png]]
Generalmente questa modalità è usata per comunicazioni end-to-end tra due hosts.
## IPSec in Tunnel Mode
In questa modalità, l’intero pacchetto IP originale viene incapsulato per diventare il payload di un nuovo pacchetto IP.
- Viene aggiunto un nuovo header IP sopra al pacchetto IP originale.
- Questa modalità è utile per proteggere il traffico tra reti diverse.
- Facilita l’instaurazione di un “tunnel” tra due gateway IPsec sicuri, che a loro volta possono collegare due reti diverse.
- Nella modalità tunnel, il pacchetto originale può essere firmato per l’integrità e l’autenticazione tramite l’header AH (Authentication Header) o l’header ESP (Encapsulating Security Payload).
- [Inoltre, il pacchetto può essere cifrato per una maggiore sicurezza](https://www.twingate.com/blog/ipsec-tunnel-mode)[1](https://www.twingate.com/blog/ipsec-tunnel-mode).
![[1717511684_grim.png]]

### Transport mode vs Tunnel Mode
![[1717511722_grim.png]]


## AH Protocol vs ESP Protocol
AH = Authentication Header Protocol
ESP = Encapsulating Security Payload Protocol

Praticamente AH era già usato quando ESP è stato ideato. ESP aggiunge funzionalità al protocollo AH.
![[1717511860_grim.png]]


## AH Protocol
Fornisce Integrity e Authentication ai pacchetti IP, prevenendo l'attacco di address-spoofing.
Il prezzo da pagare è che che le parti devono condividere un segreto. 

Fa uso inoltre di HMAC-MD5-96 o HMAC-SHA-1-96.
![[1717512023_grim.png]]

![[1717512078_grim.png]]
## ESP Protocol
Fornisce la **confidenzialità sul contenuto dei messaggi** e sulle comunicazioni a traffico limitato.

Può inoltre offrire gli stessi servizi di autenticazione che forniva il protocollo AH.

La differenza principale tra AH e ESP è che **ESP usa anche la cifratura per mettere in sicurezza il contenuto dei messaggi.**
![[1717512382_grim.png]]

La cosa bella di ESP è che **nella modalità tunnel, viene cifrato l'intero pacchetto IP**, e a ogni hop viene aggiunto un nuovo header. Questo va molto bene per le VPN e per ottenere la **gateway-to-gateway security**.

![[1717512543_grim.png]]


## Anti-replay protection
Per proteggere la comunicazione da attacchi di tipo replay viene usato un sequence number e una window size. 
Quando un nuovo pacchetto arriva, ci sono tre casi:

1. Se il suo numero di sequenza è **inferiore a N**, il **pacchetto viene scartato** (anche se autentico). Potrebbe essere un duplicato o troppo vecchio.
2. Se **N ≤ il suo numero di sequenza < N+W:**
    - Se il pacchetto non è contrassegnato come “arrivato” (in verde) e supera il test di autenticazione, viene contrassegnato come “arrivato” e accettato.
    - Altrimenti, viene scartato come duplicato.
3. Se il **numero di sequenza è maggiore o uguale a N+W**:
    - Se il pacchetto supera il test di autenticazione, la finestra di replay viene spostata al nuovo numero di sequenza (SN).
    - [Il pacchetto viene quindi contrassegnato come “arrivato” e accettato](https://community.fortinet.com/t5/FortiGate/Technical-Note-Explaining-IPSEC-Anti-replay-and-preventing/ta-p/192424)[1](https://community.fortinet.com/t5/FortiGate/Technical-Note-Explaining-IPSEC-Anti-replay-and-preventing/ta-p/192424).
![[1717512698_grim.png]]

## Security Associations
![[1717516502_grim.png]]

#### Security Association Database (SAD)
è il database che contiene tutte le security associations e che deve essere presente in ogni implementazione di IPSec
![[1717516559_grim.png]]

## Security Policies
Serve per definire la sicurezza applicata al pacchetto quando deve essere inviato o quando arriva, è una sorta di filtro come nei firewall network-based.

Prima di poter usare SAD è necessario che un host determini la policy del pacchetto predefinito (ad esempio scarta tutti i pacchetti o altro)

## Security Policy Database
I Security Policy Databases (SPD) nel contesto di IPSec **servono a definire come il traffico IP è correlato a specifiche Security Associations (SA).** Ogni voce nel SPD definisce un sottoinsieme di traffico IP e punta a una SA per quel traffico. 

In ambienti più complessi, **potrebbero esserci più voci che si riferiscono a una singola SA o più SA associate a una singola voce SPD**.
I **selettori**, costituiti da valori di campo IP e protocolli di livello superiore, vengono utilizzati **per filtrare il traffico in uscita e mapparlo in una particolare SA**. 

In breve, gli SPD consentono di gestire la politica di sicurezza per il traffico IP all’interno di una rete

## Security Policy Database Entries   
![[1717517870_grim.png]]
## SP&SA outbound & inbound processing
![[Pasted image 20240604182901.png]]
## VPN
![[1717513021_grim.png]]

![[1717514865_grim.png]]


## Internet Key Exchange
Tipicamente, sono necessarie quattro chiavi per la comunicazione tra due applicazioni: coppie di trasmissione e ricezione per l’integrità e la riservatezza. Il documento sull’architettura IPsec richiede il supporto di due tipi di gestione delle chiavi:

- **Manuale**: Un amministratore di sistema configura manualmente ciascun sistema con le proprie chiavi e quelle degli altri sistemi comunicanti. Questo approccio è pratico per ambienti piccoli e relativamente statici.
- [**Automatizzato**: Consente la creazione on-demand di chiavi per le Security Associations (SA) e facilita l’uso di chiavi in un grande sistema distribuito con una configurazione in evoluzione](https://paraphraz.it/it/)
Il key management automatico usato di default da IPSec è ISAKMP/Oakley, dove:
#### Oakley Key Determination Protocol 
è il protocollo di key exchange basato su Diffie-Hellman ed abbastanza generico in quanto non definisce nessun formato specifico

#### ISAKMP
è l'acronico di **Internet Security Association and Key Management Protocol** e specifica un framework per la gestione delle Internet Key.

Consiste in un insieme di tipi di messaggio che abilitano l'uso di una varietà di algoritmi di scambio delle chiavi.


La determinazione delle IKE Key è caratterizzato dalle seguenti features:
1. **Utilizzo di cookies**: Per prevenire attacchi di congestione.
2. **Negoziazione del gruppo**: Consente alle due parti di stabilire i parametri globali per lo scambio di chiavi Diffie-Hellman.
3. **Utilizzo di nonce**: Per proteggersi dagli attacchi di replay.
4. **Scambio delle chiavi pubbliche Diffie-Hellman**: Per stabilire la chiave segreta.
5. [**Autenticazione dello scambio Diffie-Hellman**: Per prevenire attacchi man-in-the-middle](https://www.geeksforgeeks.org/internet-key-exchange-ike-in-network-security/)[1](https://www.geeksforgeeks.org/internet-key-exchange-ike-in-network-security/).

![[1717515818_grim.png]]

![[1717515830_grim.png]]

![[1717515843_grim.png]]