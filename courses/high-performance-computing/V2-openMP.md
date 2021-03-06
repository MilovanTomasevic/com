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

- [Gorana Gojić, MS, Teaching assistant](http://www.acs.uns.ac.rs/en/user/49)
- [Veljko Petrović, Ph.D., Assistant Professor](http://www.acs.uns.ac.rs/en/user/30)
- [Faculty of Technical Sciences](http://ftn.uns.ac.rs/)
- [Computing and Control Department | Chair for Applied Computer Sciences](http://www.acs.uns.ac.rs/en)
]]]
]]

.footer.small[
- #### Slides are created according to sources in the literature & Acknowledgements
]

---

name: sadrzaj

# Sadržaj

- [Uvod](#uvod)
- [OpenMP](#openmp)
- [loop konstrukcija](#loop)
- [Redukcije](#redukcije)
- [Implicitna vs. eksplicitna barijera](#vs)
- [sections/section konstrukcija](#ss)

---
name: uvod 
class: center, middle, inverse

# Uvod

---
layout: true

.section[[Uvod](#sadrzaj)]

---

## Tematske celine
            
- OpenMP 
- OpenMPI 
- OpenACC
        
---

## Softver
            
- Vežbe se izvode na računarima sa instaliranim Ubuntu 18.04 operativnim sistemom.
- Dodatni softverski paketi potrebni za rad: 
  - `gcc`, `g++` (obavezno), `cmake`, `make` (opciono)
  - `libhdf5-dev` za rad sa izgenerisanim podacima za testiranje rešenja (vrlo poželjno)
  - `libopenmpi-dev`, `openmpi-common` i `openmpi-bin` za rad sa OpenMPI programima.
- Odgovarajuće verzije navedenih paketa su dostupne u zvaničnom Ubuntu repozitorijumu. 
- Paketi se mogu instalirati komandom:
    -  `sudo apt install <naziv-paketa>`
  

---
name: openmp 
class: center, middle, inverse
layout: false

# OpenMP

---
layout: true

.section[[OpenMP](#sadrzaj)]

---

## OpenMP

- API za programiranje paralelnih aplikacija na višeprocesnim (eng. *multiprocessing*) mašinama, zasnovan na konceptu deljenja memorije.
- Obuhvata skup kompajlerskih direktiva (eng. *compiler directives*), bibliotečkih rutina (eng. *library routines*) i promenljivih okruženja (eng. *environment variables*).
- Postoji podrška za programske jezike C, C++ i Fortran.
  
---

## Fork-join model

  ![:scale 70%](img/Fork_join.png)
  .medium[
- Paralelni region (eng. *Parallel region*) - deo programa koji izvršava tim niti
- `Master nit`
- `Novokreirane niti` - identifikatori od 1 do N − 1, ne postoje nakon paralelnog regiona u kojem su kreirane
- Sve niti u paralelnom regionu čine `tim niti`.
  ]

---

## Fork-join model

- U OpenMP se tim niti kreira korišćenjem parallel konstrukcije 
  - (eng. *parallel construct*)
  .center[
```c
#pragma omp parallel[clause[ [,] clause] ... ]
strukturirani-blok
```
  ]
- Svaka izvršava isti segment koda 
  - (*Single Process Multiple Data* (SPMD)).
  
---

## Hello World!

.medium[
.message.is-dark[
.message-header[
Primer
]
.message-body[
```c
    #include <stdio.h> 
    #include <omp.h>

    int main() {

        #pragma omp parallel
        {
            int id = omp_get_thread_num();
            printf("Hello(%d)", id);
            printf(" world!(%d)\n", id);
        }
        return 0; 
    }
```

```console
    mt@mt:~/courses/high-performance-computing$ gcc -o hello hello.c -fopenmp
    mt@mt:~/courses/high-performance-computing$ ./hello
    Hello(7) world!(7)
    Hello(2) world!(2)
    Hello(4) world!(4)
    Hello(3) world!(3)
    Hello(1) world!(1)
    Hello(0) world!(0)
    Hello(5) world!(5)
    Hello(6) world!(6)
```
]
]
]

---

## Hello World!

- Jedan primer izvršavanja:
  

- Iako nigde u primeru broj niti nije zadat, OpenMP radno okruženje (eng. *execution environment*) je odlučilo da napravi 8 niti.
- Tipično, OpenMP radno okruženje pravi onoliko niti koliko ima jezgara (fizičkih ili logičkih). 
- Npr. ovaj ispis je dobijen izvršavanjem na Intelovom procesoru Intel® Core™ i7-8550U (8 logičkih jezgara).

---

## Hello World!

- Da li bi se program brže izvršio ako bismo eksplicitno zatražili, na primer, 8 niti?
  - `omp_set_num_threads() OMP_NUM_THREADS`
- Moguće je i da radno okruženje *ne napravi* onoliko niti koliko ste od njega zatražili usled postojećih ograničenja!
- Druge interesantne funkcije: 
  - `omp_get_num_threads, omp_get_thread_num, omp_get_max_threads`

---

## Kompajliranje i pokretanje

- Kako kompajlirati? 
  - Otvoriti terminal, pozicionirati se u direktorijum u kojem se nalazi izvorna datoteka (izvornad.c) i uneti:
  .center[
    ```console
    gcc [-g] [-o izvrsnad] izvornad.c -fopenmp [-O2]
    ```
  ]
- U slučaju da postoji više izvornih datoteka, potrebno ih je sve navesti. Delovi u uglatim zagradama nisu obavezni.
- Opcije:
    - `-g` - omogućava prikupljanje podataka za debagovanje
    - `-o` - specificira naziv izlazne datoteke, u ovom slučaju to je izvršna datoteka
    - `-O2` | `-O3` - indikator kompajleru da primeni određene skupove optimizacija (O3 veći skup optimizacija od O2). 
    - Upotreba ovih opcija se *ne preporučuje* u kombinaciji sa `-g` opcijom!

  
---

## Kompajliranje i pokretanje

- Kako pokrenuti iskompajlirano rešenje? 
  - Otvoriti terminal, pozicionirati se u direktorijum u kojem se nalazi izvršna datoteka (izvrsnad) i uneti:
  .center[
    ```console
    ./izvrsnad <lista-argumenata>
    ```
  ]
- U slučaju da pri kompajliranju nije navedeno ime izvršne datoteke, ona će se podrazumevano zvati a.out.
 

---

## Zadatak 1: Računanje vrednosti broja π

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Korišćenjem samo parallel konstrukcije, paralelizovati program koji računa vrednost integrala

<p>

$$ \int_{0}^{1} \frac{4}{(1+x^{2})} $$
  
</p>

- Sekvencijalna implementacija programa u C programskom jeziku je data u direktorijumu zadaci. Rezultat integraljenja bi trebalo da bude jednak broju π. Potrebno je dodati parallel konstrukciju bez daljih modifikacija sekvencijalnog programa.

- Šta se dešava sa rezultatom?
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Funkcija `parallel_code_incorrect` <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`pi.c`</a>
]
]

---

## Tipovi promenljivih
- **Deljene**, odnosno **globalne** - Prostor za ove promenljive se
zauzima na hipu, kome pristupaju sve niti. 
- *Potencijalni kandidati za štetno preplitanje*! 
- U deljene promenljive se ubrajaju:
	- statičke promenljive i
	- promenljive alocirane izvan paralelnog regiona (na primer promenljiva sum iz zadatka 1)

- **Privatne**, odnosno **lokalne** - Vidljive samo u okviru niti u kojoj su deklarisane. Prostor za ove promenljive se zauzima na `steku niti`. 
- U privatne promenljive se ubrajaju:
	- promenljive deklarisane unutar paralelnog regiona (x)
	- promenljive promenljive deklarisane unutar funkcije koja je pozvana iz paralelnog regiona i
	- brojačke promenljive u `for` petlji prvog nivoa

- `OMP_STACKSIZE` za upravljanje veličinom steka niti

---

## Zadatak 2: Računanje vrednosti broja π

.message.is-info[
.message-header[
Zadatak
]
.message-body[

- Modifikovati rešenje prethodnog zadatka tako da se ukloni štetno preplitanje.

]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Funkcija `parallel_code` <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`pi.c`</a>
]
]

---

## Zadatak 3*: Računanje vrednosti broja π

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Paralelno rešenje zadatka 2 eliminiše problem štetnog preplitanja, ali uvodi problem lažnog deljenja (eng. *false sharing*) pri pristupu nizovnoj promenljivoj `sum`.
- Izmeniti rešenje tako se otkloni lažno deljenje.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- Razmisliti o tome koliko se elemenata niza prenosi u keš procesora kada se pristupa jednom elementu.
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Funkcija `parallel_code_no_false_sharing` <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`pi.c`</a>
]
]

---

## Sinhronizacioni konstrukti
- Sinhronizacija visokog nivoa apstrakcije:
    - barrier construct - definiše tačku u kodu do koje sve aktivne niti moraju da se zaustave dok do te tačke ne stigne i poslednja nit, nakon čega sve niti mogu nastaviti dalje izvršavanje. `#pragma omp barrier`


- `critical construct` - samo jedna nit može u jednom trenutku biti u kritičnoj sekciji.
```c
	#pragma omp critical
		strukturirani-blok
```

- `atomic construct` - hardverski podržan isključiv pristup ažuriranju vrednosti proste promenljive. Ukoliko nema hardverske podrške, ova konstrukcija se ponaša kao i `critical` - `#pragma omp atomic`

---

## Zadatak 4: Računanje vrednosti broja pi

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Doraditi rešenje zadatka 2 tako da se štetno preplitanje ukloni odgovarajućim sinhronizacionim mehanizmom.
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Funkcija  `parallel_code_synchronization` <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`pi.c`</a>
]
]

---

## Konstrukcije za podelu posla

- eng. *worksharing constructs*

	- `loop` konstrukcija (eng. *loop construct*)
	- `sections/section` konstrukcija (eng. *sections/section construct*)
	- `single` konstrukcija (eng. *single construct*)

- Na kraju bloka koda koji se izvršava u sklopu neke od konstrukcija za podelu posla podrazumevano postoji `implicitna barijerna sinhronizacija`.
- Podrazumevano ponašanje se može promeniti navođenjem `nowait` klauze u okviru direktiva za podelu posla (primer kasnije).

---

## Implicitna barijerna sinhronizacija

- Implicitna barijerna sinhronizacija se takođe podrazumevano  nalazi i na kraju paralelnog regiona, ali je za razliku od konstrukcija za podelu posla, nije moguće odatle ukloniti.

- Zašto?

---

name: loop 
class: center, middle, inverse
layout: false

# loop konstrukcija

---
layout: true

.section[[loop konstrukcija](#sadrzaj)]

---

## loop konstrukcija

- Sintaksa:
 ```c
 #pragma omp for [clause[ [,] clause] ... ]
    for-petlje
```

- Problemi u računarstvu visokih performansi se često svode na rad sa velikim nizovima! To znači da često postoji iteriranje kroz nizove...
- Taj posao može da se podeli na više niti! Svaka nit u paraleli može obraditi parče niza.

- Ovaj način raspodele podataka ste već implementirali u zadatku 2, ali ste sami morali da specificirate granice niza nad kojima radi svaka pojedinačna nit.

---

## Primer 2: `loop` konstrukcija

.message.is-dark[
.message-header[
Primer
]
.message-body[
- Primer upotrebe `for` direktive
```c
#pragma omp parallel
    {
        int sum = 0;
        #pragma omp for
        for (int i = 0; i < N; i++)
            sum += A[i];
    }
```

- Korišćenjem kombinovane konstrukcije:
```c
    #pragma omp parallel for
    {
        int sum = 0;
        for (int i = 0; i < N; i++)
            sum += A[i];
    }
```
]
]

---

## loop konstrukcija: raspoređivanje

- **Kako će iteracije petlje biti dodeljene nitima?**
- OpenMP podržava više strategija raspoređivanja koje se mogu specificirati schedule klauzom.

```c
#pragma omp for schedule(tip [,velicina_bloka])
```

---

## loop konstrukcija: raspoređivanje

- **Kako će iteracije petlje biti dodeljene nitima?**
	- `static` - blokovi iteracija se dodeljuju nitima u vreme kompajliranja po `round-robin` principu

	- `dynamic` - blokovi iteracija se dodeljuju nitima u vreme izvršavanja tako da opterećenje niti bude optimalno

	- `guided` - modifikacija dinamičkog raspoređivanja gde je svaki naredni blok dodeljen niti manji od prethodnog

	- `auto`  - kompajler određuje tip raspoređivanja koji misli da je najbolji za problem 
	- `runtime` - moguće je "spolja" uticati na tip raspoređivanja preko `OMP_SCHEDULE` promenljive okruženja

---

name: redukcije 
class: center, middle, inverse
layout: false

# Redukcije

---
layout: true

.section[[Redukcije](#sadrzaj)]

---

## Redukcije

- Prisetimo se nepotpunog paralelnog sabiranja elemenata niza iz primera 2
```c
    #pragma omp parallel for
    {
        int sum = 0;
        for (int i = 0; i < N; i++) {
            sum += A[i];
        } 
    }
    /* saberi parcijalne sume */
```

- Da bi program bio potpuno funkcionalan, potrebno je posabirati parcijalne sume koje sračunaju niti.

---

## Redukcije

- Može da se uradi ovako:
```C
	int sum = 0;
	#pragma omp parallel for
{
		for (int i = 0; i < N; i++) {
				#pragma omp critical
				sum += A[i];
		}
}
``` 
- I u deljenoj promenljivoj sum će biti konačan rezultat. 
- Ali ovo je **MNOGO** neefikasno!

---

## Redukcije

- A može i ovako:
```c
	int sum = 0;
	#pragma omp parallel for reduction(+:sum)
{
		for (int i = 0; i < N; i++) {
				sum += A[i];
		}
}
```
- Šta zapravo znači `reduction(+:sum)?`

--

.message.is-warning[
.message-header[
Info
]
.message-body[
1. Za svaku nit u paralelnom regionu napravi po jednu privatnu
instancu promenljive `sum` i inicijalizuj je na vrednost neutralnu
za navedeni redukcioni operator (u slučaju sabiranja je to 0).

2. Svaka nit menja svoju kopiju promenljive `sum.`

3. Na kraju petlje, rezultati se kombinuju upotrebom redukcionog
operatora u deljenu promenljivu `sum.`

]
]

---

## Redukcije
- Opšti format redukcije:
```c
reduction(redukcioni_operator : lista_promenljivih)
``` 
- Ugrađeni redukcioni operatori za C/C++:
  
.center-table.small[

| **Operacija** |      **Vrednost**      |
|:-------------:|:----------------------:|
|       +       |            0           |
|       *       |            1           |
|       -       |            0           |
|      min      | najveći pozitivan broj |
|      max      | najveći negativan broj |
|       &       |           ~0           |
|      I        |            0           |
|       ˆ       |            0           |
|       &&      |            1           |
|     II        |            0           |

]

---
## Zadatak 5: Računanje vrednosti broja pi

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati paralelno rešenje računanja vrednosti broja pi uz
korišćenje `for`  direktive i reduction klauze.
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Funkcija `parallel_code_for_construct` <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`pi.c`</a>
]
]

---

name: vs 
class: center, middle, inverse
layout: false

# Implicitna vs. eksplicitna barijera

---
layout: true

.section[[Implicitna vs. eksplicitna barijera](#sadrzaj)]

---
## Implicitna vs. eksplicitna barijera 

- Eksplicitna barijera je zadata `#pragma omp barrier direktivom`.
- Implicitna barijera je barijera već uključena u neku konstrukciju (npr. `for`  konstrukciju).

---
## Implicitna vs. eksplicitna barijera 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kojem delu koda će se niti sinhronizovati?
```c
	#pragma omp parallel
	{
			/* prvi blok naredbi */
			#pragma omp barrier
			/* drugi blok naredbi */
    }
```
]
]

---

## Implicitna vs. eksplicitna barijera 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kojem delu koda će se niti sinhronizovati?
```c
	#pragma omp parallel
	{
			/* prvi blok naredbi */
			#pragma omp barrier
			/* drugi blok naredbi */
    }
```
]
]
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Na barrier direktivi (eksplicitna barijerna sinhronizacija).
]
]

---
## Implicitna vs. eksplicitna barijera

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kojem delu koda će se niti sinhronizovati?
```c
	#pragma omp parallel for
	for (i = 0; i < N; i++)
		/* prvi blok naredbi */
	/* drugi blok naredbi */
```
]
]

