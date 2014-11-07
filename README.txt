                README
Minix OS Semaphore Service				
By: Manuel Perez

This patch is an implementation of a semaphore service into Minix O.S.
This service can produce an unlimited number of semaphores and can be
called by the user through system calls.

////////////////////////////
Semaphore Service Main:
///////////////////////////
First a new service was introduced by creating the sem folder and
the main file in servers/sem. Changes to other files in the system
were necessary to avoid booting errors. this service was the
semaphore service. The service contains an infinite loop where it is
always listening for message from the Kernel and once it receives
one it runs an operation according to the request:

SEM_INIT: do_init() - Creates a new semaphore
cSEM_UP: do_up() - Increment Semaphore Count
SEM_DOWN: do_down() - Decrease Semaphore Count
SEM_RELEASE: do_release() - Deletes Semaphore

///////////////////////////
Hash Table
//////////////////////////

In order for the semaphores to run at constant time a hash table was
used to store the semaphores. This can be found in servers/sem/hash.c.
The hash table is a chained hash table that dynamically grows in size
as more semaphores are added.

initializeHashTable()- Creates the hash table

addValue() - Adds a new semaphore into the hash table

hashTableFind() - Finds a specific semaphore

hashTableRemove() - Deletes a semaphore from the hash table

resize() - DOubles the size of the hash table when the number of
semaphores grows

nextPrime() - Private function determines the next prime that is at
least double the current size of the hash table.

enqueue() - Internal function to add node

dequeue() - Internal function to remove node

//////////////////
System Calls
/////////////////
In order for the User to access these semaphores system calls
that give control to the Kernel are needed. These system calls can
be found in lib/libc/sys-minix/sem2.c as:

int sem_init(int);
int sem_down(int);
int sem_up(int);
int sem_release(int);


