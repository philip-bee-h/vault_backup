---
title: "mpi-hello-c-example"
created: "2025-02-12"
course:
  - Course name/code
resource:
  - 
type: coursework
tags:
  - coursework
  - school
  - uiowa
  - active
---

> Prentice  [9:40 AM](https://uiowa-rs.slack.com/archives/C089RHZ5NUX/p1739374822036219)  
> 
> Here's my personal mpi_hello.c, which uses MPI messaging (MPI_Send, MPI_Recv)  to send the hello messages from the other ranks to rank zero and then prints the messages out in order. Definitely a bit more work to do this:

```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include "mpi.h"

int main(int argc, char* argv[]){
  int my_rank;
  int p;
  int source;
  int dest;
  int tag=0;
  char message[100];
  char my_name[40];
  MPI_Status status;

  /* Start up MPI */
  MPI_Init(&argc, &argv);

  /* Find out process rank */
  MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);

  /* Find out number of processes */
  MPI_Comm_size(MPI_COMM_WORLD, &p);

  /* What's my hostname? */
  gethostname(my_name, 40);
  /* sleep(10); */

  if (my_rank == 0) {
    printf("MPIHello running on %i processors.\n", p);
    printf("Greetings from processor %i, on host %s.\n", my_rank, my_name);
    for (source=1; source<p; source++) {
      MPI_Recv(message, 100, MPI_CHAR, source, tag, MPI_COMM_WORLD, &status);
      printf("%s", message);
    }
  } else if (my_rank != 0) {
    sprintf(message, "Greetings from processor %i, on host %s.\n", my_rank, my_name);
    dest=0;
    MPI_Send(message, strlen(message)+1, MPI_CHAR, dest, tag, MPI_COMM_WORLD);
  }
  /* sleep(10); */
  MPI_Finalize();
}
```