---
## Implicitna vs. eksplicitna barijera 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kojem delu koda će se niti sinhronizovati?
```c
	#pragma omp parallel for
	for (i = 0; i < N; i++)
		/* prvi blok naredbi */
	/* drugi blok naredbi */
```
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Na kraju prvog bloka naredbi. Sve niti moraju da završe svoje iteracije petlje da bi mogle da nastave sa drugim blokom naredbi, jer podrazumevano `loop` konstrukcija ima ugrađenu implicitnu barijeru.
]
]

---
## Implicitna vs. eksplicitna barijera 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kojem delu koda će se niti sinhronizovati?
```c
	#pragma omp parallel for nowait
	for (i = 0; i < N; i++)
		/* prvi blok naredbi */
    /* drugi blok naredbi */
``` 
]
]

---
## Implicitna vs. eksplicitna barijera 

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kojem delu koda će se niti sinhronizovati?
```c
	#pragma omp parallel for nowait
	for (i = 0; i < N; i++)
		/* prvi blok naredbi */
    /* drugi blok naredbi */
``` 
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Na kraju paralelnog regiona. Implicitna barijera loop konstrukcije je onemogućena upotrebom `nowait` klauze.
]
]

---
name: ss 
class: center, middle, inverse
layout: false

# sections/section konstrukcija

