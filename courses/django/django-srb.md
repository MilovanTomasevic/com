{% extends "base_slides.md" %}
{% block slides %}
{% raw %}

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

- [Igor Dejanović, Ph.D., Associate Professor](http://igordejanovic.net/)
- [Faculty of Technical Sciences](http://ftn.uns.ac.rs/)
- [Chair of Informatics](http://informatika.ftn.uns.ac.rs/)
]]]
]]

.footer.small[
- #### Slides are created according to sources in the literature & Acknowledgements
]
 
---
name: sadrzaj

# Sadržaj

- [Kreiranje i podešavanje projekta](#setup)
- [Django modeli](#models)
- [Admin aplikacija](#admin)
- [Pogledi - *Views*](#views)
- [Forme](#forms)
- [Generički pogledi](#genericviews)
- [Šabloni detaljnije](#templates)
- [Testiranje](#testing)
- [Stilizovanje aplikacije](#styling)
          
---
name: uvod
class: center, middle, inverse

#  Kreiranje i podešavanje projekta

---
layout: true

.section[[Setup](#sadrzaj)]

---

## Django

- Python okvir za razvoj veb aplikacija
- BSD licenca
- DRY princip
- *Full stack*
- *Database migration*
- Modularan - aplikacije
- Automatski admin interfejs
- Velika i aktivna zajednica
- *Caching, syndication, internationalization...*

---

## Instalacija

```bash
$ pip install Django
```

Provera da li je instaliran:

```bash
$ python -c "import django; print(django.get_version())"
```

.footer[
  **Preporuka:** Koristiti virtualno okruženje
  ([virtualenv](https://virtualenv.readthedocs.org/en/latest/))
]

---

## Kreiranje projekta

```sh
$ django-admin startproject mysite
```

Ovo će kreirati sledeće fajlove i foldere:

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

---

## Podešavanje sajta

- Modul `mysite/settings.py`.
- Običan Python modul sa strukturama podataka za podešavanje svih aspekata sajta
  (baza, aplikacije, middleware...)
- Podrazumevano se koristi `sqlite3` baza što je sasvim dovoljno za razvoj.

```python
...
# Database
# https://docs.djangoproject.com/en/1.8/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
...
            
```

---
            
## Pokretanje projekta

```python
$ ./manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until
they are applied.
Run 'python manage.py migrate' to apply them.

November 08, 2015 - 12:58:17
Django version 1.8.6, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

---

## Početna strana

![](DjangoStarted.png)

---

## Kreiranje inicijalne šeme baze podataka

```python
$ ./manage.py migrate
Operations to perform:
  Synchronize unmigrated apps: staticfiles, messages
  Apply all migrations: sessions, admin, auth, contenttypes
Synchronizing apps without migrations:
  Creating tables...
    Running deferred SQL...
  Installing custom SQL...
Running migrations:
  Rendering model states... DONE
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying sessions.0001_initial... OK
```
            

Inicijalna šema potiče od podrazumevano uključenih aplikacija.

---

## Admin interfejs

- Implementiran kao standardna Django aplikacija.
- U verziji 1.8 podrazumevano aktiviran.
- Potrebno je kreirati *admin* korisnika.

```python
$ python manage.py createsuperuser
Username (leave blank to use 'igor'):
Email address: igord@uns.ac.rs
Password:
Password (again):
Superuser created successfully.
```
---

## Pristup admin interfejsu

- Posle pokretanja aplikacije moguće je pristupiti admin interfejsu.
- Podrazumevano se pristupa na *http://localhost:8000/admin/*

![:scale 80%](DjangoAdmin1.png)

---

## Projekti i aplikacije

- Aplikacija je modul koji radi nešto korisno. Na primer, blog sistem,
  aplikacija za glasanje, forum i sl.
- Projekat predstavlja skup aplikacija i konfiguracije za određene web sajtove.

---

## Kreiranje aplikacije unutar projekta

```python
$ python manage.py startapp polls
```
            
Kreira fajlove i foldere sledećeg oblika:

```
polls/
    __init__.py
    admin.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

---

name: models
class: center, middle, inverse
layout: false

# Django modeli

---
layout: true

.section[[Modeli](#sadrzaj)]

---

## Django modeli

- Smešteni su u *[app_name]/models.py* fajlu
- Predstavljaju definitivan izvor definicije podataka u aplikaciji.
- DRY - sve ostalo vezano za podatke se dedukuje iz modela.

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```
            
---

## Aktivacija modela

- Potrebno je prvo aktivirati aplikaciju. Obavlja se dodavanjem imena aplikacije
  u `settings.py` fajl za sajt, n-torka `INSTALLED_APPS`

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls',
)
```

---

## Kreiranje migracije

```python
$ python manage.py makemigrations polls
Migrations for 'polls':
  0001_initial.py:
    - Create model Choice
    - Create model Question
    - Add field question to choice
```
            
Kreiran je fajl `polls/migrations/0001_initial.py`.
            
---

## Pregled SQL-a za određenu migraciju

```python
$ python manage.py sqlmigrate polls 0001
BEGIN;

CREATE TABLE "polls_choice" (
    "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
    "choice_text" varchar(200) NOT NULL,
    "votes" integer NOT NULL
);
CREATE TABLE "polls_question" (
    "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
    "question_text" varchar(200) NOT NULL,
    "pub_date" datetime NOT NULL
);
...
```

---

## Primena migracije nad bazom

```python
$ python manage.py migrate
Operations to perform:
  Synchronize unmigrated apps: messages, staticfiles
  Apply all migrations: sessions, admin, contenttypes, auth, polls
Synchronizing apps without migrations:
  Creating tables...
    Running deferred SQL...
  Installing custom SQL...
Running migrations:
  Rendering model states... DONE
  Applying polls.0001_initial... OK
```
            
---

## Migracije - sumarno

Kod migracije šeme baze podataka uz očuvanje podataka raditi
  sledeće:

  - Modifikovati <code>model.py</code>
  - Kreirati migraciju:

```python
$ python manage.py makemigrations
```

  - Primeniti migraciju:

```python
$ python manage.py migrate
```

---

## Registracija modela u admin interfejsu

- U fajlu `polls/admin.py`:

```python
from django.contrib import admin

from .models import Question, Choice

admin.site.register(Question)
admin.site.register(Choice)
```

![:scale 70%](DjangoAdmin3.png)

---

## Model API

```python
>>> from polls.models import Question, Choice
>>> Question.objects.all()
[]
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()
[<Question: Question object>]
```

---

## String reprezentacija instanci modela

```python
from django.db import models

class Question(models.Model):
    # ...
    def __str__(self):              # __unicode__ on Python 2
        return self.question_text

class Choice(models.Model):
    # ...
    def __str__(self):              # __unicode__ on Python 2
        return self.choice_text
```
            

- Na Python 2 definisati `__unicode__` metodu umesto `__str__`.
- Django automatski dodaje `__str__` koji poziva `__unicode__` i
  enkoduje ga u `UTF-8`.
  
---

## Korisničke metode nad modelom

```python
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date &gt;= timezone.now() - datetime.timedelta(days=1)
```

---

## Model API (2)

```python
>>> from polls.models import Question, Choice
>>> Question.objects.all()
[<Question: What's up?>]

>>> Question.objects.filter(id=1)
[<Question: What's up?>]
>>> Question.objects.filter(question_text__startswith='What')
[<Question: What's up?>]

>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>
```

---

## Model API (3)

```python
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

>>> Question.objects.get(pk=1)
<Question: What's up?>

>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

>>> q = Question.objects.get(pk=1)

>>> q.choice_set.all()
[]
```

---

## Model API (4)

```python
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

>>> c.question
<Question: What's up?>

>>> q.choice_set.all()
[<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]
>>> q.choice_set.count()
3

>>> Choice.objects.filter(question__pub_date__year=current_year)
[<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]

>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```

---
name: admin 
class: center, middle, inverse
layout: false

# Podešavanje admin aplikacije 

---
layout: true

.section[[Admin](#sadrzaj)]

---

## Podešavanje admin aplikacije

- Videti uputstvo [ovde](https://docs.djangoproject.com/en/1.10/intro/tutorial02/#introducing-the-django-admin)


---
name: views
class: center, middle, inverse
layout: false

# Pogledi - *Views*

---
layout: true

.section[[Pogledi](#sadrzaj)]

---

## Pogledi - *Views*


- Predstavljaju "vrstu" web stranice: blog home, arhiva po godini...
- U `polls` aplikaciji imaćemo 4 pogleda:

  - `Question` index stranicu
  - `Question` stranicu sa detaljima
  - `Question` stranicu sa rezultatima
  - Akciju za glasanje

- Pogledi generišu sadržaj koji se dostavlja klijentu na zahtev (URL request).
- Sadržaj mogu biti web stranice ali mogu biti npr. i JSON ili XML stringovi.
- Realizuju se kao python funkcije ili metode klase.

---

## Prvi pogled

- U fajlu `polls/views.py`:

```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
            
```

Iz pogleda se vraća `HttpResponse` instanca ili izuzetak.

---

## Rutiranje zahteva

.medium[
- Svaki URL zahtev se mapira na odgovarajuću *view* funkciju posredstvom tzv.
  *URLconf-a*.
- Kreirati fajl `polls/urls.py` sa sadržajem:
]

```python
from django.conf.urls import patterns, url

from polls import views

urlpatterns = patterns('',
    url(r'^$', views.index, name='index'),
)
```
.medium[
- Zatim u glavnom *URLconf* fajlu za sajt `mysite/urls.py` uključiti *URLconf*
  aplikacije `polls`.
]
            
```python
from django.conf.urls import patterns, include, url
from django.contrib import admin

urlpatterns = patterns('',
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', include(admin.site.urls)),
```
            
---

## Rutiranje zahteva (2)

Ako sada odete na adresu `http://localhost:8000/polls/` dobićete
string 

`Hello, world. You’re at the polls index.`

---

## Dodatni *polls* pogledi

Fajl `polls/views.py`:

```python
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```
            
---

## Dodatne rute

Fajl `polls/urls.py`:

```python
from django.conf.urls import patterns, url

from polls import views

urlpatterns = patterns('',
    # ex: /polls/
    url(r'^$', views.index, name='index'),
    # ex: /polls/5/
    url(r'^(?P<question_id>\d+)/$', views.detail, name='detail'),
    # ex: /polls/5/results/
    url(r'^(?P<question_id>\d+)/results/$', views.results, name='results'),
    # ex: /polls/5/vote/
    url(r'^(?P<question_id>\d+)/vote/$', views.vote, name='vote'),
)
```

---

## Pogled koji nešto zaista i radi

Fajl `polls/views.py`:

```python
from django.http import HttpResponse

from polls.models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([p.question_text for p in latest_question_list])
    return HttpResponse(output)

# Leave the rest of the views (detail, results, vote) unchanged
```

- Problem je što je izled vraćene strane hardkodiran u *view* funkciji.
- Prepustićemo renderovanje stranice Django obrađivaču šablona.

---

## Šabloni

- Kreiramo folder `templates` unutar `polls` aplikacije.
- Django koristi n-torku `TEMPLATE_LOADERS` za *callables* koji "znaju" kako da
  učitaju šablon iz raličitih izvora.
- Jedan od loadera je `django.template.loaders.app_directories.Loader` koji
  učitava iz `templates` foldera aplikacije.
- U `templates` folderu kreiramo `polls` direktorijum i u njemu fajl
  `index.html`.
- Putanja je dakle `polls/templates/polls/index.html`
- Iz aplikacije šablon se (zahvaljujući loaderu) referencira sa
  `polls/index.html`

---

## Prvi šablon

Fajl `polls/templates/polls/index.html`:

```django
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">
        {{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```

---

## Ažuriranje pogleda da koristi šablon

```python
from django.http import HttpResponse
from django.template import RequestContext, loader

from polls.models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = RequestContext(request, {
        'latest_question_list': latest_question_list,
    })
    return HttpResponse(template.render(context))
```

---

## Prečica `render`

- Učitavanje i rederovanje šablona i vraćanje `HttpResponse` instance
  je čest slučaj.
- Zbog toga postoji funkcija koja obavlja sav taj posao - `render()`

```python
from django.shortcuts import render
from polls.models import Question


def index(request):
    latest_question_list = Question.objects.all().order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```

---

## Greška 404

Recimo da želimo da renderujemo detalje `Question` objekta.

```python
from django.http import Http404
from django.shortcuts import render

from polls.models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404
    return render(request, 'polls/detail.html', {'question': question})
```

Za sada šablon `polls/detail.html` može biti prosto:

```django
{{ question }}
```
            
---

## Prečica `get_object_or_404`

- Čest obrazac je pronalaženje objekta po id-u i podizanje greške 404 ukoliko ne
  postoji.
- Za to može da se upotrebi prečica `get_object_or_404()`.

```python
from django.shortcuts import get_object_or_404, render

from polls.models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```

- Takođe postoji i `get_list_or_404()` koja koristi `filter` i podiže grešku 404
  ukoliko je lista prazna.
  
---

## Prepravka <code>details</code> šablona

Fajl `polls/templates/polls/detail.html`:

```django
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```

---

## URL-ovi u šablonima

- Link u `index.html` šablonu je bio delimično hardkodiran:

```django
<li>
  <a href="/polls/{{ question.id }}/">{{ question.question_text}}</a>
</li>
```

- To nije dobro kod većih aplikacija jer otežava promenu URL šeme.
- Zbog toga je bolje koristiti `{% url %}` tag.

```django
<li><a href="{% url 'detail' question.id %}">
  {{ question.question_text }}</a></li>
            </code></pre>
            <p>Fajl <code>polls/urls.py</code></p>
            <pre><code data-trim class="python">
...
```

```python
# the 'name' value as called by the {% url %} template tag
url(r'^(?P<question_id>\d+)/$', views.detail, name='detail'),
...
```
            

---

## *Namespaces* u URL rutama

.medium[
- U prethodnom primeru `url` tag referencira rutu iz `urls.py` fajla po imenu.
- Problem je ako imamo rute koje se isto zovu u više aplikacija.
- To se rešava domenom imena (*namespace*).

Fajl `mysite/urls.py`:

```python
from django.conf.urls import patterns, include, url
from django.contrib import admin

urlpatterns = patterns('',
    url(r'^polls/', include('polls.urls', namespace="polls")),
    url(r'^admin/', include(admin.site.urls)),
)
```
Fajl `polls/templates/polls/index.html`:

```django
<li>
   <a href="{% url 'polls:detail' question.id %}"> {{ question.question_text }}</a>
</li>
```
]
            

---
name: forms
class: center, middle, inverse
layout: false

# Forme

---
layout: true

.section[[Forme](#sadrzaj)]

---

## Pisanje jednostavne forme

Šablon `polls/detail.html`:

```django
<h1>{{ question.question_text }}</h1>

{% if error_message %}<p><strong>
  {{ error_message }}</strong></p>{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
{% for choice in question.choice_set.all %}
    <input type="radio" name="choice" id="choice{{ forloop.counter }}"
        value="{{ choice.id }}" />
    <label for="choice{{ forloop.counter }}">
        {{ choice.choice_text }}</label><br />
{% endfor %}
<input type="submit" value="Vote" />
</form>
```

---

## Pogled

.medium[
Fajl `polls/urls.py`:

```python
url(r'^(?P<question_id>\d+)/vote/$', views.vote, name='vote'),
```
            
Fajl `polls/views.py`:

```python
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect, HttpResponse
from django.core.urlresolvers import reverse
from polls.models import Choice, Question
# ...
def vote(request, question_id):
    p = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = p.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        return render(request, 'polls/detail.html', {
            'question': p,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(p.id,)))
```
]

---

## Rezultati glasanja

.medium[
Po uspešnom glasanju (POST forme) vrši se redirekcija na `polls:results` pogled.

Fajl `polls/views.py`:

```python
from django.shortcuts import get_object_or_404, render


def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})
```
            
Šablon `polls/templates/polls/results.html`:

```django
<h1>{{ question.question_text }}</h1>

<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }} -- {{ choice.votes }}
      vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>

<a href="{% url 'polls:detail' question.id %}">Vote again?</a>
```
]

---
name: genericviews
class: center, middle, inverse
layout: false

# Generički pogledi

---
layout: true

.section[[Generički](#sadrzaj)]

---

## Generički pogledi

- Prethodno prikazani pogledi su često korišćeni u web aplikacijama

  - Učitavanje podataka iz baze na osnovu parametra prosleđenog preko URL-a.
  - Renderovanje šablona i vraćanje rezultata.

- Generički pogledi upravo predstavljaju ovaj obrazac koda.
- Dva generička pogleda: `ListView` i `DetailsView`.

---

## Prerada polls urls.py

Fajl `polls/urls.py`:

```python
from django.conf.urls import patterns, url

from polls import views

urlpatterns = patterns('',
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', views.DetailView.as_view(), name='detail'),
    url(r'^(?P<pk>\d+)/results/$', views.ResultsView.as_view(),
          name='results'),
    url(r'^(?P<question_id>\d+)/vote/$', views.vote, name='vote'),
)
```

---

## Prerada pogleda

Fajl `polls/views.py`:

```python
...

class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]

class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'

class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'
...
```

---

## `ListView`

- Prikaz liste objekata.
- Podrazumevani šablon je oblika `<app name>/<model name>_list.html`
- To se može promeniti navođenjem atributa <code>template_name</code>
- U kontekst šablona listi objekata se pristupa preko reference
  `<model_name>_list` pri čemu je ime modela u *lowercase-u* (npr.
  `question_list`)
- Ovo se menja preko `context_object_name`.
- Podrazumevano je listanje svih objekata modela. Ovo se može promenitit
  navođenjem metode `get_queryset(self)`.
  
---

## `DetailsView`

- Prikaz detalja pojedinačnog objekta.
- `id` objekta se iz URL-a prosleđuje pod imenom `pk`.
- Važe ista pravila za određivanje šablona pri čemu je ime oblika
  `<app name>/<model name>_detail.html`
- U kontekstu šablona instanci se pristupa po imenu modela u *lowercase-u*.


---
name: templates
class: center, middle, inverse
layout: false

# Šabloni detaljnije

---
layout: true

.section[[Šabloni](#sadrzaj)]

---

## Šabloni

- Tekstualni fajlovi koji imaju fiksne i varijablne delove.
- Koriste se za generisanje proizvoljnog tekstualnog sadržaja: HTML, JSON, XML,
    CSS, JavaScrip, Java, Python, Email, izveštaji...
    
---

## Primer

```django
{% extends "base_generic.html" %}

{% block title %}{{ section.title }}{% endblock %}

{% block content %}
<h1>{{ section.title }}</h1>

{% for story in story_list %}
<h2>
  <a href="{{ story.get_absolute_url }}">
    {{ story.headline|upper }}
  </a>
</h2>
<p>{{ story.tease|truncatewords:"100" }}</p>
{% endfor %}
{% endblock %}
```

---

## Varijable konteksta

- Navode se u obliku `{{ varijabla  }}`.
- Može se koristiti dot notacija za pristup atributima varijable pri čemu
  je semantika sledeća:

  - Prvo se pokušava po ključu rečnika
  - Zatim pristup atributu ili metodi
  - Na kraju se pokušava pristup po numeričkom indeksu (deo iza tačke mora biti
    numerički)

- Ukoliko varijabla ne postoji u kontekstu referenca će biti renerovana na
  osnovu podešavanja `TEMPLATE_STRING_IF_INVALID` što je
  podrazumevano prazan string.
  
---

## Filteri

Na prikaz varijable se može uticati filterima.

```django
{{ value|default:"nothing" }}
  Za value == None -- nothing
{{ value|length }}
  Za value == [1, 2, 3] -- 3
{{ value|filesizeformat }}
  Za value == 123456789 -- 117.7 MB
```
Filteri se mogu povezivati:

```django
{{ text|escape|linebreaks }}
```

Mogu imati parametre:

```django
{{ bio|truncatewords:30 }}
{{ list|join:", " }}
```

---

## Tagovi

Složenije konstrukcije oblika:

```django
{% tag %} ... sadržaj... {% endtag %}
```

Služe za implementaciju kontrole toke (petlji, uslova), učitavanje eksternih
informacija i sl.

---

## `For`

```django
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```

---

## `If`, `elif` i `else`
            
```django
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
```

```django
{% if athlete_list|length &gt; 1 %}
   Team: {% for athlete in athlete_list %} ... {% endfor %}
{% else %}
   Athlete: {{ athlete_list.0.name }}
{% endif %}
```

---

## Nasleđivanje šablona

- Najkompleksniji i najmoćniji mehanizam Django obrađivača šablona.
- Omogućava definisanje šablona najvišeg nivoa i zatim redefiniciju i
  specijalizaciju za konkretne slučajeve.
- Ovim se većina konkretnih šablona minimizuje.

---

## Nasleđivanje šablona (2)

```django
&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;link rel="stylesheet" href="style.css" /&gt;
    &lt;title&gt;{% block title %}My amazing site{% endblock %}&lt;/title&gt;
&lt;/head&gt;

&lt;body&gt;
    &lt;div id="sidebar"&gt;
        {% block sidebar %}
        &lt;ul&gt;
            &lt;li&gt;&lt;a href="/"&gt;Home&lt;/a&gt;&lt;/li&gt;
            &lt;li&gt;&lt;a href="/blog/"&gt;Blog&lt;/a&gt;&lt;/li&gt;
        &lt;/ul&gt;
        {% endblock %}
    &lt;/div&gt;

    &lt;div id="content"&gt;
        {% block content %}{% endblock %}
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

---

## Nasleđivanje šablona (3)

```django
{% extends "base.html" %}

{% block title %}My amazing blog{% endblock %}

{% block content %}
{% for entry in blog_entries %}
    <h2>{{ entry.title }}</h2>
    <p>{{ entry.body }}</p>
{% endfor %}
{% endblock %}
```

---

## Nasleđivanje šablona (4)

- Roditeljski `block` tagovi treba da imaju podrazumevani sadržaj.
- Ukoliko primetite da duplirate kod u šablonima to je znak da treba da kreirate
  blok i da ga smestite u roditeljski šablon i onda samo redefinišete gde je
  potrebno.
- Sadržaj roditeljskog bloka se može referencirati iz bloka putem `{{
  block.super }}`.
- `endblock` opciono može definisati ime što je zgodno kod većih šablona.

```django
{% block content %}
...
{% endblock content %}
```

---

## Automatski HTML <i>escaping</i>

- Sprečavanje *Cross Site Scripting (XSS)*.
- Django automatski uključuje *HTML escaping* za sve stringove koje renderuje.
- To je moguće isključiti za pojedine delove šablona ili na nivou celog obrađivača.
- Sledeći karakteri se konvertuju:

  - `<` se konvertuje u `&amp;lt;`
  - `>` se konvertuje u `&amp;gt;`
  - `'` (jednostruki navodnici) se konvertuju u `&amp;#39;`
  - `"` (dvostruki navodnici) se konvertuju u `&amp; quot;`
  - `&` se konvertuje u `&amp;amp;`

---

## Automatski HTML *escaping* - varijable

```django
This will be escaped: {{ data }}
This will not be escaped: {{ data|safe }}
```
Za `data` vrednost '&amp;lt;b&amp;gt;' rezultuje sledećim kodom

```django
This will be escaped: &amp;lt;b&amp;gt;
This will not be escaped: &lt;b&gt;
```
           
---

## Automatski HTML *escaping* - blokovi

```django
{% autoescape off %}
    Hello {{ name }}
{% endautoescape %}
```

```django
Auto-escaping is on by default. Hello {{ name }}

{% autoescape off %}
    This will not be auto-escaped: {{ data }}.

    Nor this: {{ other_data }}
    {% autoescape on %}
        Auto-escaping applies again: {{ name }}
    {% endautoescape %}
{% endautoescape %}
```

---

## Pozivi metoda u šablonima

- Moguće je pozivati metode koje nemaju parametre.
- Sintaksa je ista kao za pristup atributima.

```django
{% for comment in task.comment_set.all %}
    {{ comment }}
{% endfor %}
```
            
```django
{{ task.comment_set.all.count }}
```
            
Moguće je pozivati i korisničke metode.

```python
class Task(models.Model):
    def foo(self):
        return "bar"
```
            
```django
{{ task.foo }}
```
            
---

## Biblioteke tagova i filtera

- Tagovi i filteri se mogu definisati od strane korisnika ili autora aplikacija.
- Učitavaju se sa tagom `load`.

```django
{% load humanize %}

{{ 45000|intcomma }}
```

- U ovom slučaju aplikacija `django.contrib.humanize` mora biti omogućena u
  konfiguraciji `INSTALLED_APPS`.
- Moguće je istovremeno učitati više biblioteka.

```django
{% load humanize i18n %}
```   

---
name: testiranje
class: center, middle, inverse
layout: false

# [Testiranje](https://docs.djangoproject.com/en/1.10/topics/testing/)

---
name: static-files
class: center, middle, inverse
layout: false

# [*Static files*](https://docs.djangoproject.com/en/1.10/howto/static-files/deployment/)

---
name: styling
class: center, middle, inverse
layout: false

# [Stilizovanje aplikacija](https://docs.djangoproject.com/en/1.10/intro/tutorial06/)

---


## Reference

- [Django dokumentacija](https://docs.djangoproject.com/en/1.10/)
--

class: center, middle, theend, hide-text
layout: false
background-image: url(../theend.gif)
{% endraw %}

{% endblock %}