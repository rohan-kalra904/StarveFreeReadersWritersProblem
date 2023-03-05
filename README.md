## This is the PSEUDOCODE for  starve free Readers-Writers problem
### Initialization of semaphores

```

semaphore entry=1; //semaphore for entry common to both readers and writers

semaphore rw_mutex=1; // semaphore for writer

semaphore rd_mutex=1; //  semaphore for reader

int reader_count=0; // semaphore to mantain the number of readers currently in critical section

```
### Intuition for readers code

In this solution apart from the usual mutex semaphores there is entry binary semaphore this is required before rw_mutex is accessed.

```
wait(entry); //only execute if it's turn

wait(rd_mutex); //so that only 1 reader can access reader_count

reader_count++; // to mantain the number of readers currently reading

if(reader_count==1)
{
    wait(rw_mutex); // To not let the writers enter their critical section as readers are reading
}

signal rd_mutex;



/* Reading process */


wait(rd_mutex);

reader_count--;

if(reader_count==0)
{
    reader_count--;
}

signal(rd_mutex);


```

### Intuition for writers code

Again the entry semaphore is used to allow entry into the writers code. This is before the rw_mutex is checked and it is released before the writing process commences.

```
wait(entry); //The writer has to wait for the entry semaphore it can get access to it even when readers are reading

wait(rw_mutex); // This is to make sure that the writer does not write when readers are reading

signal(entry);// So that the readers or writers can start queing when the writing process is being performed
 
 /*
 Writing Process
 */
 signal(rw_mutex)

```
### Why this works?

Due to the entry mutex when a writer enters when multiple readers are still reading it would lock the entry semaphore and due to this the readers that enter after the writer would not be able to execute even though multiple readers are still reading. The writing process would be performed after the readers that entered before have finished reading. So, clearly none of the readers or writers would starve.




