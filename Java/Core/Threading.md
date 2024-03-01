Link: https://www.youtube.com/watch?v=TpYIcJN9EV8

## Multitasking:
Process-based multitasking: Allow multiple processes to run simultaneously on single memory 
Expensive when multiple context switching happens
Thread-based multitasking: Multiple Threads run within a single process simultaneously on the same memory location.
Lightweight, context switch done within a single process or at the same memory location

## Basic Thread Concepts
- Process: Whenever we try to execute the program, a process is created and then multiple threads will be created.

- Context switching: When Process/Thread switches to another Process/Thread. While switching to another Process/Thread, the Current process/Thread is Paused and a new memory location/pointer is loaded into the CPU of the new process.

- Thread: 
  - Process created with a single Thread which is the ‘main Thread’. Then multiple threads are created and run simultaneously.
  - Whenever a new process is created and a new instance of JVM is created and allocated to that process.

### Thread block
> Each process with a separate JVM instance

Process 1 . . .
  - JVM Instance
    - Heap Memory
    - Code Segment & Data Segment
    - [ Thread 1: Register, Stack, Counter ] . . .

> CPU Structure
 [ CPU -> Register -> Cahche ] . .  -> Main Memory (RAM)


> Explaining each block
- Code segment: Byte Code, Data Segment: Global variables & Static 
- Heap: Object created with ‘new’
- Thread specific blocks
  - Stack: Local Variable, 
  - Register: Intermediate Code/Value,
  - Used in context switching
  - Counter- Points to address in Code Segment to which address to execute.
    
> Set Heap size: Heap size for New JVM instance for a single process
- java -Xms256m -Xmx5g MainClassName
  - Minimum: Xms
  - Maximum: Xmx

### Flow

- Main.java
  - Thread 1
  - Thread 2

1. Javac Main.java - Comiles and convert to ByteCode
2. Java Main - when execution starts
    - Process start and JVM instance created
    - The code segment will be loaded with Byte co
3. Create 3 threads: Main Thread, Thread 1, Thread 3
    - Allocate register, Stack, counter
    - Counter of Each Thread points to code which is going to execute from the Code segment.
4.  CPU will schedule: Load ByteCode from Code segment into CPU.register.
      1. Single CPU: CPU has a specific time to execute to T1. When time ends. It will do context switching. When time is completed for T1 the CPU will load a new Code of another thread in the register.
      2. For Multiple CPUs: context switching is not required it will execute on different threads.



## Challenges
> Concurrency issues like DeadLock, Data Inconsistency, Race condition 

DeadLock: 
The thread is waiting for the object lock which is acquired by another thread.
Two or more threads are blocked forever, waiting for each other.
```
class DeadLockExClass {
  public static void main(String[] args) throws Exception {
    String resource1 = "Resource 1";
    String resource2 = "Resource 2";
    Thread t1 = new Thread(new ThreadClass(resource1,resource2),"Thread 1");
    Thread t2 = new Thread(new ThreadClass(resource2,resource1),"Thread 2");
    t1.start();
   // t1.join(); // to avoid dead Lock
    t2.start(); // Thread dumb
  }
  public static class ThreadClass implements Runnable{
    String resource1;
    String resource2;
    public ThreadClass(String resource1,String resource2){
      this.resource1 = resource1;
      this.resource2 = resource2;
    }
    public void run(){
      try{
     	SharedClass shared = new SharedClass();
        	shared.sharedMethod(resource1,resource2);
      }catch(Exception e){
        e.printStackTrace();
      }
    }
  }  
}
class SharedClass {
  public SharedClass(){
  }
  public void sharedMethod(String resource1,String resource2){ 
    synchronized(resource1){
      System.out.println("Resource "+resource1+" is locked by " + Thread.currentThread().getName());
      System.out.println(Thread.currentThread().getName() + " waiting for resource " + resource2 + " to unlock");
      synchronized(resource2){
        System.out.println("Resource 2 inside Resource 1 accessed by " + Thread.currentThread().getName());
      }
    } 
  }
}
```

Data Concurrency:
Multiple threads access and modify common data which can lead to data inconsistency / Race condition.

Race condition:
Case 1: Condition in which two or more threads compete/Race together to get shared resources.
Case 2 or example: Thread 1 tries to read from the linked list and Thread 2 tries to delete same data which leads to a race condition and results in a run time error. 



## Thread Code level

### Thread Methods

| Methods | Descriptio |
| —- | —- |
| t1.start() | start Thread | 
| t1.join() | Make a current thread to wait for another thread to finish |
| Thread.sleep(10) | wait for specified seconds |
| Thread.currentThread() | When executed from inside thread, return ref of current thread |
| t1.toString() | String representation of current thread |
| notify() | used to wake up a single Thread. If multiple Threads are waiting, It will notify only one thread to wake up |
| notifyall() | used to wake up all threads who are in a waiting state | 
| Thread.holdsLock(object/this) | Return true if current object holds lock |

### Life Cycle of Thread
1. New (Thread.State NEW)
2. Active - (RUNNABLE) Running
3. Blocked/ waiting - Locked or waiting (BLOCKED / WAITING)
4. Timed waiting (TIMED_WAITING)- Thread lies in a waiting state for a specific time instead of forever
5. Terminated (TERMINATED)- finish job, or exception.

### Create Thread
1. Extending Thread Class
```
Public class ThreadClass extends Thread{
	public static void main(String args[]){
		ThreadClass t1 = new ThreadClass();
		t1.start();
	}
	public void run(){
		System.out.println(“Thread running”);
	}
}
```
2. Using Runnable Interface & lamda exp
```
public class App implements Runnable
{
    public static void main( String[] args )
    {
        // Using runnable
        Thread t2 = new Thread(new App());
        t2.start(); 
        
    	// Using lamda exp
        Runnable r1 = () -> {
        	System.out.println("Running Thread using Lamda");
        };
        Thread t1 = new Thread(r1);
        t1.start();
    }
	@Override
	public void run() {
		System.out.println("Running Thread by extending runnable");	
	}
}
```

### Thread Pool . 
- Thread pool manages the pool of worker thread. which are the pulled and assigned task by Service provider.
1. newFixedThreadPool(int size) : Created thread pool of fixed size
2. 2. newCachedThreadPool() : Create new thread pool that create new threads when needed but still use the previosuly created thread they are available use.

ThreadPool container where a fixed size thread are created and which are pull and assigned by 
- fixed size thread pool is created and thread from the thread pool is pulled. assigned a job by the Service provider.



## Thread Scheduler
The scheduler decides which thread to execute and which thread to wait. 
It decides based on 
Priority: Each Thread has a priority between 1 to 10. (1 > 10)
Time of arrival: If two threads with the same priority. Then arrival time will be considered.

## Daemon Thread
It's a background running thread with low priority.
When the user/main thread completes. This Thread also gets destroyed automatically
Example: GC
```
t1.setDaemon(true); // t1 Thread will become Daemon Thread
Thread.currentThread().isDaemon(); // return true if Daemon Thread
```

## Interview
Can we start Thread twice?: No, If we do then it will throw ‘IllegalThreadStateException’
Call run() directly: Considered a normal object method
