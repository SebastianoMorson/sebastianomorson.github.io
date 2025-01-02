---
layout: post
title: Lecture 6 Network Security - TLS
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
![[1715758489_grim.png]]

![[1715758635_grim.png]]

# Transport Level Security (TLS)
![[1715758796_grim.png]]

TLS è lo standard che ha surclassato il protocollo SSL (Secure Socket Layer).
TLS si basa su TCP ed è un servizio general-purpose implementato come un insieme di protocolli.
![[1715758928_grim.png]]
TLS non è un singolo protocollo, ma bensì un doppio layer di protocolli come si può vedere in figura.
Alcuni dei protocolli che fanno parte di TLS sono:
- HTTP
- Handshake Protocol
- Change Cipher Spec Protocol (CCSP)
- Alert Protocol
- Heartbeat Protocol


### Architettura
Quando si parla di TLS bisogna ricordare due concetti:
- **TLS connection**: 
	- peer-to-peer
	- transitorie
	- ogni connessione è associata con ***una*** sessione 
- **TLS session**: 
	- associazione tra client e server
	- definisce un insieme di parametri di sicurezza crittografica 
	- usata per evitare l'overhead di stabilire nuovi parametri di sicurezza per ogni connessione

Una sessione inoltre è definita dai seguenti parametri:
- **identificatore di sessione**
	-> sequenza di byte arbitraria scelta dal servere per identificare uno stato di sessione attivo o resumable
- **peer certificate**
	-> certificato X509.v3 che può avere anche valore nullo
- **metodo di compressione** 
	-> definisce l'algoritmo usato per comprimere i dati prima dell'encryption
- **cipher spec**
	-> definisce i metodi crittografici usati nelle connessioni
- **master secret**
	-> 48-byte secret condiviso tra client e server
- **is resumable**
	-> flag che specifica se la sessione può inizializzare nuove connessioni

Lo stato di connessione è invece definito dai seguenti parametri:
 - **Server and client random:** Byte sequences that are chosen by the server and client for each connection. 
 - **Server write MAC secret:** The secret key used in MAC operations on data sent by the server. 
 - **Client write MAC secret:** The secret key used in MAC operations on data sent by the client. 
 - **Server write key:** The secret encryption key for data encrypted by the server and decrypted by the client. 
 - **Client write key:** The symmetric encryption key for data encrypted by the client and decrypted by the server. 
 - **Initialization vectors:** When a block cipher in CBC mode is used, an initialization vector (IV) is maintained for each key. This field is first initialized by the TLS Handshake Protocol. Thereafter, the final ciphertext block from each record is preserved for use as the IV with the following record. 
 - **Sequence numbers:** Each party maintains separate sequence numbers for transmitted and received messages for each connection. When a party sends or receives a “change cipher spec message”, the appropriate sequence number is set to zero. Sequence numbers may not exceed $2^{64}-1$

# TLS Record Protocol
![[1715760194_grim.png]]

I passaggi svolti dal TLS Record Protocol sono:
1. fragmentation: il messaggio viene frammentato in blocchi di dimensione $\leq2^{14}$ byte 
2. compression: opzionale, in TLSv2 l'algoritmo di compressione di default è null
3. MAC: TLS usa HMAC 
4. encryption: viene usata la cifratura simmetrica, l'encryption non può incrementare la lunghezza più di 1024 byte 
![[1715760409_grim.png]]

Il record header è così composto
![[1715761214_grim.png]]

- **Content Type (8 bits):** The higher-layer protocol used to process the enclosed fragment. 
- **Major Version (8 bits):** Indicates major version of TLS in use. For TLSv2, the value is 3. 
- **Minor Version (8 bits):** Indicates minor version in use. For TLSv2, the value is 0. 
- **Compressed Length (16 bits):** The length in bytes of the plaintext fragment (or compressed fragment if compression is used). The maximum value is 214 + 2048.


