{% extends "base_slides.md" %}
{% block slides %}
name: sadrzaj
# Sadržaj
- [MPI](#mpi)
- [Komunikatori](#komunikatori)
- [Point to Point komunikacija](#p2p)
- [Kolektivna komunikacija](#kk)

---
name: mpi
class: center, middle

# MPI
---
layout: true

.section[[MPI](#sadrzaj)]
---
## Message Passing Interface (MPI)

- Standard koji propisuje komunikaciju razmenom poruka na različitim paralelnim arhitekturama.
- **MPI 1.x** (1994.) podrška za C, C++, Fortran
- **MPI 2.x** (1997.) podrška za C, C++, Fortran
- **MPI 3.x** (2012.) podrška za C, Fortran
- Od formulisanja standarda razvijene su različite implementacije MPI (komercijalne i otvorenog koda).

---
## MPI ciljna arhitektura
![:scale 80%](img/mpi.png)
--
class: center, middle, overlay, noselect, con

# aaa
---
## Format OpenMPI programa
```c
#include "mpi.h"

	int main(int argc, char *argv[]) {
	MPI_Init(&argc, &argv);

	// MPI code	
	MPI_Finalize();
	return 0;
}
```

---
## Kompajliranje MPI programa
- Pozicionirati se u direktorijum u kojem se nalazi izvorni kod MPI programa i uneti:
```console
mpicc <izvorna_datoteka>
```
- `mpicc` je omotačka skripta za `gcc`, pa se pri kompajliranju mogu navesti opcije gcc kompajlera.

- Pokretanje:
```console
mpiexec [-np <N>] <izvrsna_datoteka>
```
- `-np <N>` - opcija za zadavanje broja procesa koji će biti kreirani pri pokretanju programa.

---
## Primer 1: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/hello_world.c</a>
.message.is-dark[
.message-header[
Primer
]
.message-body[
```c
#include <stdio.h>
#include "mpi.h"

int main(int argc, char *argv[]) {
	int size, rank;

	MPI_Init(&argc, &argv);
	MPI_Comm_size(MPI_COMM_WORLD, &size);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);

	printf("Hello World iz %d/%d.\n", rank, size);
	
	MPI_Finalize();

	return 0;
}
```
]
]

---
## MPI osnovni koncepti
- Komunikator (eng. *communicator* )
- `Point to Point` komunikacija (eng. *Point-to-Point communication*)
- Kolektivna komunikacija (eng. *Collective communication*)

---
layout: false
name: komunikatori
class: center, middle

# Komunikatori
---
layout: true

.section[[Komunikatori](#sadrzaj)]
---
## Komunikatori
- Logički gledano, **komunikator** predstavlja grupu procesa unutar koje
svaki proces ima svoj **rank** (odnosno identifikator).

![:scale 80%](img/komunikator.png)
- `MPI_COMM_SELF, MPI_COMM_NULL`
---
## Komunikatori
- Tokom izvršavanja MPI istovremeno može da postoji više komunikatora.

- `MPI_Comm` tip podatka.
- Funkcije za pravljenje komunikatora:
```c
# kreiranje novog komunikatora deljenjem postojećeg
MPI_Comm_split(MPI_Comm comm, int color, int key, MPI_Comm* newcomm);

# novi komunikator je kopija postojećeg
int MPI_Comm_dup(MPI_Comm comm, MPI_Comm *newcomm)

# i druge
```

---
## Zadatak 1: Komunikatori
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napraviti OpenMPI C program koji korišćenjem funkcije `MPI_Comm_split` na osnovu podrazumevanog pravi dva nova komunikatora. Procese podeliti u dva komunikatora na osnovu parnosti ranka unutar `MPI_COMM_WORLD` komunikatora. Pri tom relativni poredak procesa unutar komunikatora treba da bude isti kao i u podrazumevanom komunikatoru. Svaki proces na standardni izlaz treba da ispiše svoj rank unutar `MPI_COMM_WORLD` i novoformiranog komunikatora.
]
]

.message.is-dark[
.message-header[
Primer
]
.message-body[
- Primer ispisa za jedan proces:
```console
MPI_COMM_WORLD rank: 0/4 - ncomm rank: 0/2
```
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z3-openMPI/#table-of-contents"> ☛ Rešenja/communicators.c</a>
]
]

---
## MPI tipovi podataka
- Zarad portabilnosti MPI standard definiše tipove podataka.

.center-table.small[
|   MPI tip podatka  |    C tip podatka   |
|:------------------:|:------------------:|
|      MPI_CHAR      |     signed char    |
|      MPI_SHORT     |  signed short int  |
|       MPI_INT      |     signed int     |
|      MPI_LONG      |   signed long int  |
|  MPI_UNSIGNED_CHAR |    unsigned char   |
| MPI_UNSIGNED_SHORT | unsigned short int |
|    MPI_UNSIGNED    |    unsigned int    |
|  MPI_UNSIGNED_LONG |  unsigned long int |
|      MPI_FLOAT     |        float       |
|     MPI_DOUBLE     |       double       |
|   MPI_LONG_DOUBLE  |     long double    |
|      MPI_BYTE      |         /          |
|     MPI_PACKED     |         /          |

]
---
layout: false
name: p2p
class: center, middle

# Point to Point komunikacija
---
layout: true

.section[[Point to Point komunikacija](#sadrzaj)]
---
## Point to Point komunikacija
- Komunikacija dva procesa. Jedan proces šalje poruku, drugi proces prima poruku.
- Funkcije za razmenu poruka.ref[1,2]:

.lcol.medium[
```c
int MPI_Recv(
	void *buf,
	int count,
	MPI_Datatype datatype,
	int source,
	int tag,
	MPI_Comm comm,
	MPI_Status *status);
```
]

.rcol.medium[
```c
int MPI_Send(
	const void *buf,
	int count,
	MPI_Datatype datatype,
	int dest,
	int tag,
	MPI_Comm comm);
```
]
![:scale 45%](img/p2p.gif)

.footer.medium[
   <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Recv.3.php">1. OpenMPI 2.0 MPI_Recv docs</a><br>
   <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Send.3.php">2. OpenMPI 2.0 MPI_Send docs</a>

] 
---
## Primer 2: Point to Point komunikacija <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> <br>☛ Primeri/send_recv.c</a>
```c
// ...
if (rank == 0) {

	int message = 1;
	printf("Proces %d salje poruku procesu %d.\n", rank, 1);
	MPI_Send(&message, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
} else if (rank == 1) {
	int message;
	printf("Proces %d treba da primi poruku od procesa %d.\n",rank, 0);
	MPI_Recv(&message, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, NULL);
	printf("Proces %d primio poruku %d od procesa %d.\n", rank, message, 0);
}

// ...
```

---
## Režimi Point-to-Point komunikacije
- P2P komunikacioni režimi slanja poruke:
	- `Synchronous` (MPI_Ssend)- pošiljalac šalje zahtev za slanje poruke, nakon što primalac odgovori poruka se šalje (*handshake protokol*)
	- `Buffered` (MPI_Bsend) - po iniciranju slanja poruka se šalje u bafer odakle je primalac može preuzeti
	- `Standard` (MPI_Send) - može imati karakteristike buffered ili `synchronous` režima (podrazumevano)
	- `Ready` (MPI_Rsend) - pretpostavlja se da je proces koji prima poruku već čeka na poruku u trenutku iniciranja slanja
- Za prijem poruke postoji samo jedan režim i poruka se smatra primljenom kada je preuzeta i može dalje da se koristi.

---
## Point to Point komunikacija

- Da bi proces2 primio poruku koju mu šalje proces1 mora da važi: 
- Da `comm` parametar oba procesa ima istu vrednost,
- Da parametar `dest` procesa 1 ima istu vrednost kao i parametar source procesa 2 ili da je vrednost parametra procesa 2 postavljena na `MPI_ANY_SOURCE`,
- Da parametar `tag` ima istu vrednost za oba procesa ili da je vrednost parametra tag `MPI_ANY_TAG`

.message.is-dark[
.message-header[
Primer
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/wrong_send_recv.c</a>
]
]

---
## Tipovi Point-to-Point komunikacije
- Slanje i prijem poruke mogu biti:
	- blokirajući - Ako `MPI_Send` blokirajuća, kontrola toka se neće vratiti pozivaocu funkcije sve dok uslov slanja ne bude ispunjen. Nakon izlaska iz funkcije bafer poruke može biti bezbedno prepisan. Ako je `MPI_Recv` blokirajuća kontrola se ne vraća pozivaocu funkcije sve dok poruka ne bude preuzeta (`podrazumevano`).
	- neblokirajući - Iz `MPI_Send`, tj. `MPI_Recv` se izlazi nakon inicijacije slanja, tj. primanja poruke. Kada se pojavi potreba za korišćenjem izvornog odnosno odredišnog bafera, potrebno je prethodno proveriti da li je podatak poslat tj. da li je stigao.

.center[
```c
MPI_{I}[S, B, R]Send(...), MPI_{I}Recv(...)
```
]

---
## Zadatak 2: Ping pong
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napraviti OpenMPI program implementiran u C programskom jeziku koji simulira igranje ping ponga između dva procesa. Lopticu simulirati promenljivom tipa int. Uvećati ovu promenljivu svaki put kada neki od procesa udari lopticu, odnosno, pre nego što neki od procesa pošalje promenljivu drugom procesu i ispisati odgovarajuću poruku.
]
]

.message.is-dark[
.message-header[
Primer
]
.message-body[
- Format očekivanog ispisa:
```console
p0 sent ping_pong_count to p1 and incremented it to 1.
p1 received ping_pong_count 1 from p0.
p1 sent ping_pong_count to p0 and incremented it to 2.
```

]
]

.message.is-warning[
.message-header[
Info
]
.message-body[
- **Napomene**: 
Pretpostavka je da će program biti pozvan opcijom `-np 2` i od ovoga se ne mora štititi.
Ne znači da program nije ispravan ukoliko ispis na ekranu nije u očekivanom redosledu.
]
]

---
## Zadatak 3: Ping pong - sekvencijalni ispis
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Modifikovati osnovni ping pong zadatak tako da se ispis na standardni izlaz odvija u redosledu u kojem procesi udaraju ping pong lopticu. Program pokrenuti sa tri procesa - procesi 0 i 1 igraju ping pong, dok treći proces ispisuje poruke na standardni izlaz. Svaki put kada neki od procesa igrača udari lopticu, on procesu štampaču šalje poruku koju treba ispisati na standardni izlaz. Poruke za ispis procesu štampaču stižu u proizvoljnom redosledu, ali on treba da ih ispiše u redosledu koji odgovara sekvencijalnom izvršavanju programa. Sve poruke su iste dužine.
- Ping pong se igra do 9.
  
]
]

.message.is-dark[
.message-header[
Primer
]
.message-body[
- Primer izvršavanja:
```console
p0 sent ping_pong_count to p1 and incremented it to 1.
p1 sent ping_pong_count to p0 and incremented it to 2.
p0 sent ping_pong_count to p1 and incremented it to 3.
```
]
]

---
## Zadatak 4: Prsten
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napisati OpenMPI C program koji prosleđuje žeton između procesa po principu prstena. Žeton je predstavljen brojem -1 i poseduje ga proces ranga 0. Svi procesi osim poslednjeg šalju žeton procesu sa rangom za jedan veći od svog. Poslednji proces (proces sa najvećim rangom u komunikatoru) žeton prosleđuje nazad procesu ranga 0. Nakon što proces ranga 0 primi žeton, program se završava. Ispisati poruku na standardni izlaz svaki put kada neki od procesa primi žeton.
]
]

.message.is-dark[
.message-header[
Primer
]
.message-body[
- Format očekivanog ispisa:
```console
Process 1 received token -1 from process 0
Process 2 received token -1 from process 1
Process 3 received token -1 from process 2
Process 0 received token -1 from process 3
```
]
]

---
## P2P: Neblokirajuća komunikacija
- `MPI_Isend` (i drugi režimi), `MPI_Irecv`
- Proces inicira slanje ili primanje poruke, ali **ne čeka na završetak operacije**.

.lcol[
```c
int MPI_Test(
	MPI_Request *request,
	int *flag,
	MPI_Status *status)
```

- Testira stanje zahteva i postavlja promenljivu flag na `true` ukoliko je zahtev izvršen, odnosno na `false` ukoliko nije..ref[1]
]
.rcol[
```c
int MPI_Wait(
	MPI_Request *request,
	MPI_Status *status)
```

- Testira da li je zahtev izvršen i završava se kada se zahtev izvrši.
- Funkcija je blokirajuća..ref[2]
]

.footer.medium[
<a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Test.3.php">1. MPI_Test docs</a><br>
<a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Wait.3.php">2. MPI_Wait docs</a>

] 
---
## Primer 3: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/send_recv_nonblocking.c</a>
```c
	if (rank == 0) {
		MPI_Request send_request;
		char *message = "Zdravo!";
		
		MPI_Issend(message, 8, MPI_CHAR, 1, 0, MPI_COMM_WORLD, &send_request);

		printf("Proces %d inicirao slanje poruke.\n", rank);
		
		printf("Proces radi nesto drugo dok se poruka salje.");

		int flag = 0;
		MPI_Test(&send_request, &flag, MPI_STATUS_IGNORE);

		if (flag != 0) 
			/* poslato */ 
		
		else 
			/* nije */
		
	}else /* ... */
```

---
## Primer 3: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/send_recv_nonblocking.c</a>
```c
	if (rank == 0) /* ... */
	} else {

		MPI_Request receive_request;
		char message[8];

		MPI_Irecv(message, 8, MPI_CHAR, 0, 0, MPI_COMM_WORLD, &receive_request);
		
		printf("Proces %d inicirao primanje poruke.\n", rank);
		
		printf("Proces radi nesto drugo dok se poruka prima...");

		MPI_Wait(&receive_request, MPI_STATUS_IGNORE);
		printf("Proces %d primio poruku: \"%s\"\n", rank, message);

	}
```

---
## Zadatak 5: Ping pong - neblokirajući sekvencijalni ispis
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Modifikovati zadatak 4 tako da slanje poruka procesu štampaču bude neblokirajuće. Pri tom se obezbediti da program radi korektno, odnosno da se ne desi da poruka koja nije poslata bude prepisana novom porukom pre nego se stara pošalje.
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`ping_pong_printf_async.c`</a>
]
]

