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

class: center, middle, inverse

# Arhitektura superračunara

## pregled mogućih rešenja

---
layout: true

.section[[Arhitektura superračunara](#sadrzaj)]

---

## Veza sa drugim predmetima

- Mi ovo radimo tek ovlaš.
- Mnogo više o ovome radite kod prof. Živanova
- Biće nešto preklapanja između onoga što ja kažem i onoga što on kaže, ali generalno govoreći od njega dobijate najbolje podatke.
- Svrha ovoga jeste da imamo rečnik koji delimo da bi mogli da
pričamo o tome šta programiramo.

---

## Faktori performanse HPC sistema

- Brzina komponenata
	- Koliko se brzo stvari dešavaju na individualnoj komponenti
- Paralelizam komponenata
	- Koliko stvari se izvršava jednovremeno
	- Koliko stvari se efektno paralelizuje
- Efikasnost komponenata
	- Koji procenat vremena je sistem sposoban da radi

---

## Faktori performanse HPC sistema

- P - performanse sistema kao apstrakcija
- e - efikasnost sistema
- S - faktor skaliranja
- a - funkcija dostupnosti
	- čiji je parametar R-pouzdanost
- µ - stopa izvršavanja instrukcija kao funkcija
	- čiji je parametar E-snaga

---

## Faktori performanse HPC sistema

<p>

$$P=e ×S×a(R)×\mu(E)$$
  
</p>

- O efikasnosti
  - Efikasnost se lako definiše: to je odnos ostvarenih i teoretskih performansi, tj.

<p>

$$e_{flops}=\frac{P_{kont}}{P_{max}}$$
  
</p>

---
## O pouzdanosti

- Sporiji algoritmi mogu da priušte da budu provereni i sigurno tačni
- Ali ako hoćemo brzinu greške su neminovne
- Sistemi su jednostavno previše komplikovani

---

## U prolazu

>Što više znam o kompjuterima, sve sam više iznenađen kada rade.
<br>Nepoznati FTN student, hol fakulteta 2008

- Ovo tek važi za superračunare.
- Svi ti delovi, sve te komponente...
- Stvarno jeste čudo što rade.

---

## Metodi pouzdanosti

- Greške nisu kraj sveta
- Pomislite na `TCP` vs. `UDP`
- Metod koji se koristi najčešće je `checkpoint/restart`
- Što da ne? 
- Sve što izgubimo je vreme.
- Ako je vreme koje gubimo na povremeni `restart` manje od vremena koje bi gubili na sporiji algoritam - pobeda!

---

## Flinova taksonomija

- Postoje mnogi načini da se opiše arhitektura paralelnih sistema i mnogi načini da se takvi sistemi projektuju.
- Da bi mogli da pričamo bez previše mahanja rukama i objašnjavanja dobro je da imamo nekakve zajedničke termine
- Primer takvih zajedničkih termina jeste Flinova taksonomija, sistem denotiranja različitih arhitektura koji je napravio Majkl Flin 70-tih kodina prošlog veka.

---

## Faktori Flinove taksonomije

- Flinova taksonomija ima dve dimenzije po kojima varira-podaci i instrukcije, označene u sistemu kao D i I.
- I D i I dimenzije mogu da variraju između dve vrednosti: S (single) i M (multiple) po tome da li se u jednom trenutku u sistemu izvršava operacija nad više tokova podataka ili se izvršava više niti kontrole
- Kombinatorika nam, onda, daje `ćoškove` faznog prostora HPC arhitekture:
	- SISD
	- SIMD
	- MIMD
	- MISD

---

## SISD

- Najklasičnija Fon Nojmanova arhitektura
- Jedna stvar u jednom trenutku
- Ništa danas ne radi ovako, praktično
- No, i dalje je koristan mentalni model pošto vaša userland aplikacija gotovo sigurno radi ovako.

---

## SIMD

- Uvek se radi ista stvar
- Ali se istovremeno radi na velikom skupu podataka
- Da bi to razumeli, hajde da razmišljamo o idealizovanoj grafičkoj kartici koja izvršava Fongov model senčenja

<p>

$$I_{p}=k_{b}i_{a}+\sum_m(k_{d}(\widehat{L}_{m}\cdot\widehat{N})i_{m,d}+k_{s}(\widehat{R}_{m}\cdot\widehat{V})^{\alpha}i_{m,s}) \\   \quad m\in light$$
  
</p>

---

## SIMD

```java

// Vertex Shader Source Code 

varying vec3 N;
varying vec3 v;

void main(void)  
{     
   v = vec3(gl_ModelViewMatrix * gl_Vertex);       
   N = normalize(gl_NormalMatrix * gl_Normal);

   gl_Position = gl_ModelViewProjectionMatrix * gl_Vertex;  
}
          
```

---

## SIMD

.medium[
```java
// Fragment Shader Source Code 
varying vec3 vN;
varying vec3 v; 
#define MAX_LIGHTS 3 

void main (void) 
{ 
   vec3 N = normalize(vN);
   vec4 finalColor = vec4(0.0, 0.0, 0.0, 0.0);
   
   for (int i=0;i<MAX_LIGHTS;i++)
   {
      vec3 L = normalize(gl_LightSource[i].position.xyz - v); 
      vec3 E = normalize(-v); // we are in Eye Coordinates, so EyePos is (0,0,0) 
      vec3 R = normalize(-reflect(L,N)); 
   
      //calculate Ambient Term: 
      vec4 Iamb = gl_FrontLightProduct[i].ambient; 
      //calculate Diffuse Term: 
      vec4 Idiff = gl_FrontLightProduct[i].diffuse * max(dot(N,L), 0.0);
      Idiff = clamp(Idiff, 0.0, 1.0); 
      // calculate Specular Term:
      vec4 Ispec = gl_FrontLightProduct[i].specular * pow(max(dot(R,E),0.0),0.3*gl_FrontMaterial.shininess);
      Ispec = clamp(Ispec, 0.0, 1.0); 
      finalColor += Iamb + Idiff + Ispec;
   }
   // write Total Color: 
   gl_FragColor = gl_FrontLightModelProduct.sceneColor + finalColor; 
}         
```
]

---
## SIMD

![:scale 60%](img/simd.png)

.footer.medium[
  [Detaljnije](https://www.opengl.org/sdk/docs/tutorials/ClockworkCoders/lighting.php)

] 

---
## SPMD

- U praski ovo nije baš SIMD.
- Nije paralelizam baš u instrukcijama
- Ali jeste slučaj da imamo jedan program a više tokova podataka.
- Tako da je moderan GPU striktno govoreći primer SPMD arhitekture.
- No, sa druge strane, SPMD je samo praktična inkarnacija SIMD ideje.
- Ne treba se previše držati Flinovih apstrakcija. One su divne da nam opišu `ćokove` prostora u kome obitavamo, ali kretanje po tom prostoru dozvoljava dosta varijacije.
- Moderan *heterogen sistem* je takav da ima zapanjujuće mnogo gradacije između stepena deljene memorije, nivoa paralelizma, itd.

---

## Heterogen sistem?

- Koristimo taj termin relativno često
- Programiranje u heterogenim sistemima će biti naš glavni izazov
- Pa šta je heterogen sistem?
- Neformalna definicija: 
    - Sistem koji meša razne delove Flinove taksonomije u svojim različitim delovima: hibrid raznih arhitektura i pristupa vo?en samo željom za najviše performansi.
- Heterogeni sistemi zvuče vrlo novo i uzbudljivo ali, istina je da su oni već duže vremena sa nama i da vi verovatno imate jedan.
- Lični računari su uvek bili heterogeni, a nikad više nego danas gde je razlika između superračunara i nekog gaming PC-ja uglavno stvar razmere ne prirode.

---

## SIMD nizovi

- SIMD niz (SIMD Array) je tehnika izrade superračunara koja je svoj vrhunac doživela u 80-tim i 90-tim godinama prošlog veka.
- Ideja je da se računar sastoji od jako mnogo prostih procesorskih elemenata kojima upravlja kontroler sekvence koji se stara da rade istu stvar u isto vreme
- Svaki takav element radi, onda, nad svojim komadom memorije uz minimalno povremeno komuniciranje sa memorijom drugih komada memorije.
- Ovo jako liči na fixed-pipeline arhitekturu grafičkih kartica ranijih generacija.

---

## Procena performansi i Amdalov zakon

- Amdalov zakon je metod da se proceni performansa paralelizovanog sistema
- Tačnije rečeno, Amdalov zakon povezuje stepen paralelizacije sa ostvarenim dobitkom u performansama.
- Ovo je težak problem, te je zakon samo približni model koji naročito odgovara baš SIMD nizovima.

---
## Amdalov zakon

- Zamislim SIMD niz procesor koji ima dva režima izvršavanja:
	- Sekvencijalni (gde izvršava funkciju po funkciju)
	- Paralelni (gde izvršava sa svim svojim PE-ovima)
- Neka je T0 ukupno vreme za neki prora?un
- A neka je TF vreme za sekvencijalno izvršavanje operacija koje se mogu izvršavati u paraleli, dok je TA vreme koje je zaista potrebno.
- Dalje, neka je g stepen paralizma, S stepen ubrzanja, a f proporcija operacija koje se mogu paralelizovati.
- Onda:

---

## Amdalov zakon

.small[
<p>

$$ Speedup_{parallel}(f,n)=\frac{1}{(1-f)+(\frac{f}{n})} \\ Speedup_{enhanced}(f,S)=\frac{1}{(1-f)+(\frac{f}{S})} $$
  
</p>
]

![:scale 60%](img/AmdahlsLaw.svg)

---

## MIMD

- Ovo je svet u kome mi, u stvari, živimo
- No, ponekad, to se krije od nas
- Najčistiji primer ovako nečega jeste sistem čvorova, gde je svaki čvor procesor koji komunicira sa drugim čvorovima preko mreže
- Provešćemo najviše vremena u hibridnom `MIMD/SIMD` svetu.

---

## Multiprocesori

- Praktično ovaploćenje `MIMD` pristupa.
- To je arhitektura modernih super-računara. 
- Mnogo čvorova, mnogo procesora.
- Multiprocesorske arhitekture imaju varijacije

---

## Multiprocesori deljene memorije

- Sistem gde imamo N procesora, deljenu magistralu, i jednu memoriju za sve.
- Idealno, ovo funkcioniše kao UMA arhitektura - *uniform memory access*. 
- To znači da bilo koji deo memorije je ne samo jednako dostupan bilo kom procesoru, nego je dostupan u jednakom vremenu.

---

## Multiprocesori deljene memorije

![:scale 85%](img/uma.gif)

---

## Problem keša

- Koherentnost keša je ogroman problem za ovakve arhitekture
- Tipično se koristi *Modified Exclusive Shared Invalid* protokol, tkzv. `MESI`.
- Poznat još i kao `njuškalo keš` (*Snooping cache*)
- `UMA` je moguća (donekle) na običnom računaru
- Ali današnje mašine su skoro sve `NUMA`, tj. ne `UMA`: neka memorija je blizu a neka daleko. Fizički a i logički.

---

## Masivno paralelni multiprocesori

- `MPM`-ovi nemaju deljenu memoriju nego `distribuiranu memoriju`.
- Striktno govoreći (i u stilu toga što su nam sve oznake labave i predstavljaju samo proizvoljne linije povučene po kontinualnim skalama) model distribuirane memorije je samo ekstreman `NUMA` model.
- Kako?
- Pa čvor ima svoju memoriju koja je `NUMA`-tično manje-više deljena, ali je povezan sa svim drugim čvorovima preko interfejsa za prosleđivanje poruka koji, tehnički, omogućava pristup (iako jako spor i indirektan) svoj memoriji sistema.

---

## Kako to izgleda danas

![:scale 75%](img/cpu.png)

---

## Potrošački klasteri

- Potrošački klaster (*comodity cluster*) nije toliko posebna arhitektura koliko je specifična implementacija masivno paralelne arhitekture.
- Ideja je da se `MPM` arhitektura implementira upotrebljavajući hardver koji je namenjen potrošačima
- Zašto ovo radi?
	1. Ekonomije skale.
	2. Video igre.
- Naravno rešenje je manje efikasno: vrhunska `MPM` rešenja su skoro pa 90% efikasna na odabranim problemima.
- Čak i najbolji potrošački klasteri retko probijaju 60%.
- Ali su mnogo jeftiniji i ekonomski se skaliraju fantastično.

---

## Internet-bazirani potroša?ki klasteri

- Ekstreman primer ovoga jeste ideja da svako ko hoće može da postane deo super-računara koji rešava neki ogroman problem.
- Folding@home, na primer.
- Ovo je slučaj gde je izolovanost čvorova komično velika, ali je to i dalje sasvim OK za određeni tip problema, uglavnom u kategoriji smešno paralelnih (više o tome kasnije).
- Najuspešniji primer je verovatno globalna bitcoin mreža
	- Pet miliona triliona proračuna SHA256 vrednosti svakog sekunda.

---

## MISD

- Neki smatraju da je ovo besmislena kategorija
- Isti podaci a više tokova instrukcija?!
- Imaju dve klasične interpretacije:
	- Coarse-grained pipeline
	- Deljena memorija
- Na jedan način, moglo bi se reći da će OpenMP sa kojim se suočavate večeras primer MISD arhitekture.

--

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}