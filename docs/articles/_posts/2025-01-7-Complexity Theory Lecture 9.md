---
layout: post
title: Completezza e Riduzioni tra classi di complessità
truncated_preview: true
excerpt_separator: <!--more-->
tags:
  - miscellaneous
categories: article
---
<!--more-->
In questo articolo introdurrò il concetto di riduzione e completezza. L'idea generale è quella sia possibile calcolare la complessità dei problemi di complessità sconosciuta tramite una trasformazione degli stessi in problemi di complessità nota.
## -- Definizione di riduzione --
Una riduzione da $L_{1}$ a $L_{2}$ è una trasformazione R definita come 
$$
R: \Sigma^*_{1}\to \Sigma^*_{2} \text{ tale che } x \in L_{1} \iff R(x) \in L_{2}
$$
R dev'essere una funzione calcolabile in spazio logaritmico.

## -- Definizione di completezza --
Affermare che un linguaggio $\mathcal{L}$ è C-completo significa che 
$$
\mathcal{L}\in C \land\forall \mathcal{L}' \in C ,\mathcal{L}'\preceq\mathcal{L}
$$
Ossia ogni linguaggio/problema appartenente alla stessa classe di complessità può essere ridotto al problema considerato.


Esempio di 