---
## P2P: dinamička komunikacija
- Nekada poruke koje procesi razmenjuju nisu fiksne dužine. 
- Tada je prvo potrebno očitati dužinu poruke, alocirati bafer za poruku, pa tek onda započeti njeno primanje.

.lcol[
```c
MPI_Probe(
	int source,
	int tag,
	MPI_Comm comm,
	MPI_Status* status)
```

- Simulira primanje poruke.
- Popunjava status polje..ref[1]
]

.rcol[
```c
MPI_Get_count(
	const MPI_Status *status,
	MPI_Datatype datatype,
	int *count)
```

- Očitava broj primljenih podataka tipa datatype na osnovu status polja..ref[2]
]

.footer.medium[
<a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Probe.3.php">1. MPI_Probe docs</a><br>
<a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Get_count.3.php">2. MPI_Get_count docs</a>

] 
---
## Primer 4: dynamic_communication.c
```c
	if (rank == 0) {
		int size = rand() % 10 + 1;
		char *message = (char *) calloc(size + 1, sizeof(char));

		for (int i = 0; i < size; i++) {
			message[i] = 'a';
		}

		MPI_Send(message, size + 1, MPI_CHAR, 1, 0, MPI_COMM_WORLD);
		free(message);
	} else {
		MPI_Status status; int size;
		MPI_Probe(0, 0, MPI_COMM_WORLD, &status);
		MPI_Get_count(&status, MPI_CHAR, &size);

		char *message = (char *) malloc(size * sizeof(char));
		MPI_Recv(message, size, MPI_CHAR, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

		printf("Primljena poruka: %s.\n", message);
	}
```