---
layout: true

.section[[sections/section konstrukcija](#sadrzaj)]

---
## sections/section konstrukcija

- Svaka nit unutar sections konstrukcije izvršava jedan blok koda koji pripada sekciji.

- Sintaksa:
```c
	#pragma omp sections [clause[ [,] clause] ... ]
	{
		[ #pragma omp section ]
				strukturirani-blok
		[ #pragma omp section
				strukturirani-blok ]
	...
} 
``` 

---
## Primer 3: sections/section konstrukcija

.message.is-dark[
.message-header[
Primer
]
.message-body[
```c
	#pragma omp parallel
	{
		#pragma omp sections
		{
		#pragma omp section
			x_calculation();
		#pragma omp section
			y_calculation();
		#pragma omp section
			z_calculation();
		}
	}
```
]
]

---
## single konstrukcija

- Sintaksa:
```c
#pragma omp single [clause[ [,] clause] ... ]
    strukturirani-blok
``` 

- Blok naredbi izvršava samo nit koja prva uđe u strukturirani blok.
```c
	#pragma omp parallel
	{
		do_many_things();
		#pragma omp single
			{ exchange_boundaries(); }
		#pragma omp barrier
			do_many_other_things();
	}
```
---
## master konstrukcija

- Sintaksa:
```c
#pragma omp master
strukturirani-blok
```

- Blok naredbi izvršava samo master nit.
```c
	#pragma omp parallel
	{
		do_many_things();
		#pragma omp master
			{ exchange_boundaries(); }
		#pragma omp barrier
				do_many_other_things();
	}
```

- Nema implicitne sinhronizacije.

---
## Sihnronizacioni mehanizmi: lock

- Pripada mehanizmima niskog nivoa sinhronizacije.

- Analogni pojam propusnici iz C++11 višenitnog okruženja.

- `critical` direktiva u pozadini koristi `lock`.  
- Zašto biste onda ikada želeli da direktno koristite ovu direktivu?

- Nema problema, ako na primer, ograničavate pristup jednoj celobrojnoj promenljivoj kroz kritičnu sekciju.  
- Ali šta će se desiti ako kroz kritičnu sekciju ograničite pristup elementima nekog niza?

---
## Primer 3: histogram

![:scale 65%](img/histogram.png)

.center[
    `int` histogram[255];
]

---
## Primer 4: histogram

```C
	#pragma omp parallel for
	for (i = 0; i < NBUCKETS; i+) {
		omp_init_lock(&hist_locks[i]);
		hist[i] = 0;
    }

	#pragma omp parallel for
	for (i = 0; i < NVALS; i++) {
		ival = (int) sample(arr[i]);
		omp_set_lock(&hist_locks[ival]);
			hist[ival]++;
		omp_unset_lock(&hist_locks[ival]);
    }

    for (i = 0; i < NBUCKETS; i++) {
		omp_destroy_lock(&hist_locks[i]);
    }
``` 

---
## Klauze za rad sa podacima

- `shared`(<lista-promenljivih>)
- `private`(<lista-promenljivih>)
- `firstprivate`(<lista-promenljivih>)
- `lastprivate`(<lista-promenljivih>)
- `default( private | shared | none )` - Ako se ništa ne navede za ovu klauzu, podrazumevana vrednost u C i C++ programskim jezicima je `shared.`

---
## Primer 5: Klauze za rad sa podacima

.message.is-info[
.message-header[
Zadatak
]
.message-body[
```c
	void dummy() {
		int tmp = 0;
	#pragma omp parallel for private(tmp)
	for (int j = 0; j < 5; j++)
		tmp += j;
	printf("%d\n", tmp);
	}
```
- Koja vrednost će biti ispisana na standardni izlaz?
]
]

---
## Primer 5: Klauze za rad sa podacima

.message.is-info[
.message-header[
Zadatak
]
.message-body[
```c
	void dummy() {
		int tmp = 0;
	#pragma omp parallel for private(tmp)
	for (int j = 0; j < 5; j++)
		tmp += j;
	printf("%d\n", tmp);
	}
```
- Koja vrednost će biti ispisana na standardni izlaz?
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[

- Na standardni izlaz će biti ispisana vrednost 0.
- **Objašnjenje**:  <br> Kako je promenljiva tmp proglašena privatnom, svaka nit će imati svoju instancu ove promenljive. Po završetku petlje, lokalne promenljive će biti uništene, a deljena promenljiva `tmp` će zadržati svoj u inicijalnu vrednost.

]
]

---
## Primer 5: Klauze za rad sa podacima

.message.is-info[
.message-header[
Zadatak
]
.message-body[
```c
	void dummy() {
		int tmp = 0;
		#pragma omp parallel for private(tmp)
		for (int j = 0; j < 5; j++)
				tmp += j;
			printf("%d\n", tmp);
}
```
- Koja je inicijalna vrednost privatnih verzija promenljive `tmp`?
]
]

---
## Primer 5: Klauze za rad sa podacima

.message.is-info[
.message-header[
Zadatak
]
.message-body[
```c
	void dummy() {
		int tmp = 0;
		#pragma omp parallel for private(tmp)
		for (int j = 0; j < 5; j++)
				tmp += j;
			printf("%d\n", tmp);
}
```
- Koja je inicijalna vrednost privatnih verzija promenljive `tmp`?
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Inicijalna vrednost privatnih promenljivih tmp je nepoznata.
- **Objašnjenje**:  <br>Klauza private`(tmp)` kaže kompajleru da treba da alocira prostor za promenljivu tmp na steku. Kompajler ne mora inicijalizovati zauzetu lokaciju.
]
]

---
## Primer 5: Klauze za rad sa podacima

.message.is-info[
.message-header[
Zadatak
]
.message-body[
```c
	void dummy() {
		int tmp = 0;
		#pragma omp parallel for private(tmp)
		for (int j = 0; j < 5; j++)
			tmp += j;
		printf("%d\n", tmp);
	}
```
- Kojom klauzom je potrebno zameniti private klauzu da bi lokalne
instance promenljive tmp dobile inicijalnu vrednost globalne
promenljive `tmp`?
]
]

---

## Primer 5: Klauze za rad sa podacima

.message.is-info[
.message-header[
Zadatak
]
.message-body[
```c
	void dummy() {
		int tmp = 0;
		#pragma omp parallel for private(tmp)
		for (int j = 0; j < 5; j++)
			tmp += j;
		printf("%d\n", tmp);
	}
```
- Kojom klauzom je potrebno zameniti private klauzu da bi lokalne
instance promenljive tmp dobile inicijalnu vrednost globalne
promenljive `tmp`?
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- `firstprivate`
]
]

---

## Zadatak 6: Mandelbrotov set

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Data je datoteka `mandelbrot.c.` Datoteka sadrži paralelno OpenMP rešenje koje određuje Mandelbrotov set. 
- Rešenje ima par problema vezanih za korišćenje klauza za rad sa podacima i poneko štetno preplitanje. Pronađite i ispravite greške.
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`mandelbrot.c`</a>
]
]

---

## Kako paralelizovati `while` i rekurziju?

- Inicijalno, OpenMP je zamišljen tako da je moguće paralelizovati
probleme za koje se unapred zna broj potrebnih iteracija za njihovo
rešavanje!

--

.message.is-danger[
.message-header[
Problem
]
.message-body[ 
- Kako primeniti OpenMP u drugim tipovima petlji u kojima se ne zna unapred broj iteracija? Ili u slučaju rekurzije?
]
]

--

.message.is-warning[
.message-header[
Info
]
.message-body[
- Opcije:
	- Problem transformisati u formu `for` petlje ako je to moguće
	- Koristiti `task` konstrukciju (od OpenMP 3.0)
]
]

---
## Primer 6: Paralelizacija obrade čvorova liste

.lcol[
```c
// petlja koju
// treba paralelizovati
while (p != NULL) {
	processwork(p);
	p = p->next;
}
```
]

.rcol[
```c
// 1. odrediti broj cvorova
while (p != NULL) {
	nelems++; p = p->next;
}
...

// 2. zapamtiti adrese cvorova
p = head;
for (i = 0; i < nelems; i++) {
	ptrs[i] = p;
	p = p->next;
}
...

// 3. pokrenuti paralelnu obradu
#pragma omp parallel for
for (i = 0; i < nelems; i++)
	processwork(ptrs[i]);
```
]

---
## task konstrukcija

- Sintaksa:
```c 
#pragma omp task[clause[ [,] clause] ... ]
strukturirani-blok
```

- Može se posmatrati kao nezavisna jedinica posla.
- Čine ga:
	- Kod koji zadatak izvršava
	- Podaci (privatne i deljene promenljive)
	- `Internal Control Variables (ICV)` (npr. indikator da li zadatak može da bude dodeljen različitim nitima, vrsta raspoređivanja, broj niti u paralelnom regionu itd.)

---
## Primer 7: Kreiranje zadataka

- `task i single` konstrukcije se često koriste zajedno:
	- jedna nit pravi zadatke koji se uvezuju u red zadataka odakle sve niti mogu da uzimaju zadatke.

```c
	#pragma omp parallel
	{
		#pragma omp single
		{
				#pragma omp task
				y = f(x)
				#pragma omp task
				z = g(x)
		}
	}
```

---
## Zadatak 7: Modifikacija liste

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Data je sekvencijalna implementacija liste `(linked.c)` u kojoj svaki element sadrži po jedan Fibonačijev broj dobijen funkcijom `processwork`. 
- Napraviti OpenMP paralelni program korišćenjem `task` konstrukcije.
]
]

