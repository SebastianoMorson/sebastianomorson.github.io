---
layout: post
title: Lecture 1 Network Security - Introduction
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
CIA: 
- confidenzialità (data confidentiality, privacy)
- integrity (data integrity, system integrity) 
- availability

Accountability: posso tracciare univocamente le azioni di un utente 
Authenticity: posso garantire la genuinità e l'affidabilità di un dato


**Security attack:** Any action that compromises the security of information 
owned by an organization. 
**Security mechanism:** A process (or a device incorporating such a process) that is designed to detect, prevent, or recover from a security attack. 
**Security service:** A processing or communication service that enhances the security of the data processing systems and the information transfers of an organization. The services are intended to counter security attacks, and they make use of one or more security mechanisms to provide the service.


**Threat**: termine che definisce qualsiasi circostanza o evento potenzialmente avverso e che può impattare sulle operazioni di un'organizzazione
**Attack**: qualsiasi tipo di attività malevola nei confronti delle risorse di un sistema informatico o delle informazioni stesse 

Secondo X.800, esistono due tipi di attacchi: passivo e attivo che differiscono per il fatto che il primo non affligge le risorse di un sistema, ma solo le informazioni. Il secondo tipo di attacco invece altera le risorse di un sistema o le operazioni.

Esempi di attacchi passivi sono:
- la diffusione del contenuto dei messaggi
- l'analisi del traffico
Esempi di attacchi attivi sono:
- **replay attack**: passive capture of a data unit and its subsequent retransmission
- **masquerade**:  one entity pretends to be a different entit
- **denial of service**: prevents or inhibits the normal use or management of communications facilitie
- **data modification**: some portion of a legitimate message is altered


# Security Services
## Authentication
Voglio garantire che la mia comunicazione sia autentica, ossia che la sorgente di un messaggio ricevuto sia esattamente quella conosciuta e che una serie di messaggi non subisca interferenze dall'esterno che alterino una delle due parti legittime.

Two specific authentication services are defined in X.800:
- **Peer entity authentication:** Provides for the corroboration of the identity of a peer entity in an association. Two entities are considered peers if they implement the same protocol in different systems
- **Data origin authentication:** Provides for the corroboration of the source of a data unit. It does not provide protection against the duplication or modification of data units


**Access Control**
L'abilità di limitare e controllare l'accesso a un sistema e alle applicazioni attraverso canali di comunicazione. Per ottenere questa abilità le entità devono prima essere identificate o autenticate così che vengano garantiti i diritti di accesso.


## Data confidentiality
Si intende la protezione dei dati trasmessi da un attacco passivo come ad esempio la traffic analysis

## Data Integrity
In questo caso ci interessa garantire che il dato non venga modificato.
Il servizio di data integrity può essere **con o senza recovery**.

Inoltre il servizio di data integrity si concentra più sulla detection più che sulla prevention. Questo perchè se l'integrità viene meno è perchè viene svolto un attacco attivo, che è difficile da prevenire. Se invece si riesce a individuare (detection) allora si può decidere di applicare una politica di semplice notifica all'agente umano, o una policy di remediation per ripristinare il dato modificato.

## Nonrepudation
in certe casistiche è importante garantire il non-repudio, per esempio nel caso di transazione bancarie. In questi casi non dev'essere possibile eseguire un'azione e successivamente negare l'autenticità dell'azione richiesta. 
Il non-repudio è un servizio che per l'appunto fa sì che le parti coinvolte nella comunicazione possano provare che ciò che è stato ricevuto provenga veramente dall'utente interessato.

## Availability 
Il servizio di availability garantisce che un sistema o una risorsa sia accessibile dagli utenti che ne hanno diritto, in qualsiasi momento. Questo servizio è importante soprattutto nei contesti in cui sono probabili attacchi di tipo denial-of-service

# Security Mechanisms
**Cryptographic algorithms:** 
We can distinguish between reversible cryptographic mechanisms and irreversible cryptographic mechanisms. A reversible cryptographic mechanism is simply an encryption algorithm that allows data to be encrypted and subsequently decrypted. Irreversible cryptographic mechanisms include hash algorithms and message authentication codes, which are used in digital 
signature and message authentication applications. 

**Data integrity:** This category covers a variety of mechanisms used to assure the integrity of a data unit or stream of data units. 

**Digital signature:** Data appended to, or a cryptographic transformation of, a data unit that allows a recipient of the data unit to prove the source and integrity of the data unit and protect against forgery. 

**Authentication exchange:** A mechanism intended to ensure the identity of an entity by means of information exchange. 

**Traffic padding:** The insertion of bits into gaps in a data stream to frustrate traffic analysis attempts. 

**Routing control:** Enables selection of particular physically or logically secure routes for certain data and allows routing changes, especially when a breach of security is suspected. 

**Notarization:** The use of a trusted third party to assure certain properties of a data exchange 

**Access control:** A variety of mechanisms that enforce access rights to resource


![[1715174003_grim.png]]
## Keyless
A **hash function** turns a variable amount of text into a small, fixed-length value called a hash value, hash code, or digest. A cryptographic hash function is one that has additional properties that make it useful as part of another cryptographic algorithm, such as a message authentication code or a digital signature. 

A **pseudorandom number generator** produces a deterministic sequence of numbers or bits that has the appearance of being a truly random sequence. Although the sequence appears to lack any definite pattern, it will repeat after a certain sequence length. Nevertheless, for some cryptographic purposes this apparently random sequence is sufficient.
## Single-key algorithms

![[1715174269_grim.png]]

## Asymmetric algorithms
![[1715174518_grim.png]]


![[1715174541_grim.png]]

Le parole chiave nel design sicuro sono:
- economy of mechanism
- fail-safe defaults
- complete mediation
- open design
- separation of privilege
- least privilege
- least common mechanism
- psychological acceptability
- isolation
- encapsulation
- modularity
- layering
- least astonishment (l'utente deve poter intuire il comportamento di un certo meccanismo, sennò c'è maggior rischio di commettere errori)

