---
layout: page
title: OpenMP training
description: >
  High-performance computing (HPC)
hide_description: true

---

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

## Setup

### Installation

```sh
apt-get install libopenmpi-dev 
apt-get install openmpi-bin 
```

### Compilation & Running OpenMP
```sh
gcc -o name name.c -fopenmp
./name
```

### Compilation & Running MPI

```sh
mpicc filename.c -o filename 
mpirun -np 1 ./filename # -lm
```

## Exercises

### omp_threadnum.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>

int main (int argc, char *argv[])
{
  printf("Hello World from thread = %d\n", omp_get_thread_num());
}
{% endhighlight %}
omp_threadnum.c - exercises
{:.figure}

### omp_hello.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>

int main (int argc, char *argv[])
{
  {
  printf("Hello World from thread = %d\n", omp_get_thread_num());
  }
}
{% endhighlight %}
omp_hello.c - exercises
{:.figure}

### omp_private.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>

int main (int argc, char *argv[])
{

int tid, nthreads;

/* Fork a team of threads */
#pragma omp parallel
{
  tid = omp_get_thread_num();
  nthreads = omp_get_num_threads();
  printf("Hello World from thread = %d out of %d\n", tid, nthreads);
  }  /* All threads join master thread and disband */
}
{% endhighlight %}
omp_private.c - exercises
{:.figure}

### omp_vectadd.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>
#include <stdlib.h>

int main (int argc, char *argv[])
{

// Initialize arrays 
int size=1000000;
int a[size];
int b[size];
int c[size];

// Time the parallel for-loop
double start,end;
start = omp_get_wtime();

// Fill with a & b with random numbers between 0 and 100
int i;
for(i=0; i<size; i++)
{
  a[i]=rand() % 100;
  b[i]=rand() % 100;
  c[i]=a[i]+b[i];
}

end = omp_get_wtime();

printf("Summed vectors a and b in %g seconds\n", end-start);

}
{% endhighlight %}
omp_vectadd.c - exercises
{:.figure}

### omp_pi.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main (int argc, char *argv[])
{

  //initialize variables
  int i;
  double pi = 0;
  int niter = 100000000;

  // Get timing
  double start,end;
  start=omp_get_wtime();

  // Calculate PI using Leibnitz sum
  for(i = 0; i < niter; i++)
  {
    pi = pi + pow(-1, i) * (4 / (2*((double) i)+1));
  }

  // Stop timing
  end=omp_get_wtime();

  // Print result
  printf("Pi estimate: %.20f, obtained in %f seconds\n", pi, end-start);
}
{% endhighlight %}
omp_pi.c - exercises
{:.figure}

## Solutions

### omp_threadnum.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>

int main (int argc, char *argv[])
{
  printf("Hello World from thread = %d\n", omp_get_thread_num());
}
{% endhighlight %}
omp_threadnum.c - solutions
{:.figure}

### omp_hello.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>

int main (int argc, char *argv[])
{
/* Fork a team of threads */
#pragma omp parallel
  {
  printf("Hello World from thread = %d\n", omp_get_thread_num());
  }  /* All threads join master thread and disband */
}
{% endhighlight %}
omp_hello.c - solutions
{:.figure}

### omp_private.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>

int main (int argc, char *argv[])
{

int tid, nthreads;

/* Fork a team of threads */
#pragma omp parallel private(tid)
  {
  tid = omp_get_thread_num();
  nthreads = omp_get_num_threads();
  printf("Hello World from thread = %d out of %d\n", tid, nthreads);
  }  /* All threads join master thread and disband */
}
{% endhighlight %}
omp_private.c - solutions
{:.figure}

### omp_vectadd.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>
#include <stdlib.h>

int main (int argc, char *argv[])
{

// Initialize arrays 
int size=1000000;
int a[size];
int b[size];
int c[size];

// Time the parallel for-loop
double start,end;
start = omp_get_wtime();

// Fill with a & b with random numbers between 0 and 100
int i;
#pragma omp parallel for
for(i=0; i<size; i++)
{
  a[i]=rand() % 100;
  b[i]=rand() % 100;
  c[i]=a[i]+b[i];
}

end = omp_get_wtime();

printf("Summed vectors a and b in %g seconds\n", end-start);

}
{% endhighlight %}
omp_vectadd.c - solutions
{:.figure}

### omp_pi.c

{% highlight c linenos %}
#include <omp.h>
#include <stdio.h>
#include <stdlib.h>
#include "math.h"

int main (int argc, char *argv[])
{

  //initialize variables
  int i;
  double pi = 0;
  int niter = 100000000;

  // Get timing
  double start,end;
  start=omp_get_wtime();

  // Calculate PI using Leibnitz sum
  /* Fork a team of threads */
#pragma omp parallel for reduction(+ : pi)
  for(i = 0; i < niter; i++)
  {
    pi = pi + pow(-1, i) * (4 / (2*((double) i)+1));
  } /* Reduction operation is done. All threads join master thread and disband */

  // Stop timing
  end=omp_get_wtime();

  // Print result
  printf("Pi estimate: %.20f, obtained in %f seconds\n", pi, end-start);
}
{% endhighlight %}
omp_pi.c - solutions
{:.figure}