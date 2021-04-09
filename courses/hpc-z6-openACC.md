---
layout: page
title: Z6-openACC
description: >
  High-performance computing (HPC)
hide_description: true

---

{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

---

## Setup

### Installation

~~~sh
# file: 'terminal'
apt-get install libopenmpi-dev 
apt-get install openmpi-bin 
~~~

### Compilation & Running OpenMP
~~~sh
# file: 'terminal'
gcc -o name name.c -fopenmp
./name
~~~

### Compilation & Running MPI

~~~sh
# file: 'terminal'
mpicc filename.c -o filename 
mpirun -np 1 ./filename # -lm
~~~

### Compilation & Running OpenACC

~~~sh
# file: 'terminal'
gcc -o izvrsna_dat izvorna_dat.c -fopenacc
./izvrsna_dat
~~~

## Primeri 

### ptraliasing.c

~~~c
// file: 'ptraliasing.c'
void assign(int *a, int *b, int size) {
    #pragma acc kernels
    {
        for (int i = 0; i < size - 1; i++)
            a[i] = b[i + 1];
    }
}

int main() {
    return 0;
}
~~~
ptraliasing.c
{:.figure}

### parallel.c

~~~c
// file: 'parallel.c'
#include <stdio.h>
#include <stdlib.h>

#ifdef _OPENACC
#include <openacc.h>
#endif

int main() {

	float *values = (float *) malloc(sizeof(float) * size);

	#pragma acc parallel
	for (int i = 0; i < 1024; i++)
		values[i] = 1.f;

	free(values);

	return 0;
}
~~~
parallel.c
{:.figure}

### parallelloop.c

~~~c
// file: 'parallelloop.c'
#include <stdlib.h>
#include <openacc.h>

#define N 1024
#define M 1024

int main() {

  float *A = (float *) malloc(N * M * sizeof(float));

  #pragma acc parallel loop gang
  for (int i=0; i<N; i++)
      #pragma acc loop vector
      for (int j=0; j<M; j++)
        A[i * N + j] = 1.f;
    
  free(A);

  return 0;
}
~~~
parallelloop.c
{:.figure}

### data.c

~~~c
// file: 'data.c'
#include <stdlib.h>

#define N 1024

int main() {

  float *x = (float *) malloc(sizeof(float) * N);
  float *y = (float *) malloc(sizeof(float) * N);

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

  free(x); free(y);

  return 0;
}
~~~
data.c
{:.figure}