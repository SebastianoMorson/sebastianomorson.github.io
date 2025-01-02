---
layout: post
title: Lecture 8 Network Security - EMAIL Security
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---
<!--more-->
Vengono usati due tipi di protocolli per trasferire email:
- SMTP e ESMTP: protocolli per muovere i messaggi attraverso Internet da una sorgente a una destinazione
- IMAP e POP: usati per accedere ai messaggi da parte del client finale dal message store

![[1717312139_grim.png]]


## Simple Mail Transfer Protocol (SMTP)
È un protocollo client-server text-based

usato per spedire i messaggi encapsulati dalla sorgente alla destinazione attraverso più MTAs (Message Transfer Agent)

ESMTP è la versione più recente del protocollo SMTP. La E sta per Extended

## Mail Access Protocols
### POP (Post Office Protocol)
Serve per permettere a un client di **scaricare un'email da un server email (MTA)**
Dopo l'autorizzazione, un user agent può usare i comandi POP3 per **recuperare o cancellare mail**.



### IMAP (Internet Mail Access Protocol)
Anche IMAP permette a un client di **accedere a mail su un server email** e usa anche lui TCP.
I due protocolli si differenziano per il fatto che **IMAP è più complicato di POP3** e **offre più funzionalità**.


## RFC 5322
Definisce il formato per i messaggi di testo inviati tramite email
Un esempio di messaggio correttamente formattato seguendo lo standard RFC 5322 è il seguente:
![[1717312824_grim.png]]


Il problema è che RFC 5322 non prevede la trasmissione di eseguibili o oggetti binari, e ha altre varie limitazioni. Per questo è stato ideato MIME (Multipurpose Internet Message Extension).

## MIME
le specifiche di MIME prevedono i seguenti elementi:
- 5 nuovi message header fields:
	- **MIME-Version** 
		- Must have the parameter value 1.0 
		- This field indicates that the message conforms to RFCs 2045 and 2046 
	- **Content-Type** 
		- Describes the data contained in the body with sufficient detail that the receiving user agent can pick an appropriate agent or mechanism to represent the data to the user or otherwise deal with the data in an appropriate manner 
	- **Content-Transfer-Encoding** 
		- Indicates the type of transformation that has been used to represent the body of the message in a way that is acceptable for mail transport 
	- **Content-ID** 
		- Used to identify MIME entities uniquely in multiple contexts 
	- **Content-Description** 
		- A text description of the object with the body; this is useful when the object is not readable
- un numero di formati di contenuto che prevedono la rappresentazione standardizzata che supporta le multimedia electronic mail.
![[1717314130_grim.png]]
- codifiche di trasferimento che permettono la conversione di qualsiasi format content in una forma che è protetta dall'alterazione data dal sistema mail
![[1717314151_grim.png]]

## S/MIME (Secure/Multipurpose Internet Mail Extension)
![[1717314934_grim.png]]

S/MIME garantisce l'autenticità con la digital signature e la confidenzialità con l'encryption tramite un misto di AES e RSA.

![[1717322752_grim.png]]
## PEC  (Posta Elettronica Certificata)
L'idea è aggiungere il non-ripudio e il certificato di invio con timestamp.
Quando il ricevitore riceve l'email viene inviata una conferma di ricezione al mittente, che garantisce che l'email è arrivata correttamente. Se il server è riconosciuto ufficialmente, questa conferma di ricezione ha valore legale.

![[1717322732_grim.png]]