---
## Zadatak 6: Ping pong - poruka varijabilne dužine
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Modifikovati zadatak 4 tako da se procesu printeru šalju poruke promenljive dužine. Koristiti funkcije `MPI_Probe` i `MPI_Get_count`. 
- Procesi mogu da izvrše maksimalno 999 razmena lopticom.
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[

-  <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`ping_pong_printf_variablelen.c`</a>
]
]

---
layout: false
name: kk
class: center, middle

# Kolektivna komunikacija
---
layout: true

.section[[Kolektivna komunikacija](#sadrzaj)]
---
## Kolektivna komunikacija
- (eng. *collective communication*)
- Komunikacija **svih** procesa unutar jednog komunikatora.
- Vrste kolektivne komunikacije:
	- Broadcast
	- Scatter
	- Gather
	- AllGather
	- Reduction
	- AllReduction
	- ...

---
## Kolektivna komunikacija: Broadcast
.lcol[
```c
int MPI_Bcast(
	void *buffer,
	int count,
	MPI_Datatype datatype,
	int root,
	MPI_Comm comm);
```
]

.rcol[
![:scale 75%](img/broadcast.png)
]
<br><br><br><br><br><br><br>

- Proces čiji je rank jednak vrednosti root parametra šalje poruku svim ostalim procesima iz komunikatora, uključujući i sebe.
---
## Primer 5: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`bcast.c`</a>
```c
int main(int argc, char *argv[]) {
	int rank, root = 0;

	MPI_Init(&argc, &argv);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	int token;
	if (rank == root) token = 123;
	MPI_Bcast(&token, 1, MPI_INT, root, MPI_COMM_WORLD);
	printf("Proces %d primio token %d.\n", rank, token);

	MPI_Finalize();
	
	return 0;
}
```

---
## Zadatak 7: Broadcast
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napisati OpenMPI C implementaciju `MPI_Bcast` funkcije korišćenjem `MPI_Send` i `MPI_Recv` funkcija. Rank procesa koji emituje vrednost žetona se unosi kao argument poziva programa. Vrednost žetona koji se emituje je -1. Nakon što root proces (proces koji emituje vrednost žetona) pošalje žeton ispisati poruku o tome.
- Nakon što svaki od preostalih procesa primi žeton, ispisati poruku i vrednost primljenog žetona.
]
]

