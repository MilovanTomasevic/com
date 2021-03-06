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
- Koristeći primere priloženih klasa koje opisuju čvor binarnog stabla sa proizvoljnim podacima napraviti (ručno konstruisati) binarno stablo čiji podaci sadrže dva atributa:
    - celobrojnu vrednost i njenu odgovarajuću vrednost u znakovnom obliku. 
- Napraviti funkcije za dodavnje levog i desnog elementa proizvoljnom čvoru, kao i funkciju za ispis vrednosti proizvoljnog čvora.
]
]

---
## Zadatak 2 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati `InorderTreeWalk` funkciju. . 
- Pseudokodovi funkcija su priloženi na slici.

![:scale 75%](img/z5/z2.png)
]
]

---

## Zadatak 3

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- mplementirati `SearchTree` i `IterativeTreeSearch` funkcije. 
- Pseudokodovi funkcija su priloženi na slici

![:scale 75%](img/z5/z3.png)
]
]

---

## Zadatak 4

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati `TreeMinimum`, `TreeMaximum` i `TreeSuccessor` funkcije. 
- Pseudokodovi funkcija su priloženi na slici.

![:scale 75%](img/z5/z4.png)
]
]

---

## Zadatak 5

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati `TreeInsert` i `TreeDelete` funkciju. 
- Pseudokodovi funkcija su priloženi na slici.

![:scale 75%](img/z5/z5.png)
]
]

---

## Zadatak 6

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napraviti proizvoljno dugačak niz slučajno generisanih celobrojnih vrednosti i iskoristiti ga kao ulaz za formiranje binarnog stabla. 
- Izmeniti `InorderTreeWalk` funkciju da umesto ispisa elemente dodaje u listu. 
- Slučajno generisani niz elemenata sortirati i proveriti rezultat. 
- Za sortiranje se preporučuje upotreba funkcionalno proverenog algoritma.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- Proveriti funkcionalnost svih osnovnih funkcija.
- Za proveru osnovnih funkcija koristiti mali broj ulaznih podataka kako bi mogli ručno proveriti funkcionalnost i potvrditi ispravnost rada funkcije.
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="../python-z5-resenja"> ☛ `Rešenja`</a>

]
]

---

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}