![[1715761911_grim.png]]
# Change Cipher Spec Protocol
Protocollo che consiste in un singolo byte del valore 1.
L'unico scopo di questo messaggio è di far sì che il pending state venga copiato nel current state, che aggiorna il cifrario da usare sulla connessione.
# Alert Protocol
Usato per comunicare alle peer entity i TLS-related alerts.
Come in altre applicazioni che usano TLS, i messaggi di alert sono compressi e cifrati.

Il messaggio di alert consiste di 2 byte:
- 1° byte: severità del messaggio (warning o fatal). Se è fatal, la comunicazione viene terminata immediatamente
- 2° byte: codice che indica l'alert specifico
# Handshake Protocol
Protocollo che serve per autenticare mutamente server e client e negoziare l'algoritmo di encryption e di MAC e le chiavi di cifratura usate.  L'Handshake Protocol è usato prima di ogni comunicazione.

![[1715764461_grim.png]]



![[1715761523_grim.png]]

### Phase 1 Establish Security Capabilities
viene inviato un messaggio da parte del client contenente:
- **versione**: la versione TLS più recente, conosciuta dal client
- **random**: 32-bit timestamp + 28 random bytes che viene usata in seguito come nonce e il key exchange per impedire replay attacks
- **session id**: variable-length session identifier. Se il session id è uguale a 0 significa che il client vuole creare una nuova connessione sulla sessione o fare un update dei parametri della connessione esistente
- **chipersuite**: una lista di algoritmi crittografici supportati dal client, in ordine decrescente rispetto alla preferenza
- **compression method**: metodi di compressione supportati dal client

### Phase 2 Server Authentication and Key Exchange
Il server invia il suo certificato e un **server_key_exhange message** (che può essere non inviato nel caso in cui viene usato RSA come key exhange o il server ha inviato un certificato con fixed Diffie-Hellman parameters)

Infine il server invia il **server_done message** per indicare la terminazione dell'hello message e dei messaggi associati.
### Phase 3 Client Authentication and Key Exchange
Il client verifica la validità del certificato ricevuto dal server e controlla che server_hello parameters siano accettabili.  Nel caso in cui ci siano degli errori nei controlli viene l'alert **no_certificate**

Successivamente il client invia **client_key_exchange message** che contiene dei parametri dipendenti dal key exchange utilizzato:
- **RSA**: il client invia una pre-master secret di 48-byte e](<RSA: Il cliente genera un segreto pre-master di 48 byte e lo cifra con la chiave pubblica del certificato del server o con una chiave RSA temporanea da un messaggio server_key_exchange. Il suo utilizzo per calcolare un segreto master è spiegato successivamente.
- **Diffie–Hellman Ephemeral o Anonimo**: I parametri pubblici Diffie–Hellman del cliente vengono inviati.
- **Diffie–Hellman Fisso**: I parametri pubblici Diffie–Hellman del cliente sono stati inviati in un messaggio di certificato, quindi il contenuto di questo messaggio è nullo.
  Infine, in questa fase, il cliente può inviare un messaggio certificate_verify per fornire una verifica esplicita di un certificato del cliente. Questo messaggio viene inviato solo a seguito di eventuali certificati del cliente che hanno capacità di firma (cioè tutti i certificati tranne quelli contenenti parametri Diffie–Hellman fissi).
### Phase 4 Finish
Il cliente invia un messaggio change_cipher_spec e copia il CipherSpec pendente nel CipherSpec corrente. Si noti che questo messaggio non è considerato parte del Protocollo di Handshake ma viene inviato utilizzando il Protocollo Change Cipher Spec.
Il cliente invia quindi immediatamente il messaggio finished con i nuovi algoritmi, chiavi e segreti. Il messaggio finished verifica che gli scambi di chiavi e i processi di autenticazione siano stati completati con successo


# Generation of Cryptographic Parameters

# SSL/TLS Attacks


# Hyper Text Transfer Protocol Secure (HTTPS)

# Secure Shell (SSH)


