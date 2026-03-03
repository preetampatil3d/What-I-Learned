

## Multitasking:
- Process-based multitasking: Allow multiple processes to run simultaneously on single memory 
- Expensive when multiple context switching happens
- Thread-based multitasking: Multiple Threads run within a single process simultaneously on the same memory location.
- Lightweight, context switch done within a single process or at the same memory location

## Basic Thread Concepts
- Reference: https://www.youtube.com/watch?v=TpYIcJN9EV8
- Process: Whenever we try to execute the program, a process is created and then multiple threads will be created.
- Context switching: When a process/Thread switches to another Process/Thread. While switching to another Process/Thread, the Current process/Thread is Paused and a new memory location/pointer is loaded into the CPU of the new process.
- CPU Scheduling: CPU will schedule the thread to execute based on the priority and time of arrival. It will load the code from the code segment into the register and execute it.

- Thread: 
  - Process created with a single Thread which is the ‘main Thread’. Then multiple threads are created and run simultaneously.
  - Whenever a new process is created, a new instance of the JVM is created and allocated to that process.

- 

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
- Thread-specific blocks
  - Stack: Local Variable, 
  - Register: Intermediate Code/Value,
  - Used in context switching
  - Counter- Points to address in Code Segment to which address to execute.
- The code segment and data segment are shared by all threads, but each thread has its own stack, register, and counter.

> Volatile Keyword
- Use volatile keyword for a shared variable to make sure all threads see the latest value of the variable. It will prevent caching of the variable and force all threads to read the value from the main memory.
- When a thread reads a value, CPU caches it in its local cahe/register for faster access. without volatile, each thread maintain its own caches copy of variable. So, if one thread updates the variable, other threads may not see the updated value and can lead to data inconsistency. With volatile, all threads will read the value from main memory and see the latest value.

> Set Heap size: Heap size for new JVM instance for a single process
- java -Xms256m -Xmx5g MainClassName
  - Minimum: Xms
  - Maximum: Xmx

### Flow

- Main.java
  - Thread 1
  - Thread 2

1. Javac Main.java - Compiles and converts to ByteCode
2. Java Main - when execution starts
    - Process start and JVM instance created
    - The code segment will be loaded with bytecode
3. Create 3 threads: Main Thread, Thread 1, Thread 3
    - Allocate register, Stack, counter
    - Counter of Each Thread points to the code which is going to execute from the Code segment.
4.  CPU will schedule: Load ByteCode from Code segment into CPU.register.
      1. Single CPU: CPU has a specific time to execute to T1. When time ends. It will do context switching. When time is completed for T1 the CPU will load a new Code of another thread in the register.
      2. For Multiple CPUs: context switching is not required; it will execute on different threads.


## Virtual Thread
<img width="646" height="618" alt="Screenshot 2026-03-03 at 11 03 22 PM" src="https://github.com/user-attachments/assets/32eff81d-2156-48ef-8db7-f65c86bbea7f" />


- Virtual Thread is a lightweight thread that is managed by the Java Virtual Machine (JVM) rather than the operating system. It allows for a large number of concurrent threads without the overhead associated with traditional threads. Virtual threads are designed to be more efficient and scalable, making them ideal for applications that require high concurrency, such as web servers or real-time systems.
- Use case:
  - High-concurrancy I/O Operations (HTTP requests, DB calls)
  - Real-time Systems like Financial Trading
```
 Runnable virtualThread = () -> {
              System.out.println("Running using Virtual Thread" +
                      "isDaemon:"+Thread.currentThread().isDaemon() + 
                      "isVertual"+Thread.currentThread().isVirtual() );
            };
            Thread.startVirtualThread(virtualThread).join();
```

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
- Multiple threads access and modify common data, which can lead to data inconsistency / Race condition.

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
**2. Using Runnable Interface & lambda expression**
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

### Thread Pool. 
- Thread pool manages the pool of worker threads. Which are the pulled and assigned task by service provider.
- Thread pool designed to reuse threads for multiple tasks.

1. newFixedThreadPool(int size): Created a thread pool of fixed size
   - ThreadPool container where a fixed size threads are created and which are pull and assigned by the Service provider.
   - Use case: For long-lived tasks, where the number of threads required is known in advance. It can be used for tasks that are executed synchronously and require a fixed number of threads.
   