--
.message.is-success[
.message-header[
Odgovor
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z1-openMP/#table-of-contents"> ☛ rešenja/`linkedlist.c`</a>
]
]

---
## Konstrukti za sinhronizaciju izvršavanja zadataka

- `taskwait` - sinhronizacija samo zadataka istog nivoa
    ```c 
    #pragma omp taskwait
    ```

- `taskgroup` - sinhronizuje i podzadatke

    ```c
    #pragma omp taskgroup
        strukturirani-block
    ``` 

- `depend` - task klauza

    ```c 
    depend(in | out | inout : lista)
    ```

---
## Primer 8: Sinhronizacija zavisnih zadataka

```c 
	int y, z;
	#pragma omp parallel
	{
		#pragma omp single
		{
				#pragma omp task
				y = f(x)
				#pragma omp taskwait
				#pragma omp task
				z = g(y)
		}
	}
```
---

## Primer 8: Sinhronizacija zavisnih zadataka

```c
	#pragma omp parallel
	{
		int y, z;
		#pragma omp single
		{
				#pragma omp task depend(out:y)
				y = f(x)
				#pragma omp task depend(in:y)
				z = g(y)
		}
	}
```

---

## Zadatak 8*: Množenje matrice i vektora

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati sekvencijalni program za množenje nekvadratne matrice i vektora u C programskom jeziku.

