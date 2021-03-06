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

- [OpenACC](#acc)
- [kernels direktiva](#kd)
- [parallel direktiva](#pd)
- [loop direktiva](#ld)
- [Model](#model)
- [data direktiva](#dd)
- [Zadaci](#zad)

---
name: acc
class: center, middle, inverse

# OpenACC

---
layout: true

.section[[OpenACC](#sadrzaj)]

---
## Šta je OpenACC

- API za prebacivanje izvršavanja delova koda na **akcelerator**.
- Obuhvata:
	- direktive (`pragma acc <direktiva>`)
	- promenljive okruženja (eng. *runtime environment variables*)
	- bibliotečke rutine (eng. *library routines*)
- Podrška za C, C++ i Fortran.
- Konceptualno veoma sličan OpenMP.

---

## Ciljna arhitektura

- Akceleratori.

.lcol[

![:scale 75%](img/acc2.png)
]

.rcol[

![:scale 110%](img/acc.jpg)

]

<br><br><br><br><br><br><br><br><br><br> 
- U laboratoriji ćemo kao akcelerator koristiti **Nvidia Quadro** grafičke kartice.

---

## OpenACC kompajleri

![:scale 90%](img/compile.png)

- Na vežbama ćemo koristiti GNU GCC kompajler.

.footer.medium[
[Izvor - OpenACC](https://www.openacc.org/tools)
] 

---

## Kompajliranje OpenACC programa

- Pozicionirati se u direktorijum u kojem se nalazi datoteka sa OpenACC kodom i u terminalu uneti:
```console
gcc -o izvrsna_dat izvorna_dat.c -fopenacc
```

- Za pokretanje programa, pozicionirati se u direktorijum u kojem je izvršna datoteka i uneti `./izvrsna_dat`.
- Ukoliko program poziva neku od OpenACC funkcija (naziv počinje sa acc_), u izvornu datoteku je potrebno dodati i
```c
#include <openacc.h>
```

---

## Terminologija

- **Domaćin** (eng. *Host*) - centralna procesna jedinica sa svojom hijerarhijom memorije.
- **Akselerator** (eng. *Accelerator* ili *Device*) - akseleratorski uređaj, npr. grafička kartica.
- **Paralelni region** - Deo koda obeležen za izvršavanje na akceleratorskom uređaju sa pridruženim strukturama podataka. Obuhvata regione koda obeležene `parallel` i `kernels` direktivama (drugačije će se zvati i računski regioni).

---

## OpenACC model izvršavanja

![:scale 50%](img/accModel.jpg)

.center[
`Figure`: Konceptualna arhitektura akseleratora.ref[*]
]

OpenACC podržava tri nivoa paralelizma: `gang, worker, vector`

.footer.medium[
  [* Link](https://www.sciencedirect.com/topics/computer-science/target-machine)
]

---

## OpenACC model izvršavanja

Opšti format OpenACC direktive:

```c
#pragma acc <directive> [clause-list] new-line
structured block
```
- Direktive za obeležavanje paralelnog koda:
	- `kernels`
	- `parallel`

---
layout: false
name: kd
class: center, middle, inverse

# kernels direktiva

---
layout: true
.section[[kernels direktiva](#sadrzaj)]

---

## kernels direktiva

- Označava deo koda koji može biti preveden za izvršavanje na akceleratoru pravljenjem jednog ili više kernela. 
- Kompajler odlučuje `šta` će i `kako` će paralelizovati.
```c
#pragma acc kernels [clause-list] new-line
structured block
```
- Neke od klauzula (parametri nisu navedeni):

.lcol[

- async
- copyin
- wait
]

.rcol[
- copyout
- copy
- ...
]

---

## Primer 1: kernel.c

```c
int main() {

	/* ... */
	#pragma acc kernels
	{
		for(i = 0; i < MATRIX_SIZE; i++)
			for(j = 0; j < MATRIX_SIZE; j++)
				randomMatrix[i * MATRIX_SIZE + j] =
				randomMatrix[i * MATRIX_SIZE + j] * 2;
	}

	return 0;
}

```

---

## Primer 2: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z6-openACC/#table-of-contents"> ☛ Primeri/`ptraliasing.c`</a>

.message.is-dark[
.message-header[
Primer
]
.message-body[
```c
void assign(int *a, int *b, int size) {
	#pragma acc kernels
	{
		for (int i = 0; i < size - 1; i++)
			a[i] = b[i + 1];
	}
}
``` 
]
]

--
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- **Pitanje**: Kada se koristi kernels direktiva, kompajler pronalazi delove koda koji su bezbedni za paralelizaciju, odnosno, u kojima nema zavisnosti među podacima. Šta mislite, da li bi OpenACC kompajler preveo ovo u kod za paralelno izvršavanje?
]
]

---

layout: false
name: pd
class: center, middle, inverse

# parallel direktiva

---
layout: true
.section[[parallel direktiva](#sadrzaj)]

---

## parallel direktiva

- Označava deo koda koji će biti preveden za izvršavanje na akceleratoru. 
Kompajler određuje **kako** će segment koda biti paralelizovan. 
Podrazumevano izvršavanje kreće u `gang-redundant` režimu.
```C
#pragma acc parallel [clause-list] new-line
structured block
```
Neke od klauzula (parametri nisu navedeni):

.lcol[

- async
- wait
- num_gangs
- num_workers
- vector_length
]

.rcol[
- reduction
- copy
- copyin
- copyout
- ...
]

---

## Primer 3: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z6-openACC/#table-of-contents"> ☛ Primeri/`parallel.c`</a>

.message.is-dark[
.message-header[
Primer
]
.message-body[
```c
#include <openacc.h>

int main() {
	float *values = (float *) malloc(sizeof(float) * size);

	#pragma acc parallel
	for (int i = 0; i < 1024; i++)
		values[i] = 1.f;
	free(values);

	return 0;
}
```
]
]

--
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- **Pitanje**: Koliko puta će svakom polju value niza biti dodeljena vrednost?
]
]

---

## parallel klauzule

- `async` - Nit koja je naišla na parallel ili kernels direktivu (lokalna nit) može da nastavi izvršavanje koda koji sledi iza paralelnog regiona bez čekanja da uređaj završi svoj posao.
- `wait` - Kada naiđe na ovu klauzulu, lokalna nit čeka.
- `num_gangs(int-exp)` - Broj `gang`-ova koji izvršavaju paralelni region.
- `num_workers(int-exp)` - Broj radnika unutar `gang`-a.
- `vector_length(int-exp)` - Dužina vektora koja se koristi za SIMD operacije.
- `reduction(op:var-list)` - Pravi lokalnu kopiju promenljive i inicijalizuje je. Na kraju paralelnog regiona se lokalne kopije redukuju.
- `copy, copyin, copyout`

---

layout: false
name: ld
class: center, middle, inverse

# loop direktiva

---
layout: true
.section[[loop direktiva](#sadrzaj)]

---

## loop direktiva

- Daje kompajleru dodatne informacije o tome kako da paralelizuje petlju na koju se odnosi direktiva. Može biti unutar `parallel` ili `kernels` direktiva.

```c
  #pragma acc loop [clause-list] new-line
    for loop
```
- Kombinovane konstrukcije:

```c
  #pragma acc parallel loop [clause-list] new-line
    for loop
```

```c
  #pragma acc kernels loop [clause-list] new-line
    for loop
```

---

## loop klauzule
- Klauzule za optimizaciju izvršavanja:
	- `gang` - Particioniše iteracije između `gang`-ova. Prevodi izvršavanje iz `gang-redundant` u `gang-partitioned` režim. U slučaju ugnježdenih petlji, spoljna mora biti `gang` petlja.
	- `worker` - Patricioniše iteracije između `worker`-a. Prevodi izvršavanje iz `worker-single` u `worker-partitioned` režim. U slučaju ugnježdenih petlji, bilo koja unutrašnja petlja, osim one najugnježdenije, može biti `worker` petlja.
	- `vector` - Vektorizacija petlje (SIMD ili SIMT ). U slučaju ugnježdenih petlji, najugnježdenija je vector petlja.
	- `seq` - Petlja će se izvršiti sekvencijalno bez obzira na potencijalne optimizacije koje je kompajler pronašao. U sučaju ugnježdenih petlji, može biti na bilo kom nivou. 
- *Optimizacijom petlji se gubi na portabilnosti programa.*

---

##loop klauzule

- Ostale klauzule:
	- private
	- reduction
	- independent - Signalizira kompajleru da nema zavisnosti podataka između iteracija petlje.
	- ...

---

## Primer 4: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z6-openACC/#table-of-contents"> ☛ Primeri/`parallelloop.c`</a>

```c
int main() {

	/* ... */

	#pragma acc parallel loop gang
	for (int i=0; i<N; i++)
		#pragma acc loop vector
		for (int j=0; j<M; j++)
			A[i * N + j] = 1.f;

	return 0;
}
```

---
layout: false
name: model
class: center, middle, inverse

# Model

---
layout: true
.section[[Model](#sadrzaj)]

---

## Model podataka i utican na na performanse

.lcol[

![:scale 95%](img/modelAcc.png)
]

.rcol[

![](img/modelAcc2.png)

]
<br><br><br><br><br><br><br><br><br><br><br>

- Optimizacija kernela bez optimizacije prenosa podataka obično ne vodi poboljšanju performansi izvršavanja programa!

.footer.medium[
	Izvor: [OpenACC Programming and Best Practices Guide](https://www.openacc.org/sites/default/files/inline-files/OpenACC_Programming_Guide_0.pdf)
] 

---

## Upravljanje podacima

- Prenos podataka između domaćina i uređaja je moguće kontrolisati na više načina:
	- Bez bilo kakvog specificiranja ponašanja, kompajler će sam odrediti kada i koje podatke treba preneti sa domaćina na uređaj ili obrnuto.
	- Modifikovanjem ponašanja `parallel` i `kernels` odgovarajućim klauzulama za rad sa podacima (npr. `copy, copyin, copyout`...). Modifikacije važe za paralelni region nad kojim su primenjene klauzule za modfikaciju.
	- Korišćenjem `data` direktive.
	- Korišćenjem `data enter` i `data exit` direktiva (uglavnom za objektno programiranje).

---

layout: false
name: dd
class: center, middle, inverse

# data direktiva

---
layout: true
.section[[data direktiva](#sadrzaj)]

---

## data direktiva

- Definiše blok koda u kojem se klauzulama kontroliše prenos podataka na relaciji domaćin uređaj.
```c
#pragma acc data [clause-list] new-line
structured block
```
	- copy
	- copyin
	- copyout
	- create
	- present

---

## Primer 5: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z6-openACC/#table-of-contents"> ☛ Primeri/`data.c`</a>

```c
int main() {
	/* ... */
	#pragma acc data
	{
		#pragma acc parallel loop
		for (int i=0; i<N; i++) {
			y[i] = 0.0f;
			x[i] = (float)(i+1);
		}
		#pragma acc parallel loop
		for (int i=0; i<N; i++) {
			y[i] = 2.0f * x[i] + y[i];
		}
	}
	/* ... */
}
```

---

## Klauzule
- `copy(var-list)` - Alocira prostor za promenljive na uređaju, kopira vrednosti sa domaćina na uređaj i sa uređaja nazad na domaćina kada se završi blok podataka (ili `parallel` i `kernels` blok ako klauzula stoji uz njih)
- `copyin(var-list)` - Alocira prostor za promenljive na uređaju i kopira vrednosti sa domaćina na uređaj. Po završetku bloka ne prenosi vrednosti nazad na domaćina.
- `copyout(var-list)` - Alocira prostor za promenljive na uređaju bez inicijalizacije podacima sa domaćina. Po završetku bloka, podaci se prenose sa uređaja na domaćina.
- `create(var-list)` - Alocira prostor za promenljive na uređaju bez inicijalizacije. Vrednosti promenljivih se na kraju bloka ne prebacuju na domaćina.
- `present(var-list)` - Označava da su promenljive prisutne u memoriji uređaja.

---

## OpenACC promenljive okruženja

- GNU GCC kompajler:
	- `ACC_DEVICE_TYPE` (standard)
	- `ACC_DEVICE_NUM` (standard)
	- `ACC_PROF` (standard)
	- `GOPMP_OPENACC_DIM` (gcc)
	- `GOMP_DEBUG` (gcc)

---

layout: false
name: zad
class: center, middle, inverse

# Zadaci

---
layout: true
.section[[Zadaci](#sadrzaj)]

---

## Zadatak 1: Računanje broja π

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Modifikovati dati sekvencijalni program za računanje vrednosti broja π korišćenjem OpenACC direktiva. Koristiti NVIDIA grafičku karticu kao akcelerator (GNU GCC trenutno ne podržava Radeon kartice). 
- Meriti izvršavanje sekvencijalnog i implementiranog ubrzanog programa.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomena**: Zadatak svakako implementirati i u slučaju da na računaru nemate dostupnu NVIDIA grafičku karticu.
]
]

---

## Zadatak 2: Računanje Jakobijana

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Modifikovati dati sekvencijalni program za računanje Jakobijana.
- Osnovnu verziju programa u C++ programskom jeziku skinuti sa
- Githab naloga OpenACCUserGroup.Koristiti NVIDIA grafičku karticu kao akcelerator (GNU GCC trenutno ne podržava Radeon kartice). 
- Meriti izvršavanje sekvencijalnog i implementiranog ubrzanog programa.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomena**: Zadatak svakako implementirati i u slučaju da na računaru nemate dostupnu NVIDIA grafičku karticu.
]
]

---

## Saveti za inkrementalno portovanje sekvencijalnog u OpenACC kod

1. Identifikovati paralelizam petlji. Paralelizovati deo po deo sekvencijalnog koda pri tom kontrolišući korektnost izvršavanja programa. Nastaviti sa ovim tipom optimizacije bez obzira na to da li se vreme izvršavanja povećava.

2. Optimizovati prenos podataka. Preklopiti prenos podataka sa računanjem, ukloniti nepotrebna kopiranja, ažuriranja promenljivih, itd...

3. Paralelizovane petlje optimizovati za ciljnu arhitekturu korišćenjem klauzula.

.footer.medium[
Izvor: [OpenACC Programming and Best Practices Guide](https://www.openacc.org/sites/default/files/inline-files/OpenACC_Programming_Guide_0.pdf)
]

---
layout: false
## Literatura

- Tekstualni materijali:
	- [OpenACC Programming and Best Practices Guide](https://www.openacc.org/sites/default/files/inline-files/OpenACC_Programming_Guide_0.pdf)
	- [OpenACC Specification 2.5](https://www.openacc.org/sites/default/files/inline-files/OpenACC_2pt5.pdf)
	- [GNU GCC OpenACC Wiki](https://gcc.gnu.org/wiki/OpenACC)
	- David B. Kirk, Wen-mei W. Hwu, Programming Massively Parallel Processors, A Hands on Approach, 2nd edition, 2012
	- [Advanced OpenACC](http://videolectures.net/site/normal_dl/tag=1058329/ihpcss2016_urbanic_advanced_openACC_01.pdf)
- Video tutorijali:
	- [Introduction to Parallel Programming with OpenACC](https://www.youtube.com/playlist?list=PLx_s9Cz7_T429SF7gBGJ51iiZoEWYVvkq)
	- [Advanced OpenACC](http://videolectures.net/ihpcss2016_urbanic_advanced_openACC/)

--

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}