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

- [Veljko Petrović, Ph.D., Assistant Professor](http://www.acs.uns.ac.rs/en/user/30)
- [Gorana Gojić, MS, Teaching assistant](http://www.acs.uns.ac.rs/en/user/49)
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

- [Posebne biblioteke](#bib)
- [Linearna algebra](#alg)
- [Ostale primene](#ostalo)

---
name: bib
class: center, middle, inverse

## Posebne biblioteke
## potporni softver za performantne naučno računarstvo

---
layout: true

.section[[Posebne biblioteke](#sadrzaj)]

---

## Zašto?

- Zašto koristimo biblioteke inače? Neko ko se samo bavi metodama za, npr. linearnu algebru može da ih implementira mnogo bolje od nas kojima je ta linearna algebra (potencijalno) samo jedna od deset stvari koje nam trebaju.

- Ovo je, u stvari, naročito dobar primer za upotrebu biblioteka zato što vrlo često mi imamo jasnu podelu između našeg algoritma i raznih matematičkih operacija i algoritama koji nam trebaju da ga implementiramo. Integracija, uz tako jasnu podelu, je mnogo lakša.

- Često ovo ide do tačke da se koristi poseban jezika (`Python`, često) ali mi nećemo nužno ići tako daleko.

---

## Izbor iz oblasti primene
 
- Linearna algebra
- Parcijalne diferencijalne jednačine
- Grafovi
- Paralelizovani I/O
- Dekompozicija meš struktura
- Vizuelizacija

---

## Struktura predavanja
 
- Prelazimo prvo najizraženiji deo HPC-a: linearnu algebru.
- U okviru HPC-a BLAS i Lapack se analiziraju u malo više detalja kao demonstracija kako izgledaju biblioteke tog tipa.
- Za ostale oblasti primene, samo se informativno prelaze koje biblioteke postoje i, konsekventno, odakle treba početi.

---
name: alg
layout: false
class: center, middle, inverse

# Linearna algebra

---

layout: true

.section[[Linearna algebra](#sadrzaj)]

---

## `BLAS`

- *Basic Linear Algebra Subprograms*
- `BLAS` je jedna od najdrevnijih biblioteka koje se još koriste.
- Nastala je kao deo rada u `JPL` ogranku `NASA`.
- `BLAS` je serijski i sastoji se od implementacija bazičnih formi vektorsko/matričnih operacija na način koji je jako optimizovan za različite platforme.

---

## Nivoi

.lcol[

- Sve `BLAS` operacije se mogu podeliti na 3 nivoa po svojoj prirodi:
	1. Vektor-vektor
	2. Matrica-vektor
	3. Matrica-matrica
- Ranije verzije su podržavale samo niže nivoe zbog ograničenja računara iz tog doba.
]

.rcol[

<br><br>

.center-table.medium[

| Nivo | Tipizujuća jednačina |
|:----:|:--------------------:|
|   1  |      𝒚 = 𝛼𝒙 + 𝒚       |
|   2  |     𝒚 = 𝛼𝐴𝒙 + 𝛽𝒚      |
|   3  |     𝐶 = 𝛼𝐴𝐵 + 𝛽𝐶     |

]

]

---

## Implementacija
 

- `BLAS` je napisan u programskom jeziku `Fortran 77`.
- Postoji varijanta prilagođena za upotrebu u programskom jeziku `C` koja se zove `CBLAS`
- Nju koristimo (danas, nju skoro svi koriste, kad koriste `BLAS`) te ako negde piše `BLAS` gotovo se sigurno misli na `CBLAS` biblioteku.

---

## Struktura naziva rutina

- Funkcije u `BLAS`-u imaju imena standardizovanog oblika koje, preko afiksa, određuju:
	- Preciznost
	- Tip matrice koji se koristi, ako ga ima.
	- Tip operacije.
- U skladu sa `c` konvencijom, postoji i prefiks biblioteke, `cblas_`

---

## Preciznost u `BLAS`-u

.center-table.medium[

| Prefiks |                  Opis                 |
|:-------:|:-------------------------------------:|
|    s    |                 Float                 |
|    d    |                 Double                |
|    c    |    Complex (dva float-a, 8 bajtova)   |
|    z    | D. Complex (dva double-a, 16 bajtova) |

]

---

## Tip matrice u `BLAS`-u

.center-table.medium[

| Prefiks |                                                        Opis                                                       |
|:-------:|:-----------------------------------------------------------------------------------------------------------------:|
|    ge   |                                          Opšta matrica: nema ograničenja.                                         |
|    sy   |                         Simetrična matrica. Matrica koja je jednaka svojoj transpoziciji.                         |
|    he   | Hermitijanska matrica. Matrica u kojoj element-parovi tokom transponovanja su jedni drugima kompleksni konjugati. |
|    tr   |                Trouglasta matrica gde su sve vrednosti ili iznad ili ispod glavne dijagonale nula.                |

]

---
## Tip čuvanja matrice u `BLAS`-u
 
- Matrice se podrazumevano čuvaju u `dense` formatu, odnosno, jednostavno se pamte sve vrednosti matrice. Redom.

- Ali, može se drugo slovo tipa matrice zameniti sa ili b ili p da odredi da je matrica drugog tipa.

---
## Tip čuvanja matrice u `BLAS`-u

.center-table.medium[

| Prefiks |                                                                                                                  Opis                                                                                                                 |
|:-------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|    b    |                                                            Trakasta (*banded*) matrica. Matrica u kojoj je većina vrednosti 0, ali one koje nisu su u dijagonalama (trakama).                                                           |
|    p    | Nabijena (*packed*) matrica. Ako je matrica takva da se od jednog trougla može rekonstruisati cela matrica (simetrična, Hermitijanska, ili trouglasta) može se čuvati samo taj jedan trougao pročitan po kolonama u jednom velikom nizu |

]

---

## `BLAS`, nivo 1, operacije bez skalarnog proizvoda

.center-table.medium[

|   Ime  |             Opis            | Nivoi preciznosti |
|:------:|:---------------------------:|:-----------------:|
| `swap` |    Menja mesta vektorima    |      s,d,c,z      |
| `scal` | Skalira vektor za konstantu |   s,d,c,z,cs,zd   |
| `copy` |        Kopira vektor        |      s,d,c,z      |
| `axpy` |     Izvršava 𝒚 = 𝛼𝒙 + 𝒚     |      s,d,c,z      |

]

---

## `BLAS`, nivo 1, operacije sa skalarnim proizvodom

.center-table.medium[

|    Ime   |                                              Opis                                              | Nivoi preciznosti |
|:--------:|:----------------------------------------------------------------------------------------------:|:-----------------:|
|  `swap`  |                                        Skalarni proizvod                                       |       s,d,ds      |
|  `dotc`  | Skalarni proizvod konjugovane vrednosti kompleksne vrednosti i neke druge kompleksne vrednosti |        c,z        |
|  `dotu`  |                                  Kompleksni skalarni proizvod                                  |        c,z        |
| `sdsdot` |                                  Skalarni proizvod plus skalar                                 |        sds        |

]

---

## `BLAS`, nivo 1, operacije norme

.center-table.medium[

| **Ime** |         **Opis**        | **Nivoi preciznosti** |
|:-------:|:-----------------------:|:---------------------:|
| `nrm2`  | Proračun 2-norme.ref[1] |       s,d,sc,dz       |
| `asum`  | Proračun 1-norme.ref[2] |       s,d,sc,dz       |
|`i_amax` | Proračun ∞-norme.ref[3] |        s,d,c,z        |

]

<p>
    $$ 1. \quad  ||x||_{2}=\sqrt{\sum|x_{i}|^{2}}$$
    $$ 2. \quad  ||x||_{1}=\sqrt{\sum|x_{i}|} $$
    $$ 3. \quad  ||x||_{∞}=max(|x_{i}|) $$
</p>

---

## `BLAS`, nivo 1, operacije rotacije

.center-table.small[

| **Ime** |                                                                     **Opis**                                                                     | **Nivoi preciznosti** |
|:-------:|:------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------:|
|  `rotg` |                          Uz date skalare a i b, sračunaće c i s takve da .ref[1] <br>Ovo se zovu paramteri Givensove rotacije                         |          s,d          |
|  `rot`  | Primenjuje Givens rotaciju, drugim rečima, ako joj se daju dva vektora kao ulaz, svaki element vektora se transformiše na sledeći način .ref[2] |          s,d          |

]

<p>
    $$ 1. \quad  \left(\begin{array}{c}c  \quad  s\\ -s  \quad c\end{array}\right)\left(\begin{array}{c}a\\ b\end{array}\right)=\left(\begin{array}{c}\sqrt{|a|^{2}+|b|^{2}}\\ 0\end{array}\right) $$
    $$ 2. \quad  x_{i}=cx_{i}+sy_{i} \\  \quad y_{i}=-sx_{i}+cy_{i} $$
</p>

---

## `BLAS`, nivo 1, operacije rotacije

.center-table.small[

| **Ime** |                                                                                      **Opis**                                                                                      | **Nivoi preciznosti** |
|:-------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------:|
| `rotmg` |  roračuna modifikovanu Givensovu rotacionu matricu. Ako su dati skalirajući faktori `d1` i `d2` i koordinate `(x1, y1)` ulaznog vektora sračunati `2x2` matricu H takvu da .ref[1] |          s,d          |
|  `rotm` |                                                                     Za vektore `x` i `y` proračuna .ref[2]                                                                        |          s,d          |
]

<p>
    $$ 1. \quad  \left(\begin{array}{c}x_{1}\\ 0\end{array}\right)=H\left(\begin{array}{c}x_{1}\sqrt{d_{1}}\\ y_{1}\sqrt{d_{2}}\end{array}\right) $$
    $$ 2. \quad  \left(\begin{array}{c}x_{i}\\ y_{i}\end{array}\right)=H\left(\begin{array}{c}x_{i}\\ y_{i}\end{array}\right) $$
</p>

---

## `BLAS`, nivo 2 i 3, operacije

.center-table.small[

| **Ime** |                                      **Opis**                                     |
|:-------:|:---------------------------------------------------------------------------------:|
|   `mv`  |                            Prozivod matrice i vektora.                            |
|   `sv`  | Rešava matricu (trouglastu), tj. sistem linearnih jednačina koji ona predstavlja. |
|   `mm`  |                      Proizvod dve matrice uz učešće skalara.                      |
|   `rk`  |                           Računa opštu jednačinu .ref[1]                          |
|  `r2k`  |                           Računa opštu jednačinu .ref[2]                          |
]

<p>
    $$ 1. \quad  C=\alpha AA^{T}+\beta C $$
    $$ 2. \quad  C=\alpha AB^{T}+\overline{\alpha}BA^{T}+\beta C $$
</p>

---

## Primer, prost
 
- Setite se, jako davno, kada smo radili benchmarking `HPC` sistema.
- Pominjao sam da postoji jako često korišćen `DGEMM` korak benchmark-a.
- `D-double` preciznost
- `GE`-opšta matrica
- `MM`-množenje
- Funkcija bi bila `cblas_dgemm`

---
## cblas_dgemm

.medium[
```c
void clbas_dgem(
    const enum CBLAS_ORDER order,
    //matrice u redovi-prvo ili kolone prvo formi
    //CblasRowMajor ili CblasColMajor
    const enum CBLAS_TRANSPOSE transB,
    //Da li matricu A treba transponovati
    //CblasNoTrans, CblasTrans, CblasConjTrans
    const int M,
    //Broj redova uA iC
    const int N,
    //Broj kolona u B iC
    const int K,
    //Broj kolona u matrici A i broj redova u matrici B
    const double alpha,
    //Skalirajuci faktor za A*B
    const double *A,
    //Pokazivac na podatke za A
    const int lda,
    //Velicina prve dimenzije matrice A
    const double *B,
    //Pokazivac na podatke za B
    const int ldb,
    //Velicina prve dimenzije matrice B
    const double beta,
    //skalirajuci faktor za C
    double *C,
    //Pokazivac na podatke za C
    const int ldc
    //Velicina prve dimenzije za C
)
```
]

---

## Upotreba (1/3)

```c
#include <stdio.h>
#include <stdlib.h>
#include <cblas.h>

int main()
{
    double *A, *B, *C;
    int m = 3; // square matrix, number of rows and columns
    int i,j;

    A = (double *) malloc(m*m*sizeof(double));
    B = (double *) malloc(m*m*sizeof(double));
    C = (double *) malloc(m*m*sizeof(double));

    // initialize the matrices
    for (i=0;i<m;i++) {
        for (j=0;j<m;j++) {
            A[j + m*i] = j + m*i; // arbitrarily initialized
            B[j + m*i] = 3.14*(j + m*i);
            C[j + m*i] = 0.0;
        }
    }
...
```

.footer.medium[
  [High Performance Computing: Modern Systems and Practices no. 323 ](https://books.google.si/books?id=qOHIBAAAQBAJ&printsec=frontcover&source=gbs_ge_summary_r&cad=0#v=onepage&q&f=false)

] 

---

## Upotreba (2/3)

```c
...
    double alpha = 1.0;
    double beta = 0.0;

    cblas_dgemm(CblasRowMajor, CblasNoTrans, CblasNoTrans, m, m, m, alpha, A, m, B, m, beta, C, m);

    for (i=0;i<m;i++) {
        for (j=0;j<m;j++) {
            printf(" A[%d][%d]=%g ",i,j,A[j+m*i]);
        }
        printf("\n");
    }

    for (i=0;i<m;i++) {
        for (j=0;j<m;j++) {
            printf(" B[%d][%d]=%g ",i,j,B[j+m*i]);
        }
        printf("\n");
    }
...

```
.footer.medium[
  [High Performance Computing: Modern Systems and Practices no. 323 ](https://books.google.si/books?id=qOHIBAAAQBAJ&printsec=frontcover&source=gbs_ge_summary_r&cad=0#v=onepage&q&f=false)

] 

---

## Upotreba (3/3)

```c
    for (i=0;i<m;i++) {
        for (j=0;j<m;j++) {
            printf(" C[%d][%d]=%g ",i,j,C[j+m*i]);
        }
        printf("\n");
    }

    free(A);
    free(B);
    free(C);

    return 0;
}
```
.footer.medium[
  [High Performance Computing: Modern Systems and Practices no. 323 ](https://books.google.si/books?id=qOHIBAAAQBAJ&printsec=frontcover&source=gbs_ge_summary_r&cad=0#v=onepage&q&f=false)

] 

---

## `BLAS` primena
 
- `BLAS` se intenzivno koristi u pozadini velikog broja paketa baziranih na linearnoj algebri.
- Direktno se i ne koristi puno, ne više.
- Korisno je savladati kako radi zato što se koncepti iz BLAS sveta pojavljuju na dosta mesta.

---

## Lapack
 
- *Linear Algebra PACKage*.
- Naslanja se na `BLAS` i zamenjuje stari `Linpack`.
- Lapack je implementiran u `Fortran`-u.
- Verzija za `C` (koja je deo `Lapack` projekta) je `Lapacke`.
- Sve rutine počinju sa `LAPACKE_` prefiksom koji prati kod koji se sastoji od:
	- Tipa podataka (kao u `BLAS`)
	- Tipa matrice (kao u `BLAS` uz par dodatnih)
	- Operacije

---

## `LAPACK` operacije

.center-table.small[

|        Ime        |                                                                Opis                                                               |
|:-----------------:|:---------------------------------------------------------------------------------------------------------------------------------:|
|         SV        |                                                Rešava sisteme linearnih jednačina.                                                |
| LS, LSY, LSS, LSD | Rešava problem najmanjih kvadrata, minimizuje x u formuli ravnoj drugoj normi razlike između vektora b i matrice A puta vektor x. |
|        LSE        |                                           Kao gore, ali sa ograničenjem da Bx jednako d.                                          |
|        GLM        |    Rešenje problema opšteg linearnog modela, tj. minimiziranje  x bazirano na drugoj normi vektora y sa ograničenjem𝑑 = 𝐴𝑥 + 𝐵𝑦   |
|    EV, EVD,EVR    |                              Računanje sopstvene vrednosti i sopstvenih vektora simetrične matrice A                              |

]

---

## `LAPACK` operacije

.center-table.medium[

|   Ime   |                              Opis                             |
|:-------:|:-------------------------------------------------------------:|
|    ES   | Traši sopstvene vrednosti i vektore za nesimetrične matrice A |
| SVD,SDD |                     Dekompozicija matrice.                    |

]

---

## Primer `LAPACK` funkcije

.medium[

```c
    lapack_int LAPACKE_dgesv (
    
        int matrix_layout,
        
        //LAPACK_ROW_MAJOR ili LAPACK_COL_MAJOR
        lapack_int n,

        //broj jednacina
        lapack_int nrhs,

        //broj vrednosti sa desne strane jednacine
        double* a,

        //nxn matrica A
        lapack_int lda,

        //prva dimenzija niza a
        lapack_int* ipiv,

        //niz duzine n u koji ce biti spremljena
        //permutacija matrice tokom faktorizacije
        double* b,

        //nxnhrs matrica desnih strana, kasnije tu budu resenja
        lapack_int ldb

        //prva dimenzija niza b
);
```
]

---
## Upotreba (1/2)

```c
#include <stdio.h>
#include <lapacke.h>

int main (int argc, const char * argv[])
{
    double A[3][3] = {1,3,2,4,1,9,5,7,2};
    double b[3] = {-1,-1,1};

    lapack_int ipiv[3];
    lapack_int info,m,lda,ldb,nrhs;

    int i,j;
    m = 3;
    nrhs = 1;
    lda = 3;
    ldb = 1;

```
.footer.medium[
  [High Performance Computing: Modern Systems and Practices no. 325 ](https://books.google.si/books?id=qOHIBAAAQBAJ&printsec=frontcover&source=gbs_ge_summary_r&cad=0#v=onepage&q&f=false)

] 

---

## Upotreba (2/2)

```c
    // Solve the linear system
    info = LAPACKE_dgesv(LAPACK_ROW_MAJOR,m,nrhs,*A,lda,ipiv,b,ldb);

    // check for singularity
    if (info > 0 ) {
        printf(" U(%d,%d) is zero! A is singular\n",info,info);
        return 0;
    }

    // print the answer
    for (i=0;i<m;i++) {
        printf(" b[%i] = %g\n",i,b[i]);
    }

    printf( "\n" );
    
    return 0;
}

```
.footer.medium[
  [High Performance Computing: Modern Systems and Practices no. 325 ](https://books.google.si/books?id=qOHIBAAAQBAJ&printsec=frontcover&source=gbs_ge_summary_r&cad=0#v=onepage&q&f=false)

] 

---
## Skalabilnost `BLAS/LAPACK`
 
- Podrazumevano, sve rutine u `BLAS/LAPACK` su serijske.
- Rešenje? 
- Postoje `HPC`-optimizivovane verzije koje rade iste stvari, ali imaju `MPI` sposobnosti:
	- `ScaLapack`
	- `PBLAS`

---

## Alternative

- *GNU Scientific Library (GSL)*
	- Izuzetno široka biblioteka.
	- Ima ponešto za svaku temu.
	- Za nas je najzanimljivije što ima `BLAS` interfejs koji čini bazične operacije sa matricama nešto manje mučnim.
- `SuperLU`
	- `Supernodal LU`
	- Fokusira se na rešavanje ogromih sistema jednačina.
	-Ima izuzetnu podršku za ubrzanje kroz `OpenMP`, `MPI`, i čak `GPU` ubrzanje

---

## Alternative

- `PETSc`
	- *Portable extensible toolkit for scientific computation*
	- Uglavnom namenjen za rešavanje diferencijalnih jednačina, ali kao deo posla rešavanja sistema parcijalnih diferencijalnih jednačina ima odlične sisteme za rešavanje običnih linearnih jednačina.
	- Poseduje `HPC` podršku direktno u samoj biblioteci.
	- Koristi `MPI`
	- Intenzivno se koristi u `HPC` krugovima
- `SLEPc`
	- *Scalable Library for Eigenvalue Problem Computations*
	- Proširenje `PETS`c koje računa i probleme sopstvenih vrednosti i vektora

---
## Alternative

- `ELPA`
	- *Eigenvalue Solvers for Petaflop-Applications*
	- Još jedan način da se na `HPC`-optimizovan način računaju sopstvene vrednosti i vektori.
	- Ova varijanta je naročito popularna u kvantnoj hemiji i nauci materijala zbog svoje sposobnosti da brzo računa sopstvene vektore Hermitijanskih matrica.
- `HYPRE`
	- Kombinuje se sa `PETS`c i proširuje njene sposobnosti da rešava sisteme linearnih jednačina.
	- Koristi `MPI`

---

name: ostalo
layout: false
class: center, middle, inverse

# Ostale primene

---

layout: true

.section[[Ostale primene](#sadrzaj)]

---

## Parcijalne diferencijalne jednačine

- `PETS`c
	- Već je pomenut.
	- Specijalizovan je za baš diferencijalne jednačine.
	- Podržava i rešavanje nelinearnih jednačina.
- Trilinos projekat
	- Veliki sistem za brze naučne proračune napravljen da radi na velikom broju `HPC` arhitektura.

---

## Grafovi

- `PBGL`
	- *Parallel Boost Graph Library*
	- Bazira se na `Boost`-ovoj biblioteci za grafove ali je proširuje da radi u `HPC` okruženju.
	- Radi dobro sa arhitekturama distribuirane memorije.
- *Combinatorial BLAS*
	- Oslanja se na i formiran je po ugledu na `BLAS`
	- Takođe cilja na arhitekture sa distribuiranom memorijom
- `Giraph`

---
## I/O

- `NetCDF`
	- Format razvijen za geologiju, geografiju, i geofiziku.
	- Karakteriše ga odlična podrška za arhiviranje.
- `HDF5`
	- Sa ovim, kako čujem, imate dosta iskustva.
- `Silo`
	- Biblioteka višeg nivoa koja se oslanja na HDF5 da olakša unos iz ispisivanje podataka u distribuiranim aplikacijama.

---

## Dekompozicija meš struktura

- `METIS`
	- Tretira meš kao graf odn. hipergraf i dekomponuje na osnovu toga.
	- Serijalna je, ali ima `HPC`-optimizovanu verziju: `ParMETIS` koja koristi `MPI`
- `Trilinos`
	- `Trilinos` podržava i ovu funkcionlnost.

---

## Vizuelizacija
 
- O ovome, celo predavanje, ali najbitnija biblioteka u ovoj oblasti jeste *VTK: Visualization Toolkit*.

--

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}
