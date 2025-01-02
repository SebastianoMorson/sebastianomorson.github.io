---
layout: post
title: Lecture 3 Network Security - Digital Signatures
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - cybersec
categories: article
---

<!--more-->
![[1715333879_grim.png]]

# Digital Signature Properties and requirements
Le proprietà che deve garantire una firma digitale sono:
- deve verificare l'autorità, la data e il tempo in cui è stata effettuata la firma
- deve autenticare il contenuto del messaggio nella data e nel tempo in cui è stata effettuata la firma
- dev'essere verificabile da una terza parte per evitare dispute

Inoltre una signature digitale deve essere un insieme di bit che dipendono dal messaggio che dev'essere firmato. Se così non fosse si potrebbe forgiare facilmente una firma che va bene per più di un messaggio.

Inoltre bisogna che la firma sia facile da produrre, riconoscere, verificare e sia costruita in modo che chi l'ha creata non possa ripudiarla. È importante inoltre che non sia possibile forgiare una firma falsa in tempo utile.

# Attacks
![[1715334307_grim.png]]

# Forgeries
![[1715334363_grim.png]]

# ElGamal Digital Signature
Questa firma usa la chiave privata per firmare e la chiave pubblica per verificare la firma.
Inoltre usa un numero primo $q$ e un valore $a$ che è la radice primitiva di $q$.

The ElGamal Digital Signature Scheme involves the following steps for generating and verifying digital signatures:

1. Key Generation: A user generates a public key and a private key. The public key consists of the values p, g, and y, where p is a large prime number, g is a generator of the multiplicative group modulo p, and y = g^x mod p, where x is the private key.

2. Signing: To sign a message M, the signer performs the following steps:

a. Generate a random number k such that 1 < k < p-1.  
b. Calculate r = g^k mod p.  
c. Calculate h = hash(M), where hash is a fixed-size hash function. d. Calculate s = (h — xr) * k^-1 mod (p-1).  
e. The signature of the message M is the pair (r, s).

3. Verifying: To verify the signature (r, s) of a message M, the verifier performs the following steps:

a. Verify that 1 < r < p-1 and 0 < s < p-1. If either condition is not satisfied, the signature is invalid.

b. Calculate h = hash(M).  
c. Calculate v1 = (y^r * r^s) mod p.  
d. Calculate v2 = g^h mod p.  
e. If v1 = v2, the signature is valid. Otherwise, the signature is invalid.
# Schnorr Digital Signature

# NIST Digital Signature Algorithm

![[1715335024_grim.png]]

![[1715336606_grim.png]]

![[1715349305_grim.png]]
# Elliptic Curve Digital Signature Algorithm (ECDSA)
![[1715335083_grim.png]]

In essence, four elements are involved. 
1. **All those participating in the digital signature scheme use the same** global domain parameters, which define an elliptic **curve and a point of origin on the curve**. 
2. A **signer** must first **generate a public, private key** pair. For the private key, the signer **selects a random or pseudorandom number**. Using that random number and the point of origin, the signer **computes another point on the elliptic curve**. This is the signer’s public key. 
3. **A hash value is generated for the message to be signed**. Using the private key, the domain parameters, and the hash value, a signature is generated. **The signature consists of two integers, r and s**. 
4. To verify the signature, the verifier uses as input the signer’s public key, the domain parameters, and the integer s. The **output is a value v that is compared to r. The signature is verified if the v = r**.

