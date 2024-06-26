

## Multitasking:
- Process-based multitasking: Allow multiple processes to run simultaneously on single memory 
- Expensive when multiple context switching happens
- Thread-based multitasking: Multiple Threads run within a single process simultaneously on the same memory location.
- Lightweight, context switch done within a single process or at the same memory location

## Basic Thread Concepts
- Reference: https://www.youtube.com/watch?v=TpYIcJN9EV8
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

**DeadLock**: 
- The thread is waiting for the object lock which is acquired by another thread.
- Two or more threads are blocked forever, waiting for each other.
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

**Data Concurrency:**
- Multiple threads access and modify common data which can lead to data inconsistency / Race condition.

**Race condition:**
- Case 1: Condition in which two or more threads compete/Race together to get shared resources.
- Case 2 or example: Thread 1 tries to read from the linked list and Thread 2 tries to delete same data which leads to a race condition and results in a run time error. 

## Thread Code level

### Thread Methods

| Methods | Description |
|---|---|
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
**1. Extending Thread Class**
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
**2. Using Runnable Interface & lamda exp**
```
public class ThreadUsingRunnable implements Runnable
{
    public static void main( String[] args )
    {
        // Using runnable
        Thread t1 = new Thread(new ThreadUsingRunnable());
        t1.start(); 
        
    	// Using lamda exp
        Runnable r = () -> {
        	System.out.println("Running Thread using Lamda");
        };
        Thread t2 = new Thread(r);
        t2.start();
    }
	@Override
	public void run() {
		System.out.println("Running Thread by extending runnable");	
	}
}
```
**3. Using Lymda Expression**
```
// before Lymda expression 
Runnable runnable = new Runnable() {
	@Override
	public void run() {
		System.out.println("Thread started");
	}
};
Thread t1 = new Thread(	
	runnable
);
t1.start();
	
// Remove boiler of code
Runnable r = () -> { System.out.println("Thread started"); }; // Lymda expression, pass it to Thread
Thread t2 = new Thread( r );
	
// Final
Thread t3 = new Thread(()-> System.out.println("Thread Started"));
```

### Thread Pool . 
- Thread pool manages the pool of worker thread. which are the pulled and assigned task by Service provider.
1. newFixedThreadPool(int size) : Created thread pool of fixed size
2. 2. newCachedThreadPool() : Create new thread pool that create new threads when needed but still use the previosuly created thread they are available use.

ThreadPool container where a fixed size thread are created and which are pull and assigned by 
- fixed size thread pool is created and thread from the thread pool is pulled. assigned a job by the Service provider.

Example:
```
ExecutorService service = Executors.newFixedThreadPool(10);
for(int i=0;i <10; i ++) {
	Thread t = new Thread(() -> System.out.println("Thread Started : " + Thread.currentThread().getName()) );
	t.setName("Thread " + i);	
	service.submit(t);
}
while(!service.isTerminated()) {
	service.shutdown();
}
```

## Callable & Future
- It can be used for Case, Response/Return some result from Thread.
- It will Return Future<T> Instance. In short, it will start Thread and Execute the next code after the Future statement. When we try to get a result from the Future object. Then it will block the main Thread until execution from callable is finished.
- Future<T>: It contains a value which will be arrived in future.
- We can set a timeout for the Future. Ie. If a specific Task is not complete in the provided time. It will through Exceptions and move to the next Operations. Ex: future.get(1, TimeUnit.SECOND);

```
ExecutorService service = Executors.newFixedThreadPool(2);
Future<String> future = service.submit(new CallableThread());
// .. executor other/unrelated operation
String result = future.get(); // here it will block the main Thread until executor from Callable.call() completes.

Class CallableThread implements Callable<String>{
	public String call(){
		return “Test String”;
	}
}
```

## Use of Blocking Queue


## Thread Scheduler
- The scheduler decides which thread to execute and which thread to wait. 
- It decides based on 
	- Priority: Each Thread has a priority between 1 to 10. (1 > 10)
	- Time of arrival: If two threads with the same priority. Then arrival time will be considered.

## Daemon Thread
It's a background running thread with low priority.
When the user/main thread completes. This Thread also gets destroyed automatically
Example: GC
```
t1.setDaemon(true); // t1 Thread will become Daemon Thread
Thread.currentThread().isDaemon(); // return true if Daemon Thread
```

