---
layout: page
title: Z5-Rešenja
description: >
  Baze podataka - rešenje zadataka
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

## Primer 1

{% highlight sql linenos %}
select *
from predmet
where bodovi >= all ( select bodovi
    from predmet);
{% endhighlight %}

## Primer 2

{% highlight sql linenos %}
select *
from dosije
where not god_rodjenja >= all ( select god_rodjenja
    from dosije);

select *
from dosije
where god_rodjenja < any ( select god_rodjenja
    from dosije);
{% endhighlight %}

## Primer 3

{% highlight sql linenos %}
select distinct current_time from dosije; values current time;
select current_time from sysibm.sysdummy1;
{% endhighlight %}

## Primer 4

{% highlight sql linenos %}
values user;
{% endhighlight %}

## Primer 5

{% highlight sql linenos %}
values dayname(date('18.11.2010')); 
values dayname('18.11.2010');
{% endhighlight %}

## Primer 6

{% highlight sql linenos %}
values (dayofyear(current_date), week(current_date), dayofweek(current_date), dayname(current_date), monthname(current_date));
{% endhighlight %}

## Primer 7

{% highlight sql linenos %}
values second(current_time);
{% endhighlight %}

## Primer 8

{% highlight sql linenos %}
values date('2008-11-11') - date('2005-08-06');

values (mod(date('2008-11-11') - date('2005-08-06'),100), (mod(date('2008-11-11') - date('2005- 08-06'),10000))/100, integer(date('2008-11-11') - date('2005-08-06'))/10000);

values (day(date('2008-11-11') - date('2005-08-06')), month(date('2008-11-11') - date('2005-08- 06')), year(date('2008-11-11') - date('2005-08-06')));
{% endhighlight %}

## Primer 9

{% highlight sql linenos %}
values current date + 12 years + 5 months + 25 days;
{% endhighlight %}

## Primer 10

{% highlight sql linenos %}
select *
from ispit
where datum_ispita>date('2011-01-28');

select *
from ispit
{% endhighlight %}

## Primer 11

{% highlight sql linenos %}
select *
from ispit
where current_date-datum_ispita < 800;
{% endhighlight %}

## Primer 12

{% highlight sql linenos %}
select indeks, ime || ' ' || prezime "ime i prezime", substr(ime,1,1) || substr(prezime,1,1) inicijali, replace(mesto_rodjenja, 'Beograd', 'Bg') "mesto rodjenja"
from dosije;

select indeks, ime || ' ' || prezime "ime i prezime", concat(substr(ime,1,1),substr(prezime,1,1)) as inicijali, replace(mesto_rodjenja, 'Beograd', 'Bg') "mesto rodjenja"
from dosije;
{% endhighlight %}

## Primer 13

{% highlight sql linenos %}
select indeks, ime, prezime, coalesce(mesto_rodjenja, 'Nepoznato') as "mesto rodjenja" 
from dosije;
{% endhighlight %}

## Primer 14

{% highlight sql linenos %}
values char(current_time, ISO), char(current_time, USA), char(current_time, LOCAL);
{% endhighlight %}

## Primer 15

{% highlight sql linenos %}
-- a)
select sifra, naziv, decimal(bodovi*1.200, 6, 2) as uvecanje 
from predmet;

-- b)
select sifra, naziv, ceil(bodovi*1.2) as uvecanja 
from predmet
where ceil(bodovi*1.2)>8;
{% endhighlight %}

## Primer 16

{% highlight sql linenos %}
select indeks, naziv, ocena, year(current_date-datum_ispita) godina, month(current_date- datum_ispita) meseci, day(current_date-datum_ispita) dana
from ispit i 
join predmet p on i.id_predmeta=p.id_predmeta
where year(current_date-datum_ispita)<=5 ;
{% endhighlight %}

## Primer 17

{% highlight sql linenos %}
select ime, prezime from dosije d1
where exists ( select *
    from dosije d2
    where d1.god_rodjenja+1=d2.god_rodjenja and d1.mesto_rodjenja=d2.mesto_rodjenja);

select ime, prezime
from dosije d1
where (god_rodjenja, mesto_rodjenja) IN (select god_rodjenja-1, mesto_rodjenja
    from dosije d2);

select d1.ime, d1.prezime
from dosije d1 
join dosije d2 on d1.god_rodjenja=d2.god_rodjenja-1 and d1.mesto_rodjenja=d2.mesto_rodjenja;
{% endhighlight %}

## Primer 18

{% highlight sql linenos %}
select naziv, id_predmeta from predmet p
where not exists (select *
    from dosije d
    where mesto_rodjenja='Beograd' and not exists (select * d.indeks=i.indeks and ocena>5));
        from ispit i
        where p.id_predmeta=i.id_predmeta and
{% endhighlight %}