.message.is-dark[
.message-header[
Primer
]
.message-body[
- Format očekivanog ispisa:
```console
Proces 0 poslao zeton -1
Proces 1 primio zeton -1
Proces 2 primio zeton -1
Proces 3 primio zeton -1
```
]
]

---
## Zadatak 8: Kolektivna komunikacija nad podskupom komunikatora
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kolektivnoj komunikaciji učestvuju svi procesi unutar komunikatora. Međutim, pri prešavanju kompleksnijih problema može se pojaviti potreba da se neki podatak pošalje samo delu procesa komunikatora. Ustanovili smo da korišćenje metoda kolektivne komunikacije može biti efikasnije u odnosu na pojedinačno pozivanje `MPI_Send` i `MPI_Recv` za svaki od procesa u komunikatoru kojima treba proslediti podatak. 
- `Kako biste podatak poslali samo delu procesa u nekom komunikatoru korišćenjem kolektivne komunikacije?`
]
]

---
## Zadatak 8: Kolektivna komunikacija nad podskupom komunikatora
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- U kolektivnoj komunikaciji učestvuju svi procesi unutar komunikatora. Međutim, pri prešavanju kompleksnijih problema može se pojaviti potreba da se neki podatak pošalje samo delu procesa komunikatora. Ustanovili smo da korišćenje metoda kolektivne komunikacije može biti efikasnije u odnosu na pojedinačno pozivanje `MPI_Send` i `MPI_Recv` za svaki od procesa u komunikatoru kojima treba proslediti podatak. 
- `Kako biste podatak poslali samo delu procesa u nekom komunikatoru korišćenjem kolektivne komunikacije?`
]
]