## Mutex & Semaphore
Semaphore & Mutex focus on how Thread access the Resources. and Restrict them.

| Semaphore | Mutex | ReentranLock (Example of Mutex) |
|---|---|---|
| Restrict the number (Declare count) of threads that can be accessed to resource | Only one Thread can be accessed | Type of Mutex |
| Ex: Limit max 10 connections to access a file simultaneously | Ex: Only 1 Thread can access file at time |  |
| Non-ownership-release, Any Thread can release lock | No-ownship-release | Only owner can release the lock, Thread which has created lock can release it |
| Semaphore sp = new Semaphore(10) | Semaphore mtx = new Semaphore(1) / ReentranLock / Synchronize | Lock lock = new ReentranLock() | 
| sp.aquire() , sp.release(), sp.availablePermits() | | lock.lock(), lock.unlock() |

> [!NOTE] If there is no special case like DB Transaction or any such operations we can go Sychronized instead of Semaphore. Synchronized block will take care of lock & release.

- Example of Semaphore & Mutex (using semaphore)
```
public final static Semaphore semaphore = new Semaphore(2); // use '1' in case of mutex
...
public class MyThread extends Thread{
	String name;
	void MyThread(String name){
		this.name = name;
	}

	void run(){
		semaphore.aquire();
		try{
			System.out.println(name +" Thread has started, waiting count "+semaphore.availablePermits());
			Thread.sleep(2000);
		}catch(Exception e){
		}finally{
		semaphore.release();
		// Print - semaphore.availablePermits()
		}	
	}
}
public static void main(String[] args){
	MyThread t1=new MyThread("A");
	t1.start();
	MyThread t2=new MyThread("B");
	t2.start();
	MyThread t3=new MyThread("B"); // It will be waiting for semaphore
	t3.start();
}
```

- Example of Reentrantlock
```
private final Lock lock = new Reentrantlock(true);
...
public void processRequest(){
	lock.lock();
	try{
		// Code
	}catch(){
	}finally{
	lock.unlock();
	}
}
```

## Producer - Consumer - Program
```
import java.util.LinkedList;

class ProducerConsumer  {
  public static void main(String[] args) throws InterruptedException{
    final PC pc = new PC();
   
      Thread producer = new Thread(() -> {
        try{
        	pc.produce();
        }catch(Exception e){
        }
      });

      Thread consumer = new Thread(() -> {
        try{
        	pc.consume();
        }catch(Exception e){
          
        }
      });
      producer.start();
      consumer.start();

      producer.join();
      consumer.join();
  }
}
class PC{
  int capacity = 2;
  LinkedList<Integer> list = new LinkedList<Integer>();
  
  public void produce() throws InterruptedException{
    int value = 0;
    while(true){
      synchronized(this){
        if(list.size() == capacity){
          wait(); 
        }
        value++;
        list.add(value);
        notify();
        
        System.out.println(Thread.currentThread().getName()+ " - Producre - has added value " + value);
        Thread.sleep(1000); 
      }
    } 
  }
  public void consume() throws InterruptedException{
    while(true){
      synchronized(this){ 
        if(list.size() == 0){
          wait();
        }
        int value = list.removeFirst();
        notify();
        
        System.out.println(Thread.currentThread().getName()+ " - Consumer - has revomed value " + value);
        Thread.sleep(1000);
      }   
    }
  } 
}
```

## Thread DeadLock Case & Solutions 
```
class DeadLockExClass {
  public static void main(String[] args) throws Exception {
    
    String resource1 = "Resource 1";
    String resource2 = "Resource 2";
    
    Thread t1 = new Thread(new ThreadClass(resource1,resource2),"Thread 1");
    Thread t2 = new Thread(new ThreadClass(resource2,resource1),"Thread 2");
    t1.start();
   // t1.join(); // to avoid dead Lock
    t2.start();
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
		//Thread.sleep(2000);
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
      
     // try{ Thread.sleep(2000);    }catch(Exception e){ e.printStackTrace();}
      
      
      System.out.println(Thread.currentThread().getName() + " waiting for resource " + resource2 + " to unlock");
      
      synchronized(resource2){
        
        System.out.println("Resource 2 inside Resource 1 accessed by " + Thread.currentThread().getName());
      }   
    } 
  }  
}

```



## Interview
- Can we start Thread twice?: No, If we do then it will throw ‘IllegalThreadStateException’
- Call run() directly: Considered a normal object method