- Nakon što se uverite da program daje očekivane rezultate, implementirani OpenMP paralelni algoritam na osnovu sekvencijalnog programa.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**: Svaka od dimenzija matrice treba da bude makar 1000. Za potrebe testiranja programa dimenzije matrice mogu da budu i manje.
    Meriti izvršavanje programa funkcijom `omp_get_wtime()`.
]
]

---

## Zadatak 9*: Množenje matrica - domaći

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati sekvencijalni program za množenje dve nekvadratne
matrice u C programskom jeziku.

- Nakon što se uverite da program daje očekivane rezultate,
implementirati OpenMP paralelni program na osnovu
sekvencijalnog programa.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**: Korektan sekvencijalni program testirati na velikim matricama (oko 1000 po dimenziji, modifikovati zavisno od karakteristika računara na kojem radite zadatak). 
    Za potrebe testiranja, dimenzije matrica mogu da budu i manje.
    Meriti izvršavanje programa funkcijom `omp_get_wtime()`.
]
]

---

## Zadatak 10*: Akumuliranje vrednosti čvorova stabla

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napraviti sekvencijalnu implementaciju stabla u C programskom jeziku. 
- Svaki čvor stabla sadrži jedan razlomljeni broj u jednostrukoj preciznosti. 
- Potrebno je implementirati minimalni skup funkcionalnosti (kreiranje stabla, sabiranje vrednosti čvorova stabla i uništavanje stabla).

 - Nakon što se uverite da program daje očekivane rezultate implementirati OpenMP paralelni program na osnovu sekvencijalnog programa
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**:
Paralelni program implementirati korišćenjem task konstrukcije.
Meriti izvršavanje programa funkcijom `omp_get_wtime()`.
]
]

