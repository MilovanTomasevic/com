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
- [OpenACC i CUDA](#cuda)

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

<br><br><br><br><br><br><br><br><br>
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

## Primer 2: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z7-openACC/#table-of-contents"> ☛ Primeri/`ptraliasing.c`</a>

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

## Primer 3: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z7-openACC/#table-of-contents"> ☛ Primeri/`parallel.c`</a>

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

## Primer 4: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z7-openACC/#table-of-contents"> ☛ Primeri/`parallelloop.c`</a>

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

## Primer 5: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z7-openACC/#table-of-contents"> ☛ Primeri/`data.c`</a>

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
name: cuda
class: center, middle, inverse

# OpenACC i CUDA

---
layout: true
.section[[OpenACC i CUDA](#sadrzaj)]

---
## CUDA ponavljanje

- `CUDA` (eng. *Compute Unified Device Architecture*) - platrofma za paralelno programiranje na NVIDIA grafičkim karticama.
- Mapiranje OpenACC na CUDA termine:

|            **OpenACC**           |           **CUDA**          |
|:--------------------------------:|:---------------------------:|
|       Domaćin (eng. *Host*)      |    Domaćin (eng. *Host*)    |
| Akcelerator (eng. *Accelerator*) |    Uređaj (eng. *Device*)   |
|   Paralelni ili računski region  | Jedan ili više CUDA kernela |

- U OpenACC kod je na višem nivou abstrakcije u odnosu na CUDA kod i može se prevesti za izvršavanje na GPU.

---

## Arhitektura CUDA grafičke kartice

- Streaming Multiprocessors (SM) & Kuda jezgra
- Veliki broj hardverskih niti čini grafičke kartice pogodnim za probleme za izraženim SIMD paralelizmom.

.lcol[

![:scale 75%](img/cudaM.png)
]

.rcol[

![:scale 65%](img/cuda.jpg)
]

---

## Memorijska hijerarhija CUDA grafičke kartice

![:scale 55%](img/cudaMH.png)

- **Globalna memorija, L2 keš** - mogu da joj pristupe svi SM.
- **Deljena memorija, L1 keš** - mogu joj pristupiti sva jezgra unutar jednog SM.
- **Registri** - pristup na nivou jezgra.
- Konstantna memorija, memorija tekstura

---

## CUDA model izvršavanja

.lcol[

![:scale 85%](img/cudaMI.png)

]

.rcol[

- Nit
- Blok niti (eng. *thread block*) sadrži jednu ili više niti
- Osnova (eng. *warp*) - sadrži jednu ili više niti iz istog bloka, blok je podeljen na jednu ili više osnova
- Mreža niti (eng. *grid*) sadrži jedan ili više blokova niti

]

---

## OpenACC CUDA mapiranja

.center[

| **CUDA logički** | **OpenACC logički** | **CUDA fizički nivo** |
|:----------------:|:-------------------:|:---------------------:|
|    Mreža niti    |    više gang -ova   |   svi SM na kartici   |
|     Blok niti    |         gang        |           SM          |
|       warp       |        worker       |           SM          |
|     CUDA nit     |        vector       |      CUDA jezgro      |

]

- Mapiranje pojmova CUDA modela izvršavanja (prva kolona) na OpenACC model izvršavanja (druga kolona) i na fizičke komponente GPU koja podržava CUDA tehnologiju.

---

## Tehnike za ubrzavanje CUDA programa

- Preporuke za ubrzanje izvršavanja CUDA programa:
	- Ukoliko je moguće, CUDA niti treba da pristupaju uzastopnim lokacijama globalne memorije (eng. *coalesced access*).
	- Izbeći divergenciju izvršavanja niti unutar jedne osnove.
	- Podatke koji se često koriste prebaciti u bržu memoriju u odnosu na globalnu (`cache direktiva`).
	- Optimizovati prenos podataka u/iz globalne memorije grafičke kartice (`data direktiva`).
	- ... 

--

.message.is-warning[
.message-header[
Info
]
.message-body[
- **Preporučeno čitanje**: Programming Massively Parallel Processors: A Hands-on Approach, David B. Kirk and Wen-mei W. Hwu, poglavlje 6 Performance Considerations
]
]

---

## Primer 6: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z7-openACC/#table-of-contents"> ☛ Primeri/`matrixop.c`</a>

```c
int main(int argc, char *argv[]) {
	int *randomMatrix =
	(int *) calloc(MSIZE * MSIZE * sizeof(int));

	#pragma acc kernels
	{
		for(i = 0; i < MSIZE; i++) {
			for(j = 0; j < MSIZE; j++) {
				randomMatrix[j * MSIZE + i] =
				randomMatrix[j * MSIZE + i] + 2;
			}
		}
	}
}
```

---

## Primer 7: <br><a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z7-openACC/#table-of-contents"> ☛ Primeri/`matrixop-coalesced.c`</a>

```c
int main(int argc, char *argv[]) {
	int *randomMatrix =
	(int *) calloc(MSIZE * MSIZE * sizeof(int));

	#pragma acc kernels
	{
		for(int i = 0; i < MSIZE; i++) {
			for(int j = 0; j < MSIZE; j++) {
				randomMatrix[i * MSIZE + j] =
				randomMatrix[i * MSIZE + j] + 2;
			}
		}
	}
}
```

---

## Pristup elementima matrice

- Pristup lokacijama koje nisu uzastopne

![:scale 65%](img/m1.png)

- Pristup uzastopnim lokacijama

![:scale 65%](img/m2.png)

.attention[
**Pretpostavka**: Svaki element matrice obrađuje jedna CUDA nit.
]

---

## matrixop.c Visual Profiler

![:scale 75%](img/vp.png)
---

## matrixop-coalesced.c Visual Profiler

![:scale 75%](img/vp2.png)

---

## Divergencija pri izvršavanju niti

- Ukoliko različite niti unutar iste osnove imaju različite tragove izvršavanja, izvršavanje grupa niti sa različitim tragovima unutar osnove se sekvencijalizuje.
- Do divergencije u izvršavanju mogu dovesti naredbe za kontrolu toka izvršavanja `if-else, switch, do, for, while`.

--

.message.is-warning[
.message-header[
Info
]
.message-body[
- **Videti**: CUDA C Best Practices Guide - Branching and Divergence.
]
]

---

## Neki radovi na temu OpenACC, CUDA i OpenCL

- [A Comprehensive Performance Comparison of CUDA and OpenCL](https://ieeexplore.ieee.org/document/6047190)
- [CUDA vs OpenACC: Performance Case Studies with Kernel Benchmarks and a Memory-Bound CFD Application](https://ieeexplore.ieee.org/document/6546071)
- [An in-depth evaluation of GCC’s OpenACC implementation on Cray systems](https://cug.org/proceedings/cug2017_proceedings/includes/files/pap174s2-file1.pdf)
- [OpenACC cache Directive: Opportunities and Optimizations](https://ieeexplore.ieee.org/document/7836580)

---

## Zadatak 3: Množenje matrica

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Implementirati OpenACC C program za množenje dve kvadratne matrice. 
- Kao ulazne matrice koristiti matrice u hdf5 formatu sa prethodnih vežbi.
]
]

--
.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**:
- Sačuvati rezultat množenja matrica 3x3 ili rezultat ispisati na konzolu.
- Meriti vreme izvršavanja programa i ispisati ga na standardni izlaz (za primer merenja pogledati matrixop.c datoteku u primerima).
- Opciono uporediti vremena izvršavanja implementiranog OpenACC rešenja prevedenog za izvršavanje na GPU sa ekvivalentnim CUDA rešenjima (direktorijum Samples u CudaToolkit-u ima implementiran primer množenja dve matrice bez i sa korišćenjem optimizovane cublas biblioteke (direktorijumi cudaMatMul i cudaMatMulCUBLAS).
]
]

---

## Zadatak 3: Množenje matrica - kompajliranje i pokretanje zadatka

.message.is-warning[
.message-header[
Info
]
.message-body[
- **GNU GCC 7**
- Za prevođenje OpenACC koda za izvršavanje na domaćinu:
```console
gcc -fopenacc <izvorna_datoteka>
```
- Za prevođenje OpenACC koda za izvršavanje na NVIDIA GPU:
```console
gcc -fopenacc -foffload=nvptx-none <izvorna_datoteka>
```
- Da bi ovo radilo, neophodno je prvo instalirati paket
`gcc-7-offload-nvptx` *iz repzitorijuma*.

- Pokretanje programa: `./<izvrsna_datoteka>`
- Svi programi koji se izvršavaju na NVIDIA GPU kartici se mogu pratiti pozivom alata `nvidia-smi -l 1`, što će osvežavati listu procesa koji koriste GPU na svaku sekundu.
]
]

---

## Zadatak 3: Množenje matrica - kompajliranje i pokretanje zadatka

.message.is-warning[
.message-header[
Info
]
.message-body[
- **PGCC 18.10**
- Za prevođenje OpenACC koda za izvršavanje na domaćinu:
```console
pgcc -acc -ta=host <izvorna_datoteka>
```
- Za prevođenje OpenACC koda za izvršavanje na NVIDIA GPU:
```console
pgcc -acc -ta=tesla -Minfo <izvorna_datoteka>.
```
- Pokretanje programa: `./<izvrsna_datoteka>`

- Svi programi koji se izvršavaju na NVIDIA GPU kartici se mogu pratiti pozivom alata nvidia-smi -l 1, što će osvežavati listu procesa koji koriste GPU na svaku sekundu.
]
]

---
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