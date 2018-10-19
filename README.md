#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

int A[1000];
int chunk_size = 100;

void * Sum (void *n)
{
  int number = (int)n;
  int _size = number + chunk_size;
  int sum = 0;
  for(int i = number; i < _size; i++)
	sum = sum + A[i];
  return ((void *) sum);
}

int main()
{
  for (int i = 0; i < 1000; i++)
  {
      A[i] = i;
  }

  pthread_t B[10];
  for (int j = 0; j < 10; j++)
  {
      pthread_create (&B[j], NULL, Sum, (void *) (j * chunk_size));
  }

  int _s[10];
  for (int k = 0; k < 10; k++)
  {
      pthread_join (B[k], (void **) &_s[k]);
  }

  int sum = 0;
  for (int l = 0; l < 10; l++)
  {
    sum = sum + _s[l];
  }

  printf ("%d ", sum);

  return 0;
}

