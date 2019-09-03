{% extends "base_slides.md" %}
{% block slides %}

name: sadrzaj

# Sadržaj

- [Uvod](#uvod)
- [Naučna zamisao istraživanja](#nzi)
- [Predmet i problem istraživanja](#problem)
- [Istraživačka pitanja](#pitanja)
- [Aktuelno stanje u oblasti](#stanje)
- [Potrebe istraživanja](#potrebe)
- [Istraživačke hipoteze](#hipoteze)
- [Ciljevi istraživanja](#ciljevi)
- [Metodologija istraživanja](#metod)
- [Očekivani rezultati i njihova primenljivost](#rezultati)
- [Predlog strukture rada](#predlog)
- [Literatura](#literatura)
      
---
name: uvod
class: center, middle, inverse

# UVOD

---
layout: true

.section[[Uvod](#sadrzaj)]

---

## Uvod

- Superračunar predstavlja **računarsku arhitekturu velikog kapaciteta**, tj. visokih performansi, sposobnu da **obrađuje veliku količinu podataka u veoma kratkom vremenu**.
- Superračunari se koriste za rešavanje najrazličitijih problema koji uključuju i intenzivna izračunavanja, kao npr. 
  - u upravljanju zalihama, 
  - u vojnim obaveštajnim službama, 
  - u predviđanju klime i modeliranju zemljotresa, 
  - u transportu, u proizvodnji, 
  - u ljudskom zdravlju i bezbednosti, 
  - tj. u praktično svakom području nauke ili poslovanja.
---

name: nzi
class: center, middle, inverse
layout: false

# Naučna zamisao istraživanja

---
layout: true

.section[[Naučna zamisao istraživanja](#sadrzaj)]

---

## Ideja

- Imajući u vidu povećane potrebe za izvođenje složenih analiza podataka u nauci i primenu HPC tehnologija u podršci takvih analiza, postoji objektivna potreba za **adekvatnim načinom vrednovanja i izborom odgovarajuće HPC arhitekture**, što predstavlja izazov za inženjersku zajednicu u oblasti računarstva i informatike. 
- Da bi se  odgovorilo tom izazovu, ova **disertacija ima za motiv proučavanje i analiziranje** različitih modela, metoda i alata za kvalitetnu procenu izbora HPC arhitekture u cilju pronalaženja novog metodološkog pristupa koji će obezbediti odgovarajuće vrednovanje, odlučivanje i primenu HPC u različitim okruženjima.
- Ideja ovog istraživanja je da omogućimo upotrebu **sistema za podršku odlučivanju** koji će biti od koristi menadžerima u organizacijama koji odlučuju o **nabavci i uvođenju HPC arhitektura**. Predloženo rešenje biće izloženo kao javno, pa kao takvo, može koristiti i široj zajednici koja ima isti problem odlučivanja.

---

name: problem
layout: false
class: center, middle, inverse

# PREDMET I PROBLEM ISTRAŽIVANJA

---
layout: true

.section[[Predmet i problem istraživanja](#sadrzaj)]

---

## Predmet istraživanja

- Superračunarski sistemi troše značajnu količinu energije, što dovodi do visokih operativnih troškova, smanjenja pouzdanosti i rasipanja prirodnih resursa. 
- Ova činjenica jasno ukazuje na osetljivost procesa odlučivanja o izboru i primeni jedne takve arhitekture. 
- Iz tog razloga proizilazi da **predmet ove doktorske disertacije** predstavlja istraživanje modela, metoda i alata za kvalitetnu procenu izbora HPC arhitekture, na osnovu identifikovanih grupa parametara i kriterijuma koje treba uzeti u obzir.

---
## Problem istraživanja
- Pri proceni HPC arhitektura neophodno je analizirati probleme koji postavljaju različite i raznorodne zahteve, često sa različitim relativnim značajem, različito osetljive na promene ulaznih i izlaznih veličina. 
- Zbog toga takvo odlučivanje zahteva primenu metoda višekriterijumskog odlučivanja. 
- Veći broj različitih i raznorodnih kriterijuma pruža sveobuhvatniju i objektivniju sliku u skladu sa zahtevima koje nosilac odlučivanja postavlja. 
- Kriterijumi se mogu pojavljivati u različitim jedinicama, često sa različitim relativnim značajem i različitim zahtevima za maksimizacijom ili minimizacijom.
---
## Problem istraživanja
- Složenost navedenog problema odlučivanja, kao i pojava rizika koji imaju izuzetno jak uticaj na donošenje odluke, izlažući donosioce odluka i organizacije mogućim neželjenim ishodima, nameće potrebu za odgovarajućom podrškom koja bi minimizirala negativne uticaje pri donošenju odluke, odnosno maksimizirala uspešnost odlučivanja pri proceni jedne HPC arhitekture. 
- Saglasno tome, **problem** složenosti odlučivanja u izboru odgovarajuće HPC arhitekture u cilju postizanja željenog ishoda je **problem** koji predložena doktorska disertacija treba da razmatra. 
- U ovom radu, biće predložen novi, tzv. **Fazi adaptivni model za procenu izvodljivosti HPC arhitektura**, eng. ___The Fuzzy Adaptive Model for Feasibility Assessment of High Performance Computing Architectures___, ili skraćeno FAM4FAHPCA.
---


name: pitanja
layout: false
class: center, middle, inverse

# ISTRAŽIVAČKA PITANJA

---
layout: true

.section[[Istraživačka pitanja](#sadrzaj)]

---
## Istraživačka pitanja

1.	Koji su sve parametri, njihove moguće vrednosti i značajnost, a koji mogu da utiču na **isplativost investicije uspostave i razvoja** jedne HPC arhitekture?
1.	Na koji način **obezbediti saglasnost** između postavljenih kompanijskih **ciljeva**, merenih identifikatora i izbora željene **arhitekture**?
1.	Koji su modeli i metode neophodni u cilju **merenja i ocenjivanja** parametara koji utiču na proces odlučivanja o izboru HPC arhitekture?

---
name: stanje
layout: false
class: center, middle, inverse

# AKTUELNO STANJE U OBLASTI

---
layout: true

.section[[Aktuelno stanje u oblasti](#sadrzaj)]

---

## Analiza

.medium[
- Logical scoring of preferences (LSP)
- Six-Step Service Improvement method used Logical scoring of preferences (SSSI)
- Fuzzy Aggregation Method for Quality Service/software (FAM4QS)
- Electre I,II, III, IV
- PROMETHEE I, PROMETHEE II, PROMETHEE III, PROMETHEE IV, PROMETHEE V, PROMETHEE VI, Fuzzy PROMETHEE
- AHP
- Goal Programming
- Surrogate Worth Trade-off metoda – SWT
- STEp Method – STEM
- SEquential Multi- Objective Problem Solving – SEMOPS
- Sequential Information Generator for Multi-Objective Problems – SIGMOP
- Goal Programming STEM – GPSTEM
- ...
]

---

name: potrebe
layout: false
class: center, middle, inverse

# POTREBE ISTRAŽIVANJA

---
layout: true

.section[[Potrebe istraživanja](#sadrzaj)]

---

## Potrebe

- **Pogrešne odluke su skupe**, a za skupe sisteme i najmanja greška može imati veliki uticaj na dalji razvoj, na buduće odluke, a pre svega na očekivani rezultat.
- Zato je neophodno da se **minimizuju greške** pri odlukama što je više moguće. 
- Iz te motivacije dolazi i potreba za razvojem modela **FAM4FAHPCA**, sa ciljem dostizanja što višeg nivoa adaptivnosti, fleksibilnosti i integrisanosti.
- Takođe, **potreba za istraživanjima i unapređenjem** ovakvih modela, metoda, sistema, procedura  koje utiču na donošenje odluka, proizilazi iz trenutnog stanja sa kojim su kompanije suočene, kako bi nastavile sa tendencijom rasta korisnika kojima je potrebna IT usluga.

---

name: hipoteze
layout: false
class: center, middle, inverse

# ISTRAŽIVAČKE HIPOTEZE

---
layout: true

.section[[Istraživačke hipoteze](#sadrzaj)]

---

## Istraživačke hipoteze

- U odnosu na motive ovog istraživanja postavljene su sledeće hipoteze:
.medium[

- **H1**: Moguće je **napraviti FAM4FAHPCA model** čiji će rezultat podržati odlučivanje pri odabiru HPC arhitekture.
- **H2**: Moguće je **napraviti metodu** kojom se definišu mogući pristupi i načini realizacije aktivnosti prikupljanja podataka, analiza podataka i vrednovanja odluka od strane eksperata.
- **H3**: Moguće je **napraviti aplikativno rešenje** na osnovu FAM4FAHPCA modela koje omogućava efikasnije i sigurnije rešenje, koje će minimizirati rizik nepravilnih odluka i greški u procesu odlučivanja.
- **H4**: **Povećanjem baze znanja** sa podacima o aktuelnim HPC arhitekturama, **povećava se i preciznost** FAM4FAHPCA modela.
- **H4.1**: FAM4FAHPCA model će dati **preciznije rezultate** kod korisnika koji u svojim ogranizacijama već poseduju HPC.
- **H4.2**: Organizacije koje **nemaju** formalizovan način izbora arhiktektura, imaju **manju uspešnost** usvajanja u odnosu na organizacije koje imaju u određenoj meri formalizovan navedeni postupak.
]

---

name: ciljevi
layout: false
class: center, middle, inverse

# CILJEVI ISTRAŽIVANJA

---
layout: true

.section[[Ciljevi istraživanja](#sadrzaj)]

---

## Ciljevi

.medium[
- FAM4FAHPCA model, koji bi proizašao iz ovog istraživanja, uz datu specifikaciju, podložan je promenama i nadogradnji, što znači da će se vremenom poboljšavati i usavršavati u cilju **povećanja preciznosti predloženih odluka, boljem odgovoru na nove potrebe i povećanju agilnosti u odlučivanju**.
- Naučni cilj istraživanja je **proširenje saznanja** o mogućnostima povećanja adaptibilnosti i fleksibilnosti modela procene HPC arhitektura koje doprinose ostvarenju postavljenih ciljeva.
- Društveni cilj istraživanja je **obezbeđivanje naučnih saznanja** na osnovu kojih se može povećati fleksibilnost i adaptibilnost sistema za procenu HPC arhitektura, čime bi se omogućilo poslovnim sistemima da kontinuirano prate, mere i upravljaju poslovnim procesima, i tako ostvaruju što bolje poslovne rezultate.
- Osnovni cilj istraživanja jeste **ostvarivanje FAM4FAHPCA modela** i svih kriterijuma koji utiču na njegov rezultat, a zatim i izvođenje zaključaka na koji način ti faktori utiču na konačan razultat modela tj izveštaj na osnovu koga bi se donosila odluka pri odabiru HPC arhitektura.
- Prvi prateći cilj predstavlja **ostvarenje autentičnog integrisanog, fleksibilnog i adaptivnog modela** koji bi odgovoro na  što veći broj korisničkih zahteva pri odluci za HPC arhitekturu kao i da bude precizniji u što većoj meri.
- ...
]

---
name: metod
layout: false
class: center, middle, inverse

# METODOLOGIJA ISTRAŽIVANJA

---
layout: true

.section[[Metodologija istraživanja](#sadrzaj)]

---

## Metod

.medium[
- Da bi ideja istraživanja bila uspešno realizovana, a postavljene hipoteze bile potvrđene ili opovrgnute, postupak metodologije istraživanja će se izvršiti po ključnim fazama.
.medium[
- **U prvoj fazi** biće izvršen detaljan **teorijski pregled** na poljima teorija i sistema odlučivanja kao i HPC, odnosno HPCA.
  - Ishod  prve faze bi bio uvid u postojećim teorijama, metodama, modelima, pristupima, konceptima i alatima iz nadevenih oblasti kao i načini upotrebe svakog predloženog rešenja. 
  - Stečena iskustva i informacije biće od krucijalne važnosti u razvoju FAM4FAHPCA modela. 

- **Druga faza** biće posvećena **razvoju** FAM4FAHPCA modela. U tom cilju, biće obavljeno prikupljanje podataka primenom tehnika anketa, upitnika i intervjua. 
  - Dobijeni podaci će biti osnova za definisanje i projektovanje FAM4FAHPCA modela.

- **U trećoj fazi** biće sprovedeno **ocenjivanje** FAM4FAHPCA modela, odnosno njegovih rezultata.
  - Nakon završenog ocenjivanja rezultati će biti analizirani. 
]
- Analiza dobijenih rezultata treba da ukaže na **dalja poboljšanja** sistema, odnosno da se kroz nekoliko **iteracija** ponavljanja prve ili druge faze dođe do **željenog cilja** FAM4FAHPCA modela. 
- Tako da se na osnovu prihvatljivih **rezultata** FAM4FAHPCA modela mogu postaviti **terijske osnove i specifikacije** za dalja poboljšanja. 
]

---
name: rezultati
layout: false
class: center, middle, inverse

# OČEKIVANI REZULTATI I NJHOVA PRIMENLJIVOST

---
layout: true

.section[[Očekivani rezultati i njihova primenljivost](#sadrzaj)]

---

## Rezultati

- Rezultati dobijeni istraživanjem u radu treba da **pomognu u odlučivanju** pri odabiru HPC arhitektura. 
- Šire, mogu biti od koristi i **svima** drugima koji nameravaju da se bave različitim metodama i modelima za podršku odlučivanju na različitim nivoima upravljanja.
- Istraživanje naročito treba da bude korisno svima koji se bave **HPC sistemima**: istraživačima, ekspertima, naučnim organizacijama i drugima, koji bi učestvovali pružanjem potrebnih znanja i iskustava i u ovom istraživanju.
- Rezultati ovog istraživanja treba da omoguće i **metodološki pristup** koji bi bio osnova za **konsultantski rad** u procesu odlučivanja u oblasti implementacije HPCA.

---
## Doprinos

- Doprinos disertacije bi se ogledao u nekoliko ključnih rezultata: 
    - kreirani **model** za odnošenje odluka pri odabiru HPCA primenom relevantnih kriterijumima i definisanog sistema njihovog ocenjivanja; 
    - pregled **pristupa** metodologiji za procenu HPCA;
    - sveobuhvatni **pregled i sistematizacija** kriterijuma za za donošenje odluka;
    - definisan **skup kriterijuma** za donošenje odluka i kreirano **aplikativno rešenje** za ocenjivanje na osnovu ulaznih parametara od strane izvršioca modela.

---

## Naučna i društvena opravdanost istraživanja

- Naučna opravdanost istraživanja se ogleda u **unapređenju razvoja modela** FAM4FAHPCA sa mogućnostima povećanja **adaptivnosti i fleksibilnosti**. 
- Opisom predloženog modela obogaćuje se saznajni fond nauke.
- Društvena opravdanost istraživanja proizilazi iz **naučnog saznanja** o FAM4FAHPCA za donošenje odluka pri odabiru HPCA, koji omogućava adaptivnost i fleksibilnost, i mogućnosti **primene dobijenih rezultata u praksi**.


---
name: predlog
layout: false
class: center, middle, inverse

# PREDLOG STRUKTURE RADA

---
layout: true

.section[[Predlog strukture rada](#sadrzaj)]

---
## Sadržaj

.small[

.lcol[
- SPISAK SLIKA
- SPISAK DIJAGRAMA
- SPISAK TABELA
- SPISAK DOKUMENATA
- PROGRAMSKI KOD

- **1.UVOD**
  - 1.1 Predmet i problem istraživanja
  - 1.2 Istraživačka pitanja
  - 1.3 Aktuelno stanje u oblasti
  - 1.4 Potrebe istraživanja
  - 1.5 Polazišta i hipoteze istraživanja
  - 1.6 Ciljevi istraživanja
  - 1.7 Metodologija istraživanja
  - 1.8 Očekivani rezultati i njihova primenljivost
    - 1.8.1 Naučna i društvena opravdanost istraživanja
  - 1.9 Kratak pregled rada
- **2.ODLUČIVANJE I VIŠEKRITERIJUMSKA OPTIMIZACIJA**
  - 2.1 Osnovni pojmovi o odlučivanju
  - 2.2. Definicije odlučivanja
  - 2.3. Problemi u odlučivanju

]

.rcol[

- **2.**
  - 2.4 Proces odlučivanja
  - 2.5 Pojam optimizacije
  - 2.6 Definicije odlučivanja
  - 2.7 Problemi u odlučivanju
  - 2.8 Osnove višekriterijumske optimizacije
  - 2.9 Definisanje težinskih koeficijenata za kriterijume
  - 2.10 Faze razvoja odlučivanja i višekriterijumske optimizacije

- **3.METODE ZA VIŠEKRITERIJUMSKU OPTIMIZACIJU**
  - 3.1 Metode za određivanje neinferiornih rešenja 
    - 3.1.1 Metode težinskih koeficijenata
  - 3.2 Metode sa unapred izraženom preferencijom
    - 3.2.1 Metoda Electre
    - 3.2.2 Metoda PROMETHEE
  - 3.3 Interaktivne metode
    - 3.3.1 Metoda STEM
    - 3.3.2 Metoda SEMOPS
  - 3.4 Stohastičke metode
    - 3.4.1 Metoda PROTRADE

]
]

---
## Sadržaj

.small[

.lcol[

- **4.SUPERRAČUNARI I ARHITEKTURE SUPERRAČUNARA**
  - 4.1 Definicije superračunara
  - 4.2 Ciljevi superračunara
  - 4.3 Problemi koji se rešavaju superračunarom
  - 4.4 Upravljanje superračunarima
  - 4.5 Evolucija superračunara
  - 4.6 Uslovi za upravljanje superračunarima
  - 4.7 Koncepti superračunara
  - 4.8 Faze razvoja superračnara
    - 4.8.1 Tipični profili arhitektura superračunara

- **5.FAZI ADAPTIVNI MODEL ZA PROCENU IZVODLJIVOSTI HPC ARHITEKTURA**
  - 5.1 Metodološki pristup izgradnji modela 
    - 5.1.1 Slučajevi korišćenja FAM4FAHPCA
    - 5.1.2 Dijagrami aktivnosti FAM4FAHPCA
  - 5.2 Prilagođavanje modela korisniku
  - 5.3 Implementacija
  - 5.4 Ocenjivanje HPCA

]

.rcol[

- **6.VERIFIKACIJA MODELA**
  - 6.1 Metodološki pristup verifikaciji modela
  - 6.2 Prilagođavanje modela korisniku
  - 6.3 Definisanje, praćenje i kontrola procesa
  - 6.4 Implementacija
  - 6.5 Ocenjivanje HPCA
- **7.REZULTATI ISTRAŽIVANJA**
  - 7.2 Fazi adaptivni model
  - 7.3 Primena modela i verifikacija hipoteza
- **8.ZAKLJUČAK**
- **9.LITERATURA**
- **10.PRILOZI**
- **BIOGRAFIJA**


]
]

---
name: literatura
layout: false
class: center, middle, inverse

# LITERATURA

---
layout: true

.section[[Literatura](#sadrzaj)]

---
## Literatura

.small[
1. Abu S. Masud and C. L. Hwang, Interactive Sequential Goal Programming,  The Journal of the Operational Research Society, Vol. 32, No. 5 (May, 1981), pp. 391-400
1. Alexander Supalov, Andrey Semin, Christopher Dahnken, Optimizing HPC Applications with Intel Cluster Tools, 2014
1. Andrea De Mauro, Marco Greco, Michele Grimaldi, Georgios Giannakopoulos, Dami- anos P Sakas, and Daphne Kyriaki-Manessi. What is big data? A consensual definition and a review of key research topics. In AIP conference proceedings, volume 1644, pages 97–104. AIP, 2015.
1. Benayoun, R., de Montgolfier, J., Tergny, J. et al., Linear programming with multiple objective functions: Step method (stem), Mathematical Programming (1971 1: 366. https://doi.org/10.1007/BF01584098
1. Brian Dr. Tuomanen, Hands-On GPU Programming with Python and CUDA, 2018
1. Brans, J.P., Mareschal, B., PROMETHEE: A new family of outranking methods in multicriteria analysis, Operational Research 84, 1984
1. Brans, J.P., Vincke, Ph., Marechal, B., How to select and how to rank projects: The PROMETHEE method; European Journal of Operational Research, 24, 228- 238, 1986.
1. Chao Wang, High Performance Computing for Big Data: Methodologies and Applications, 2017
1. Charnes A., Cooper WW., Ferguson R., Optimal estimation of executive compensation by linear programming, Management Science, 1, 138-151, 1955
1. Charnes A., Cooper WW.,Management models and industrial applications of linear programming, Wiley, New York, 1961
1. Christoph Niethammer, Michael M. Resch, Wolfgang E. Nagel, Holger Brunst, Hartmut Mix, Tools for  High Performance Computing 2017, Proceedings of the 11th International Workshop on Parallel Tools for High Performance Computing, Dresden, Germany, September 2017
1. Cyrille Rossant, Learning IPython for Interactive Computing and Data Visualization, 2013
1. Duane Storti, Mete Yurtoglu, CUDA for Engineers: An Introduction to High-Performance Parallel Computing, 2015
]

---

## Literatura

.small[

1. Dujmović Jozo, Soft Computing Evaluation Logic - The LSP Decision Method and Its Applications, 2018
1. Dujmović Jozo and H. Nagashima, LSP method and its use for evaluation of Java IDEs, International Journal of Approximate Reasoning 41(1), 2006
1. Feroz Zahid, Network Optimization for High Performance Cloud Computing, Doctoral Dissertation, Faculty of Mathematics and Natural Sciences at the University of Oslo, August, 2017
1. Frank Nielsen, Introduction to HPC with MPI for Data Science, 2016
1. Geoffrey Fox, Judy Qiu, Shantenu Jha, Saliya Ekanayake, and Supun Kamburuga- muve. Big data, simulations and hpc convergence. In Workshop on Big Data Bench- marks, pages 3–17. Springer, 2015.
1. Gerald W. Evans, An Overview of Techniques for Solving Multiobjective Mathematical Programs, Management Science, Vol. 30, No. 11, A Special Issue on Multiple Criteria (Nov., 1984), pp. 1268-1282
1. Haimes Y.Y., Hall W.A., Freedom H.T., Multi-Objective in Water Resources Systems: The Surrogate Worth Trade – Off Method, Elsevier Scientific Publishing Company ,Amesterdam, 1975
1. High Performance Computing, 8th CCF Conference, HPC 2012 Zhangjiajie, China, October 2012
1. Hsinchun Chen, Roger Chiang, and Veda Storey. Business Intelligence and Analytics: From Big Data to Big Impact. MIS Quarterly, 36(4), 2012.
1. Inês Dutra, Rui Camacho, Jorge Barbosa, Osni Marques, High Performance Computing for Computational Science – VECPAR 2016, 12th International Conference Porto, Portugal, June 28–30, 2016
1. International Technical Support Organization, Linux HPC Cluster Installation, 2001
1. Jack R Collins, Robert M Stephens, Bert Gold, Bill Long, Michael Dean, and Stan- ley K Burt. An exhaustive  DNA micro-satellite map of the human genome using high performance computing. Genomics, 82(1):10–19, 2003.
1. João Manuel Paiva Cardoso, José Gabriel de Figueiredo Coutinho, Pedro C. Diniz, Embedded Computing  for High Performance: Efficient Mapping of Computations Using Customization, Code Transformations and Compilation, 2017

]

---

## Literatura

.small[

1. J Michalakes, J Dudhia, D Gill, T Henderson, J Klemp, W Skamarock, and W Wang. The weather research and forecast model: software architecture and performance. In Proceedings of the Eleventh ECMWF Workshop on the Use of High Performance Computing in Meteorology, pages 156–168. World Scientific: Singapore, 2005.
1. Joanna Kołodziej , Florin Pop, Ciprian Dobre, Modeling and Simulation in HPC and Cloud Systems, 2018
1. Julian M. Kunkel, Rio Yokota Michela Taufer, John Shalf, High Performance Computing, ISC High Performance 2017 International Workshops, Frankfurt, Germany, June 18–22, 2017
1. Kevin R. Wadleigh, Isom L. Crawford, Software Optimization for High-Performance Computing, 2000
1. KY Sanbonmatsu and C-S Tung. High performance computing in biology: multimillion atom simulations of nanoscale systems. Journal of structural biology, 157(3):470–480, 2007.
1. Le Lu, Yefeng Zheng, Gustavo Carneiro, Lin Yang, Deep Learning and Convolutional Neural Networks for Medical Image Computing Precision Medicine, High Performance and Large-Scale Datasets 2017
1. Marković, V., Maksimović, R., A Contribution to Continual Software Service Improvement Based On The Six-step Service Improvement Method, International Journal of Software Engineering and Knowledge Engineering, Vol. 22, No. 4, 1-21, Doi: 10.1142/S0218194012005883, 2012
1. M. A. H. Dempster, Juho Kanniainen, John Keane, Erik Vynckier, High-Performance Computing in Finance  Problems, Methods, and Solutions, 2018
1. Marilyn Wolf, High-Performance Embedded Computing, Applications in Cyber-Physical Systems and Mobile Computing, Georgia Institute of Technology SECOND EDITION, 2014
1. Markus Götz, Scalable Data Analysis in High Performance Computing, Doctoral Dissertation, Faculty of Industrial Engineering, Mechanical Engineering and Computer Science, Reykjavík, December 2017
1. Masaaki Geshi, The Art of High Performance Computing for Computational Science, Vol. 1, Techniques of Speedup and Parallelization for General Purposes, 2019
1. Michael A Rappa. The utility business model and the future of computing services. IBM Systems Journal, 43(1):32, 2004.

]

---

## Literatura

.small[

1. Min Chen, Shiwen Mao, and Yunhao Liu. Big Data: A Survey. Mobile Networks and Applications, 19(2):171–209, 2014.
1. Paweł Czarnul, Parallel Programming for Modern High Performance Computing Systems, 2018
1. Rio Yokota , Michèle Weiland John Shalf, Sadaf Alam, High Performance Computing, ISC High  Performance 2018 International Workshops Frankfurt/Main, Germany, June 28, 2018
1. Ritu Arora, Conquering big data with high performance computing, 2016.
1. Rui Sarmento, Vera Costa, Comparative Approaches to Using R and Python for Statistical Data Analysis, A  volume in the Advances in Systems Analysis, Software Engineering, and High Performance Computing, 2017
1. Saaty, T.L.,Analytic hierarchy process, McGraw-Hill, New York, 1980
1. Salvatore Greco, Matthias Ehrgott, José Rui Figueira, Multiple Criteria Decision Analysis, State of the Art Surveys, Second Edition, International Series in Operations Research & Management Science Volume 233, © Springer Science+Business Media New York 2016
1. Shimizu K., Kawabe H., Aiyoshi E., A Theory for Interactive P-reference Optimization and its Algorithm-Generalized SWT method, The Transactions of the Institute of Electronics and Communication Engineering of Japan , Vol.J61- A,No.11,PP.1075-1082, 1978
1. Stephen Jarvis, Steven Wright, Simon Hammond, High Performance Computing Systems, Performance Modeling, Benchmarking, and Simulation, 8th International Workshop, PMBS 2017 Denver, CO, USA, November 13, 2017
1. Sunita Chandrasekaran, Guido Juckeland, Accelerator Programming Using Directives 4th International Workshop, WACCPD 2017, Held in Conjunction with the International Conference for High Performance Computing, Networking, Storage and Analysis, SC 2017 Denver, CO, USA, November 13, 2017
1. Taufer Michela, Bernd Mohr, Julian M. Kunkel, High Performance Computing, ISC High Performance 2016 International Workshops, Frankfurt, Germany, June 19–23, 2016
1. Tomáš Kozubek, Martin Cermák, Petr Tichý, Radim Blaheta, Jakub Šístek, Dalibor Lukáš, Jir Jaroš, High Performance Computing in Science and Engineering, Third International Conference, HPCSE 2017 Karolinka, Czech Republic, May 22–25, 2017

]

---
## Literatura

.small[
1. TOMAŠEVIĆ M., Nebojša RALEVIĆ, Željko STEVIĆ, Vidan MARKOVIĆ, Zdravko TEŠIĆAdaptive Fuzzy Model for Determining Quality Assessment Services in the Supply Chain, Tehnički vjesnik, Vol. 25 No. 6, 2018
1. Thomas Sterling, Matthew Anderson, Maciej Brodowicz, High Performance Computing Modern Systems and Practices, School of Informatics, Computing, and Engineering Indiana University, Bloomington Foreword by C. Gordon Bell, 2018
1. Usama Fayyad, Gregory Piatetsky-Shapiro, and Padhraic Smyth. The KDD Process for Extracting Useful Knowledge from Volumes of Data. Communi- cations of the ACM, 39(11):27–34, 1996.
1. Vernon Turner, John F Gantz, David Reinsel, and Stephen Minton. The digital uni- verse of opportunities: Rich data and the increasing value of the internet of things. IDC Analyze the Future, 2014.
1. Vinai K. Singh David Gao AndreasFischer, Advances in Mathematical Methods and High Performance Computing, 2019
1. Wolfgang GENTZSCH and Hermann LEDERER, DEISA Mini-Symposium on Extreme Computing in an Advanced Supercomputing Environment, Parallel Computing: From Multicores and GPU’s to Petascale 477 B. Chapman et al. (Eds.) IOS Press, 2010
1. Xindong Wu, Xingquan Zhu, Gong-Qing Wu, and Wei Ding. Data Mining with Big Data. IEEE Transactions on Knowledge and Data Engineering, 26 (1):97–107, 2014.
1. Xiuwen Zheng, David Levine, Jess Shen, Stephanie M Gogarten, Cathy Laurie, and Bruce S Weir. A high-performance computing toolset for relatedness and principal component analysis of SNP data. Bioinformatics, 28(24):3326–3328, 2012.


- Internet izvori:
1. High-Performance Computing and EuroHPC initiative, http://europa.eu/rapid/press-release_MEMO-18-5901_en.htm pristupljeno 27.06.2019.
1. HPC wire - Since 1987 - Covering the Fastest Computers in the World and the People Who Run Them https://www.hpcwire.com/ pristupljeno 23.07.2019.
1. Top 500 Super Computer Sites. http://www.top500.org/ pristupljeno 23.07.2019.

]

---
## .center[ *HVALA NA PAŽNJI* ]

.center.medium[
.attention[
**PITANJA ?**]
]



---

class: center, middle, theend
layout: false
background-image: url(../theend.gif)

{% endblock %}