2. newCachedThreadPool(): Create a new thread pool that creates new threads when needed, but still uses the previously created threads that are available for use.
    - It will create new threads if there are no available threads in the pool. If there are available threads, it will reuse them. It will also terminate threads that have been idle for 60 seconds.
    - Use case: For short-lived tasks, where the number of threads required is not known in advance. It can be used for tasks that are executed asynchronously and do not require a fixed number of threads.

- Note: 
  - We should always shutdown the thread pool after use. Otherwise, it will keep running and consume resources. It will not allow the JVM to exit until all threads are finished. So, we need to call shutdown() method to shutdown the thread pool.
  - Never share a single connection between multiple threads.
  - CPU-bound tasks: Pool size = number of CPU cores
    I/O-bound tasks: Pool size > CPU cores (larger pool beneficial)

| Aspect | Normal Thread | Executor Service |
|--------|---------------|------------------|
| Auto-terminates after task | Yes | No |
| Reuses threads | No | Yes |
| JVM exit after task | Yes (if main completes) | No (threads stay alive) |
| Resource management | Manual | Pooled (must shutdown) |

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
- It is a thread-safe queue that supports operations that wait for the queue to become non-empty when retrieving an element, and wait for space to become available in the queue when storing an element.
- It is used in Producer-Consumer problems where one thread is producing data and another thread is consuming data. The producer thread will put data into the queue and the consumer thread will take data from the queue. If the queue is full, the producer thread will wait until there is space in the queue. If the queue is empty, the consumer thread will wait until there is data in the queue
- Example:
```
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10); // capacity of 10
// Producer Thread
new Thread(() -> {
    for(int i=0; i<20; i++) {
        try {
            queue.put(i); // will block if queue is full
            System.out.println("Produced: " + i);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}).start(); 
// Consumer Thread
new Thread(() -> {
    for(int i=0; i<20; i++) {
        try {
            int value = queue.take(); // will block if queue is empty   
            System.out.println("Consumed: " + value);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}).start();
```


## Thread Scheduler
- The scheduler decides which thread to execute and which thread to wait. 
- It decides based on 
	- Priority: Each Thread has a priority between 1 to 10. (1 > 10)
	- Time of arrival: If two threads with the same priority. Then arrival time will be considered.
Example:

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


## FAQs
- Can we start the thread twice?: No, if we do, then it will throw ‘IllegalThreadStateException’
- Call run() directly: It will be considered as a normal object method.
- How to avoid deadlock?
1. Avoid Nested Locks: Avoid acquiring multiple locks at the same time. If you need to acquire multiple locks, make sure to acquire them in a consistent order across all threads.
2. Use a Timeout: When acquiring locks, use a timeout to prevent threads from waiting indefinitely. If a thread cannot acquire a lock within the specified time, it can release any locks it holds and try again later.
3. Use a Single Lock: Instead of using multiple locks, consider using a single lock to protect all shared resources. This can simplify the locking mechanism and reduce the chances of deadlock.
4. Use a Deadlock Detection Algorithm: Implement a deadlock detection algorithm that periodically checks for deadlocks and takes appropriate action, such as terminating one of the threads involved in the deadlock.
5. Avoid Circular Wait: Ensure that there is no circular wait condition, where each thread is waiting for a resource that is held by another thread. This can be achieved by enforcing a strict ordering of resource acquisition.
6. Use Higher-Level Concurrency Utilities: Instead of using low-level locks, consider using higher-level concurrency utilities provided by the Java Concurrency API, such as `java.util.concurrent` package, which provides thread-safe data structures and synchronization mechanisms that can help avoid deadlock situations.
- Difference between wait() and sleep()?
  - wait(): It is a method of the Object class and is used to make the current thread wait until another thread calls notify() or notifyAll() on the same object. It releases the lock on the object and allows other threads to acquire it. It is typically used in a synchronized block or method to coordinate the execution of threads.
  - sleep(): It is a static method of the Thread class and is used to pause the execution of the current thread for a specified amount of time. It does not release any locks and does not allow other threads to acquire them. It is typically used to introduce a delay in the execution of a thread or to simulate a time-consuming task.