---

## Zadatak 11: Transponovanje matrice

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Data je sekvencijalna implementacija transponovanja matrice <br><a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z2-openMP/#table-of-contents"> ☛ zadaci/`Matrix Transpose`</a>. Napraviti OpenMP paralelnu verziju algoritma.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**: Meriti izvršavanje programa funkcijom `omp_get_wtime()`.
]
]

---

## Zadatak 12: Jednostavni genetski algoritam

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Data je sekvencijalna implementacija jednostavnog genetskog algoritma implementiranog u C programskom jeziku <br><a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z2-openMP/#table-of-contents"> ☛ zadaci/`Genetic Algorithm`</a>.  
- Pokrenuti sekvencijalni algoritam nad svim datim primerima prema uputstvu u <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z2-openMP/#table-of-contents"> ☛ zadaci/`Genetic Algorithm/ReadMe`</a> i analizirati vremena izvršavanja delova genetskog algoritma.
- Odrediti kritične delove koda i paralelizovati ih korišćenjem OpenMP.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**: Meriti izvršavanje programa funkcijom `omp_get_wtime()`.
]
]

---
## Zadatak 13*: Traženje korena funkcije nad intervalom - domaći

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Data je sekvencijalna implementacija metode za određivanje korena funkcije nad zadatim intervalom metodom bisekcije <br><a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z2-openMP/#table-of-contents"> ☛ zadaci/`Bisection`</a>.  
- Pokrenuti sekvencijalni algoritam nad svim datim primerima prema uputstvu u <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z2-openMP/#table-of-contents"> ☛ zadaci/`Bisection/ReadMe`</a> i pogledati rešenja sva tri zadata primera.
- Zatim implementirati OpenMP paralelno rešenje.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**: Algoritam isprobati na još primera. Pri proveri tačnosti dobijenog rešenja moguće je koristiti neki od alata za određivanje korena funkcije (Volfram, na primer). Meriti izvršavanje programa funkcijom `omp_get_wtime()`.
]
]

---
## Korišćeni materijali

- Dokumentacija sa OpenMP sajta, videti `www.openmp.org`
- `"Introduction to OpenMP", Tim Mattson`, dostupno na [ovom linku](https://www.youtube.com/playlist?list=PLLX-Q6B8xqZ8n8bwjGdzBJ25X2utwnoEG)
- [`"Introduction to OpenMP"`, prateća prezentacija](https://www.openmp.org/wp-content/uploads/Intro_To_OpenMP_Mattson.pdf)
- `"Parallel Computing Book"`, Victor Eijkhout, elektronska verzija knjige je dostupna na [ovom linku](http://pages.tacc.utexas.edu/~eijkhout/pcse/html/omp-basics.html)

--

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}