.message.is-success[
.message-header[
Odgovor
]
.message-body[
- Napraviti novi komunikator za procese kojima treba poslati podataka i koristiti kolektivnu komunikaciju na nivou novonapravljenog komunikatora.
]
]

---
## Kolektivna komunikacija: Scatter
.lcol[
```c
int MPI_Scatter(
	const void *sendbuf,
	int sendcount,
	MPI_Datatype sendtype,
	void *recvbuf,
	int recvcount,
	MPI_Datatype recvtype,
	int root,
	MPI_Comm comm);
```

]
.rcol[
![:scale 85%](img/scatter.png)
]
<br><br><br><br><br><br><br><br>

- Korenski proces šalje delove podataka procesima iz komunikatora..ref[1]
- Svaki proces dobija "parče" podatka iste veličine.

.footer.medium[
    1. <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Scatter.3.php">MPI_Scatter docs</a>
] 

---
## Primer 6: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`scatter.c`</a>
```c
int main(int argc, char *argv[]) {

	/* ... */
	int *data = NULL, *partial_data = NULL;
	int piecelen = datalen / size;

	if (rank == root) {
		/* inicijalizacija data niza */

	}
	partial_data = (int *) malloc(sizeof(int) * piecelen);

	MPI_Scatter(data, piecelen, MPI_INT, partial_data, piecelen, MPI_INT, root, MPI_COMM_WORLD);
	/* ... */
	return 0;
}
```