- Difference between notify() and notifyAll()?
  - notify(): It is a method of the Object class and is used to wake up a single thread that is waiting on the same object. If multiple threads are waiting, it will wake up one of them, but it does not guarantee which thread will be woken up. It is typically used when you want to signal a specific thread to wake up and continue its execution.
  - notifyAll(): It is also a method of the Object class and is used to wake up all threads that are waiting on the same object. It is typically used when you want to signal all waiting threads to wake up and continue their execution. It is important to note that both notify() and notifyAll() must be called from within a synchronized block or method that locks the same object on which the threads are waiting. Otherwise, it will throw an IllegalMonitorStateException.
- Difference between wait() and join()?
  - wait(): It is a method of the Object class that causes the current thread to wait until another thread calls notify() or notifyAll() on the same object. It is typically used in a synchronized block or method to coordinate the execution of threads. When a thread calls wait(), it releases the lock on the object and allows other threads to acquire it.
  - join(): It is a method of the Thread class that allows one thread to wait for the completion of another thread. When a thread calls join() on another thread, it will block until the other thread has finished executing. It is typically used to ensure that a thread has completed its task before allowing another thread to proceed with its execution. Unlike wait(), join() does not release any locks and does not allow other threads to acquire them while waiting for the specified thread to complete.
- Difference between sleep() and yield()?
  - sleep(): It is a static method of the Thread class that causes the current thread to pause its execution for a specified amount of time. It does not release any locks and does not allow other threads to acquire them. It is typically used to introduce a delay in the execution of a thread or to simulate a time-consuming task.
  - yield(): It is a static method of the Thread class that causes the current thread to pause its execution and allow other threads of the same priority to execute. It does not guarantee that the current thread will be paused or that other threads will be scheduled to run. It is typically used to give other threads a chance to execute when the current thread is performing a long-running task, but it should be used with caution as it can lead to unpredictable behavior and is generally not recommended for use in production code.
- Difference between Callable and Runnable?
    - Callable: It is an interface from the java.util.concurrent package that represents a task that can return a result and may throw an exception. It has a single method, call(), which is similar to the run() method of the Runnable interface, but it can return a value and throw checked exceptions. Callable tasks are typically executed using an ExecutorService, which allows you to submit Callable tasks and retrieve their results using Future objects.
        - Runnable: It is a functional interface that represents a task that can be executed by a thread. It has a single method, run(), which does not return a value and cannot throw checked exceptions. Runnable tasks are typically executed by creating a Thread object and passing the Runnable instance to its constructor.
- Difference between Thread and ExecutorService?
    - Thread: It is a class that represents a thread of execution in a Java program. You can create a new thread by extending the Thread class or implementing the Runnable interface. Each thread runs independently and can be started, paused, or stopped using various methods provided by the Thread class.
    - ExecutorService: It is an interface from the java.util.concurrent package that provides a higher-level abstraction for managing and executing threads. It allows you to submit tasks for execution and manage a pool of threads to execute those tasks. ExecutorService provides methods for submitting tasks, shutting down the service, and retrieving results from tasks executed by the service. It is generally recommended to use ExecutorService for managing threads in modern Java applications, as it provides better performance and scalability compared to manually managing threads using the Thread class.
- Difference between ReentrantLock and Synchronized block?
  - ReentrantLock: It is a class from the java.util.concurrent.locks package that provides an explicit lock implementation. It offers more flexibility and features compared to synchronized blocks, such as the ability to interrupt threads waiting for the lock, try to acquire the lock without blocking, and specify a fairness policy for thread scheduling. ReentrantLock also allows for multiple locks to be held by the same thread, which can be useful in certain scenarios.
  - Synchronized block: It is a built-in language feature in Java that provides a simple way to synchronize access to a shared resource. It is less flexible than ReentrantLock and does not offer the same level of control over thread scheduling and locking behavior. Synchronized blocks are easier to use and are generally sufficient for most synchronization needs, but ReentrantLock can be a better choice in situations where more advanced locking features are required.
  - In summary, ReentrantLock provides more advanced features and flexibility for thread synchronization, while synchronized blocks are simpler and easier to use for basic synchronization needs. The choice between the two depends on the specific requirements of your application and the level of control you need over thread synchronization.
