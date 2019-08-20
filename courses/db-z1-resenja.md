---
layout: page
title: Z1-Rešenja
description: >
  Baze podataka - rešenje zadataka
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

## Primer 1

Prikazati šifru i naziv predmeta.

{% highlight sql linenos %}
PREDMET[SIFRA, NAZIV]
{% endhighlight %}

## Primer 2

Prikazati podatke o predmetima koji imaju po 6 kredita.

{% highlight sql linenos %}
PREDMET where KREDITI=6
{% endhighlight %}

## Primer 3

Izdvojiti ime i prezime studenta sa indeksom 25/2010.

{% highlight sql linenos %}
(DOSIJE where INDEKS=20100025)[IME, PREZIME]
{% endhighlight %}

## Primer 4

Izdvojiti nazive rokova koji su održani posle 2009. godine.

{% highlight sql linenos %}
(ISPITNI_ROK where GODINA_ROKA>2009)[NAZIV]
{% endhighlight %}

## Primer 5

Izdvojiti indekse studenata koji imaju.

{% highlight sql linenos %}
-- ocenu 10 ili 9
(ISPIT where OCENA=10 OR OCENA=9)[INDEKS]

-- ili 
(ISPIT where OCENA=10)[INDEKS]
UNION
(ISPIT where OCENA=9)[INDEKS]

-- ocenu 10 i 9
(ISPIT where OCENA=10)[INDEKS]
INTERSECT
(ISPIT where OCENA=9)[INDEKS]

-- samo ocene 10
(ISPIT where OCENA=10)[INDEKS]
MINUS
(ISPIT where OCENA<10)[INDEKS]
{% endhighlight %}}

## Primer 6

Pronaći studente koji su rođeni u godini kada je održan neki ispitni rok.
Izdvojiti indeks, godinu rođenja i naziv roka.

{% highlight sql linenos %}
(DOSIJE times ISPITNI_ROK where
DOSIJE.GOD_RODJENJA=ISPITNI_ROK.GODINA_ROKA)
[DOSIJE.INDEKS, DOSIJE.GOD_RODJENJA, ISPITNI_ROK.NAZIV]
{% endhighlight %}

## Primer 7

Za svakog studenta izdvojiti predmete koje je polagao. Izdvojiti indeks studenta, naziv
predmeta i ocenu koju je dobio.

{% highlight sql linenos %}
((ISPIT TIMES PREDMET) where
ISPIT.ID_PREDMETA=PREDMET.ID_PREDMETA) [ISPIT.INDEKS,
PREDMET.NAZIV, ISPIT.OCENA]

-- ili 

(ISPIT JOIN PREDMET)[ISPIT.INDEKS, PREDMET.NAZIV, ISPIT.OCENA]
{% endhighlight %}

## Primer 8

Izdvojiti indekse studenata koji su polagali sve predmete.

{% highlight sql linenos %}
ISPIT[INDEKS, ID_PREDMETA]
DIVIDE BY
PREDMET[ID_PREDMETA]
{% endhighlight %}

## Primer 9

Izdvojiti parove predmeta koji imaju isti broj kredita. 
Izdvojiti šifre i nazive predmeta.

{% highlight sql linenos %}
DEFINE ALIAS PREDMET1 FOR PREDMET
((PREDMET TIMES PREDMET1) where
PREDMET.KREDITI=PREDMET1.KREDITI AND
PREDMET.ID_PREDMETA<PREDMET1.ID_PREDMETA)
[PREDMET.SIFRA, PREDMET.NAZIV, PREDMET1.SIFRA, PREDMET1.NAZIV]
{% endhighlight %}

## Primer 10

Izdvojiti indekse studenata koji su položili predmet Geometrija.

{% highlight sql linenos %}
((PREDMET where NAZIV=’ Geometrija’) JOIN (ISPIT where OCENA>5))
[ISPIT.INDEKS]
((PREDMET where NAZIV=’ Geometrija’)[ID_PREDMETA]JOIN (ISPIT where OCENA>5))[ISPIT.INDEKS]
{% endhighlight %}

## Primer 11

Izdvojiti podatke o studentima koji su polagali u januarsokm ispitnom roku 2011 godine.

{% highlight sql linenos %}
DOSIJE SEMIJOIN (ISPIT WHERE GODINA_ROKA=2011 AND OZNAKA_ROKA=’jan)
{% endhighlight %}

## Primer 12

Izdvojiti podatke o predmetima koje je položio bar jedan student rodjen 1992.

{% highlight sql linenos %}
PREDMET SEMIJOIN ((ISPIT WHERE OCENA>5) SEMIJOIN (DOSIJE
WHERE GOD_RODJENJA=1992)
{% endhighlight %}

## Primer 13

Izdvojiti podatke o studentima koji nisu polagali ispite u januarskom ispitnom roku.

{% highlight sql linenos %}
DOSIJE SEMIMINUS (ISPIT WHERE GODINA_ROKA=2011 AND OZNAKA_ROKA=’jan’)
{% endhighlight %}

## Primer 14

Za svaki predmet prikazati njegove podatke i koliko košta njegovo upisivanje za samofinansirajuće studente. 
Jedan kredit košta 1500 din.

{% highlight sql linenos %}
EXTEND PREDMET ADD (KREDITI*1500) AS CENA
{% endhighlight %}

## Primer 15

Za svaku godinu izračunati koliko studenata je rođeno te godine.

{% highlight sql linenos %}
SUMMARIZE DOSIJE PER DOSIJE[GOD_RODJENJA] ADD COUNT AS BRSTUD
{% endhighlight %}

## Primer 16

Za svakog studenta i godinu u kojoj su održani ispitni rokovi prikazati koliko predmeta je položio i prosečnu ocenu.

{% highlight sql linenos %}
SUMMARIZE (ISPIT WHERE OCENA>5) PER ISPIT[INDEKS,GODINA_ROKA] ADD
(COUNT AS BRP, AVG(OCENA) AS PROSEK)
{% endhighlight %}

## Primer 17

Pronaći ispitni rok u kome su neki ispit polagali svi studenti. 
Izdvojiti godinu i oznaku roka i id predmeta.

{% highlight sql linenos %}
ISPIT[GODINA_ROKA, OZNAKA_ROKA, ID_PREDMETA, INDEKS]
DIVIDE BY
DOSIJE[INDEKS]
{% endhighlight %}

## Primer 18

Pronaći studente koji su polozili neki predmet od 6 kredita. 
Izdvojiti indeks, ime, prezime i naziv predmeta.

{% highlight sql linenos %}
(DOSIJE JOIN (ISPIT where OCENA>5) JOIN
(PREDMET where KREDITI=6))
[DOSIJE.INDEKS, DOSIJE.IME, DOSIJE.PREZIME,PREDMET.NAZIV]
{% endhighlight %}