---
## Kolektivna komunikacija: Gather
.lcol[
```c
int MPI_Gather(
	const void *sendbuf,
	int sendcount,
	MPI_Datatype sendtype,
	void *recvbuf,
	int recvcount,
	MPI_Datatype recvtype,
	int root,
	MPI_Comm comm);
```

]
.rcol[
![:scale 85%](img/gather.png)
]
<br><br><br><br><br><br><br><br>
- Korenski proces prima podatke od procesa iz komunikatora..ref[1] 
- Svaki proces šalje "parče" podatka iste veličine.

.footer.medium[
    1. <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Gather.3.php">MPI_Gather docs</a>
] 

---
## Primer 7: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`gather.c`</a>
```c
int main(int argc, char *argv[]) {

	int *data = NULL, *partial_data = NULL;
	int piecelen = datalen / size;
	partial_data = (int *) malloc(sizeof(int) * piecelen);

	/* ... */
	if (rank == root)
	data = (int *) malloc(sizeof(int) * datalen);
	
	MPI_Gather(partial_data, piecelen, MPI_INT, data, piecelen, MPI_INT, root, MPI_COMM_WORLD);
	
	/* ... */
	
	return 0;
}
```

---
## Zadatak 9: Računanje srednje vrednosti
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napraviti OpenMPI C program koji računa srednju vrednost elemenata niza u više procesa korišćenjem funkcija `MPI_Scatter` i `MPI_Gather`. Program napisati tako da:
	- Korenski proces inicijalizuje niz dužine n nasumično izgenerisanim celim brojevima. Dužina niza mora biti deljiva brojem pokrenutih procesa.
	- Razdeliti izgenerisani niz na jednake delove između svih procesa.
	- Svaki proces treba da izračuna sumu elemenata niza koji su mu prosleđeni.
	- Nakon što su sve parcijalne sume sračunate, prebacuju se nazad korenskom procesu koji od parcijalnih suma pravi konačnu sumu koju deli ukupnim brojem elemenata i ispisuje srednju vrednost niza.
]
]

---
## Kolektivna komunikacija: AllGather
.lcol[
```c
int MPI_Allgather(
	const void *sendbuf,
	int sendcount,
	MPI_Datatype sendtype,
	void *recvbuf,
	int recvcount,
	MPI_Datatype recvtype,
	MPI_Comm comm);
```

]
.rcol[
![:scale 60%](img/allgather.png)
]
<br><br><br><br><br><br><br><br><br>
- Efektivno poziv `MPI_Gather` praćen `MPI_Bcast` pozivom..ref[1]
.footer.medium[
    1. <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Allgather.3.php">MPI_Allgather docs</a>
] 

---
## Primer 8: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`allgather.c`</a>
```c
int main(int argc, char *argv[]) {

	/* ... */
	int *data = (int *) malloc(sizeof(int) * size);
	int token = rank;
	MPI_Allgather(&token, 1, MPI_INT, data, 1, MPI_INT, MPI_COMM_WORLD);

	free(data);
	
	/* ... */
	
	return 0;
}
```

---
## Kolektivna komunikacija: Reduce
.lcol[
```c
int MPI_Reduce(
	const void *sendbuf,
	void *recvbuf,
	int count,
	MPI_Datatype datatype,
	MPI_Op op,
	int root,
	MPI_Comm comm);
```

]
.rcol[
![:scale 90%](img/reduce.png)
]
<br><br><br><br><br><br><br><br><br>
- Procesi unutar komunikatora šalju podatke korenskom procesu koji vrši redukciju nad podacima zadatom funkcijom (npr. `MPI_SUM`, `MPI_AND`...)..ref[1]
.footer.medium[
    1. <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Reduce.3.php">MPI_Reduce docs</a>
] 

