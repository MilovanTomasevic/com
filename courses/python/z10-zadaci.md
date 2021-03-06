{% extends "base_slides.md" %}
{% block slides %}


class: split-20 nopadding
background-image: url(../key.jpg)

.column_t2.center[.vmiddle[
.fgtransparent[
![:scale 80%](../zahvalnica.png)
]
]]
.column_t2[.vmiddle.nopadding[
.shadelight[.boxtitle1[
.small[
## Acknowledgements

#### University of Novi Sad | Serbia

- [Doc. dr Ivan Kaštelan](http://www.rt-rk.uns.ac.rs/nastavno-osoblje/doc-dr-ivan-kaštelan)
- [Faculty of Technical Sciences](http://ftn.uns.ac.rs/)
- [Sub-department for Computer Engineering and Computer Communications](http://www.rt-rk.uns.ac.rs)
]]]
]]

.footer.small[
- #### Slides are created according to sources in the literature & Acknowledgements
]
 
---
name: zadaci
name: uvod 
class: center, middle, inverse

# Zadaci

---
layout: true

.section[[Zadaci](#sadrzaj)]

---

## Zadatak 1 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati graf prikazan na slici. 
- Izmeniti klasu `Vertex` tako da podrži definisanje težinksih koeficijenata na ivicama grafa. 
- Pored klase `Vertex` napraviti i klasu Edge koja predstavlja ivicu grafa. 
- Klasa `Edge` treba da sadrži barem `source, destination i weight` atribute.

![:scale 60%](img/z10/z1.png)
]
]

---
## Zadatak 2

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati Dijkstrin algoritam za pronalaženje najkraće putanje grafa. 
- Pseudokodovi potrebnih funkcija su dati na slici 2. Kao ulazni graf koristiti graf sa slike 1. 

![:scale 80%](img/z10/z2a.png)
]
]

---
## Zadatak 2

.message.is-info[
.message-header[
Zadatak
]
.message-body[

- Pseudokod funkcija potrebnih za implmentaciju Dijkstrinog algoritma

![:scale 90%](img/z10/z2b.png)
]
]

---
## Zadatak 3

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napraviti funkciju za nasumično generisanje usmerenog težinskog grafa. 
- Korišćenjem funkcije `PrintPath` (Vežba 7) ispisati sve putanje od čvora u ka svim ostalim čvorovima, i na osnovu toga skicirati dobijeni graf. 
- Čvor u takođe zabrati nasumično.

]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="../python-z10-resenja"> ☛ `Rešenja`</a>

]
]

---

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}