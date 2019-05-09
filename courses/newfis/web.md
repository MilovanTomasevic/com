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

#### Dekan: prof. dr. Dejan Jelovac

- [Faculty of Information Studies in Novo Mesto, Slovenia](https://www.fis.unm.si/si/)
]]]
]]

.footer.small[
- #### The slides were created with the support of the Dean for the project "NOVI FIŠ"
]
 
---

name: content

# Sadržaj

- [Nove funkcionalnosti](#part1)
- [Administacija](#part2)
- [Making your Own Image](#part3)
- [Docker on HPC](#part4)

---
name: part1
class: center, middle, inverse

# Nove funkcionalnosti


.columns[

.column[

![:scale 45%](assets/oko.png)

]
.column[

![:scale 37%](assets/web.png)
]

.column[

![:scale 30%](assets/katanac.png)

]

]

---
layout: true

.section[[Nove funkcionalnosti](#content)]

---
## Funkcionalnosti

- Najnovije CMS okruženje
- SEO optimizacija
- Dinamičko dodavanje sekcija
- Administriranje korisnika
- Integracija sa novim studentskim referatom
- Automatska optimizacija slika (prolagođavanje uređajima, brže učitavanje)
- Podrška za mobilne platformame (Andrioid i iOS)
- Generisanje događaja dinamički (dodavanje novih prozora na javni deo stranice)
- Višejezičnost

---
## Docker vs VMs
* Virtual machines solve the same problem as docker, but are much less lightweight
* Virtual machines package the entire guest OS, while Docker uses the host kernel and a minimal OS that can be shared between containers

.columns[
  .column[
    ![:scale 30%](assets/container_stack.png)
  ]
  .column[
    ![:scale 30%](assets/vm_stack.png)
  ]
]


---

name: part2
class: center, middle, inverse
layout: false

# Administacija

---
layout: true

.section[[Administacija](#content)]

---

## Administriranje prava korisnika

- Zaposleni i studenati




---


* To run a container, all you need to do is specify the image name, and docker will pull the image from Docker Hub, and begin running it 
* `docker run <IMAGE NAME>`

.message.is-info[
.message-header[
Exercise
]
.message-body[
- Run the following command. What does it output?

```bash
docker run hello-world
```
]
]
--
.message.is-success[
.message-header[
Answer
]
.message-body[
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```
]
]

---

name: end
class: center, middle
layout: false

# That's All 👨🏻‍🎓
# Thank you for listening!

--

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)

{% endblock %}