---
## Primer 9: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`reduce.c`</a>
```c
int main(int argc, char *argv[]) {
	/* ... */

	int token = rank, result;
	MPI_Reduce(&token, &result, 1, MPI_INT, MPI_SUM, root, MPI_COMM_WORLD);

	printf("Proces %d: result = %d.\n", rank, result);
	MPI_Finalize();
	
	return 0;
}
```

---
## Zadatak 10: Računanje srednje vrednosti 2
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Modifikovati zadatak koji računa srednju vrednost elemenata niza tako da se u odgovarajućem koraku koristi funkcija `MPI_Reduce`.
]
]

---
## Kolektivna komunikacija: AllReduce
.lcol[
```c
int MPI_Allreduce(
	const void *sendbuf,
	void *recvbuf,
	int count,
	MPI_Datatype datatype,
	MPI_Op op,
	MPI_Comm comm);
```

]
.rcol[
![:scale 90%](img/allreduce.png)
]
<br><br><br><br><br><br><br><br><br>
- Efektivno poziv `MPI_Reduce` praćen `MPI_Bcast` pozivom.
.footer.medium[
    1. <a target="_blank" rel="noopener noreferrer" href="https://www.open-mpi.org/doc/v2.0/man3/MPI_Allreduce.3.php">MPI_Allreduce docs</a>
] 

---
## Primer 10: <a target="_blank" rel="noopener noreferrer" href="/courses/hpc-z4-openMPI/#table-of-contents"> ☛ Primeri/`allreduce.c`</a>
```c
int main(int argc, char *argv[]) {
	/* ... */

	int token = rank, result;
	MPI_Allreduce(&token, &result, 1, MPI_INT, MPI_SUM, MPI_COMM_WORLD);

	printf("Proces %d: result = %d.\n", rank, result);
	/* ... */
	
	return 0;
}
```

---
## Zadatak 11: Množenje matrice i vektora - domaći
.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napisati OpenMPI C program za množenje kvadratne matrice i vektora. Ulazna matrica i vektor sadrže razlomljene brojeve u jednostrukoj preciznosti i podrazumeva se da će dimenzije matrice i vektora biti odgovarajuće. Ulazni podaci za zadatak su izgenerisani i dati u h5 formatu. Takođe, dat je kostur rešenja sa primerima kako učitati matricu/vektor iz h5 datoteke i ispisati ih na standardni izlaz (direktorijum MatrixVectorMultiplication). Za detalje oko kompajliranja i pokretanja rešenja pogledati README.md datoteku.
- Implementirati:
  - Sekvencijalni algoritam za množenje matrice i vektora. Meriti vreme izvršavanja i ispisati ga na standardni izlaz.
  - OpenMPI C algoritam za množenje matrice i vektora. Meriti vreme izvršavanja i ispisati ga na standardni izlaz. Ostaviti mogućnost da se na standardni izlaz ispiše i rezultat računanja.

]
]

---
## Zadatak 12: Množenje matrica - domaći

.message.is-info[
.message-header[
Zadatak
]
.message-body[
- Napisati OpenMPI C program za množenje dve kvadratne matrice.
- Obe ulazne matrice sadrže brojeve u jednostrukoj preciznosti.
- Ulazni podaci za zadatak su izgenerisani i dati u h5 formatu.
- Takođe, dat je kosturešenja sa primerima kako učitati matricu iz h5 datoteke i ispisati ih na standardni izlaz (direktorijum MatrixMultiplication). 
- Za detalje oko kompajliranja i pokretanja rešenja, pogledati README.md datoteku.
- Implementirati:
	- Sekvencijalni algoritam za množenje dve kvadratne matrice. Meriti vreme izvršavanja i ispisati ga na standardni izlaz.
	- OpenMPI C algoritam za množenje dve kvadratne matrice. Meriti vreme izvršavanja i ispisati ga na standardni izlaz. Ostaviti mogućnost ispisa rezultata. U simulacijama na jednom računaru OpenMPI rešenje ne mora biti brže od sekvencijalnog.
]
]

---
## Korisni materijali
- [MPI standard dokumentacija](https://www.mpi-forum.org/docs/)
- [OpenMPI dokumentacija](https://www.open-mpi.org/doc/)
- Peter S. Pacheco "Parallel Programming with MPI"
- Victor Eijkhout "Parallel Computing" (besplatna onlajn verzija knjige)
- [MPI tutorijal](http://mpitutorial.com/)

{% endblock %}
