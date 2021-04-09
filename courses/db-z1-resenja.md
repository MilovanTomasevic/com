---
layout: page
title: Z1-Rešenja
description: >
  Baze podataka - rešenje zadataka
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Primer 1

Prikazati šifru i naziv predmeta.

~~~sql
-- file: 'primer1-1.sql'
PREDMET[SIFRA, NAZIV]
~~~

## Primer 2

Prikazati podatke o predmetima koji imaju po 6 kredita.

~~~sql
-- file: 'primer1-2.sql'
PREDMET where KREDITI=6
~~~

## Primer 3

Izdvojiti ime i prezime studenta sa indeksom 25/2010.

~~~sql
-- file: 'primer1-3.sql'
(DOSIJE where INDEKS=20100025)[IME, PREZIME]
~~~

## Primer 4

Izdvojiti nazive rokova koji su održani posle 2009. godine.

~~~sql
-- file: 'primer1-4.sql'
(ISPITNI_ROK where GODINA_ROKA>2009)[NAZIV]
~~~

## Primer 5

Izdvojiti indekse studenata koji imaju.

~~~sql
-- file: 'primer1-5.sql'
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
~~~}

## Primer 6

Pronaći studente koji su rođeni u godini kada je održan neki ispitni rok.
Izdvojiti indeks, godinu rođenja i naziv roka.

~~~sql
-- file: 'primer1-6.sql'
(DOSIJE times ISPITNI_ROK where
DOSIJE.GOD_RODJENJA=ISPITNI_ROK.GODINA_ROKA)
[DOSIJE.INDEKS, DOSIJE.GOD_RODJENJA, ISPITNI_ROK.NAZIV]
~~~

## Primer 7

Za svakog studenta izdvojiti predmete koje je polagao. Izdvojiti indeks studenta, naziv
predmeta i ocenu koju je dobio.

~~~sql
-- file: 'primer1-7.sql'
((ISPIT TIMES PREDMET) where
ISPIT.ID_PREDMETA=PREDMET.ID_PREDMETA) [ISPIT.INDEKS,
PREDMET.NAZIV, ISPIT.OCENA]

-- ili 

(ISPIT JOIN PREDMET)[ISPIT.INDEKS, PREDMET.NAZIV, ISPIT.OCENA]
~~~

## Primer 8

Izdvojiti indekse studenata koji su polagali sve predmete.

~~~sql
-- file: 'primer1-8.sql'
ISPIT[INDEKS, ID_PREDMETA]
DIVIDE BY
PREDMET[ID_PREDMETA]
~~~

## Primer 9

Izdvojiti parove predmeta koji imaju isti broj kredita. 
Izdvojiti šifre i nazive predmeta.

~~~sql
-- file: 'primer1-dodatak.sql'
DEFINE ALIAS PREDMET1 FOR PREDMET
((PREDMET TIMES PREDMET1) where
PREDMET.KREDITI=PREDMET1.KREDITI AND
PREDMET.ID_PREDMETA<PREDMET1.ID_PREDMETA)
[PREDMET.SIFRA, PREDMET.NAZIV, PREDMET1.SIFRA, PREDMET1.NAZIV]
~~~

## Primer 10

Izdvojiti indekse studenata koji su položili predmet Geometrija.

~~~sql
-- file: 'primer1-10.sql'
((PREDMET where NAZIV=’ Geometrija’) JOIN (ISPIT where OCENA>5))
[ISPIT.INDEKS]
((PREDMET where NAZIV=’ Geometrija’)[ID_PREDMETA]JOIN (ISPIT where OCENA>5))[ISPIT.INDEKS]
~~~

## Primer 11

Izdvojiti podatke o studentima koji su polagali u januarsokm ispitnom roku 2011 godine.

~~~sql
-- file: 'primer1-11.sql'
DOSIJE SEMIJOIN (ISPIT WHERE GODINA_ROKA=2011 AND OZNAKA_ROKA=’jan)
~~~

## Primer 12

Izdvojiti podatke o predmetima koje je položio bar jedan student rodjen 1992.

~~~sql
-- file: 'primer1-12.sql'
PREDMET SEMIJOIN ((ISPIT WHERE OCENA>5) SEMIJOIN (DOSIJE
WHERE GOD_RODJENJA=1992)
~~~

## Primer 13

Izdvojiti podatke o studentima koji nisu polagali ispite u januarskom ispitnom roku.

~~~sql
-- file: 'primer1-13.sql'
DOSIJE SEMIMINUS (ISPIT WHERE GODINA_ROKA=2011 AND OZNAKA_ROKA=’jan’)
~~~

## Primer 14

Za svaki predmet prikazati njegove podatke i koliko košta njegovo upisivanje za samofinansirajuće studente. 
Jedan kredit košta 1500 din.

~~~sql
-- file: 'primer1-14.sql'
EXTEND PREDMET ADD (KREDITI*1500) AS CENA
~~~

## Primer 15

Za svaku godinu izračunati koliko studenata je rođeno te godine.

~~~sql
-- file: 'primer1-15.sql'
SUMMARIZE DOSIJE PER DOSIJE[GOD_RODJENJA] ADD COUNT AS BRSTUD
~~~

## Primer 16

Za svakog studenta i godinu u kojoj su održani ispitni rokovi prikazati koliko predmeta je položio i prosečnu ocenu.

~~~sql
-- file: 'primer1-16.sql'
SUMMARIZE (ISPIT WHERE OCENA>5) PER ISPIT[INDEKS,GODINA_ROKA] ADD
(COUNT AS BRP, AVG(OCENA) AS PROSEK)
~~~

## Primer 17

Pronaći ispitni rok u kome su neki ispit polagali svi studenti. 
Izdvojiti godinu i oznaku roka i id predmeta.

~~~sql
-- file: 'dodatak.sql'
ISPIT[GODINA_ROKA, OZNAKA_ROKA, ID_PREDMETA, INDEKS]
DIVIDE BY
DOSIJE[INDEKS]
~~~

## Primer 18

Pronaći studente koji su polozili neki predmet od 6 kredita. 
Izdvojiti indeks, ime, prezime i naziv predmeta.

~~~sql
-- file: 'dodatak.sql'
(DOSIJE JOIN (ISPIT where OCENA>5) JOIN
(PREDMET where KREDITI=6))
[DOSIJE.INDEKS, DOSIJE.IME, DOSIJE.PREZIME,PREDMET.NAZIV]
~~~
