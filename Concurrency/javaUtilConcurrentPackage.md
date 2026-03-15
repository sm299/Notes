# Main Components 
**Executor**<br>
**ExecutorService**<br>
**ScheduledExecutorService**<br>

**Future**<br>
**CountDownLatch**<br>
**CyclicBarrier**<br>
**Semaphore**<br>
**ThreadFactory**<br>
**BlockingQueue**<br>
**DelayQueue**<br>
**Locks**<br>
**Phaser**<br>

**Executor ->**
Executor is an interface that represents an object that executes provided tasks.
Instead of doing 
Thread t = new Thread(() -> {
    System.out.println("Task running");
});
t.start();

with Executor we do
Executor executor = Executors.newSingleThreadExecutor();
executor.execute(() -> {
    System.out.println("Task running");
});

You give the task to the Executor, and it handles:<br>
Creating threads<br>
Reusing threads<br>
Managing lifecycle<br>
Queueing tasks

Creating threads manually is:<br>
Expensive<br>
Hard to manage<br>
Hard to scale<br>
Hard to control<br>
Executor solves this by using thread pools.

👉 Executor only defines how to submit a task<br>
👉 It does NOT guarantee the task runs in a new thread<br>
👉 It may run in the same thread<br>
So Executor ≠ always multi-threading.

A simple executor example:<br>
Executor executor = command -> {
    command.run();   // directly calling run()
};

If current thread only is being used:<br>
executor.execute(() -> {
    System.out.println(Thread.currentThread().getName());
});
That means:<br>
No new thread created<br>
No async execution<br>
Just a method call<br>
This is called synchronous execution.

Executor executor = command -> {
    new Thread(command).start();
};
Now it runs in a new thread.<br>
This is asynchronous.

new Thread(task).start(); //task logic and thread creation logic tightly coupled

executor.execute(task); //task submission and execution mechanism are different

Executor does not strictly require the task execution to be asynchronous->
Means:<br>
Executor is just an interface:<br>
void execute(Runnable command);<br>
It doesn’t say:<br>
Must create new thread<br>
Must run asynchronously<br>
Must use thread pool<br>
Implementation decides.<br>


This Design Is Powerful<br>
Because:<br>
In production → use ThreadPoolExecutor<br>
In testing → use direct executor (same thread)<br>
In debugging → change execution strategy easily<br>
Without changing business logic.<br>


*Does Executor automatically have a thread pool?*
No.<br>
Executor is just an interface:<br>
void execute(Runnable command);<br>
It does NOT create a thread pool automatically.<br>
You must provide an implementation.<br>
2️⃣ Who Provides Thread Pool?<br>
Usually we use:<br>
ExecutorService executor = Executors.newFixedThreadPool(5);<br>
Behind the scenes, this creates a:<br>
👉 ThreadPoolExecutor<br>
So yes — Java provides built-in thread pool implementations, but you must explicitly create them.<br>
Common ones:<br>
Executors.newFixedThreadPool(n)<br>
Executors.newCachedThreadPool()<br>
Executors.newSingleThreadExecutor()<br>
Executors.newScheduledThreadPool(n)<br>

All internally use ThreadPoolExecutor.<br>
3️⃣ If I specify thread pool, how does it know Prod vs Lower env?<br>
It DOESN'T know automatically.<br>
You control it via configuration.<br>
Example:<br>
In application.properties (Spring Boot)<br>
thread.pool.size=5   // lower env<br>
thread.pool.size=50  // prod<br>
Then:<br>
@Value("${thread.pool.size}")<br>
private int poolSize;<br>
@Bean<br>
public ExecutorService executorService() {
    return Executors.newFixedThreadPool(poolSize);
}

Now:<br>
Dev → 5 threads<br>
Prod → 50 threads<br>

Same code.<br>
Different config.<br>
4️⃣ How Enterprise Systems Handle This<br>
In real systems:<br>
Thread pool size depends on:<br>
CPU cores<br>
Traffic load<br>
Memory<br>
Blocking vs Non-blocking tasks<br>
Sometimes we calculate:<br>
For CPU-bound tasks:<br>
Threads = CPU cores<br>
For IO-bound tasks:<br>
Threads = CPU cores × 2
or more<br>
5️⃣ What Actually Happens Internally

When you submit tasks:<br>
Task goes into a queue<br>
If free thread available → executes immediately<br>
If all threads busy → task waits in queue<br>
If queue full → RejectedExecutionHandler triggers<br>
This is managed by ThreadPoolExecutor.<br>

6️⃣ In Production Best Practice
Never use:<br>
Executors.newFixedThreadPool()
Directly in high-scale production.
Instead configure:
new ThreadPoolExecutor(
    corePoolSize,
    maxPoolSize,
    keepAliveTime,
    TimeUnit.SECONDS,
    new LinkedBlockingQueue<>(capacity)
);

Because:
You control queue size<br>
You control rejection policy<br>
Avoid memory leaks<br>



## ExecutorService->
ExecutorService is a complete solution for asynchronous processing. It manages an in-memory queue and schedules submitted tasks based on thread availability.

To use ExecutorService, we need to create one Runnable class.<br>
public class Task implements Runnable {
    @Override
    public void run() {
        // task details
    }
}
Now we can create the ExecutorService instance and assign this task. At the time of creation, we need to specify the thread-pool size.<br>
ExecutorService executor = Executors.newFixedThreadPool(10);

If we want to create a single-threaded ExecutorService instance, we can use newSingleThreadExecutor(ThreadFactory threadFactory) to create the instance.

Once the executor is created, we can use it to submit the task.
public void execute() { 
    executor.submit(new Task()); 
}

We can also create the Runnable instance while submitting the task.<br>
executor.submit(() -> {
    new Task();
});

It also comes with two out-of-the-box execution termination methods. The first one is shutdown(); it waits until all the submitted tasks finish executing. The other method is shutdownNow() which attempts to terminate all actively executing tasks and halts the processing of waiting tasks.

There is also another method awaitTermination(long timeout, TimeUnit unit) which forcefully blocks until all tasks have completed execution after a shutdown event triggered or execution-timeout occurred, or the execution thread itself is interrupted,<br>
try {
    executor.awaitTermination( 20l, TimeUnit.NANOSECONDS );
} catch (InterruptedException e) {
    e.printStackTrace();
}

Without ExecutorService:<br>
You create too many threads<br>
System crashes (OutOfMemoryError)<br>
Context switching overhead<br>
Hard to manage lifecycle<br>

With ExecutorService:<br>
Controlled number of threads<br>
Better performance<br>
Task queue management<br>
Graceful shutdown<br>
Future result handling<br>
A Future represents the result of an asynchronous computation.
Because when you run tasks asynchronously:
You don’t know when they finish<br>
You may need the result later<br>
You may want to cancel<br>
You may want to check status<br>
Future acts like a placeholder for a result that will come later<br>

Qstn->
Okay, so in our project we use transactions, we close it after the main call, then we do that async call, it may or may not complete. so in this scenario how future will help, we are aready out of control, like we gave the response that async call will later update something in db. this is fire-and-forget pattern<br>

In this architecture:

👉 Future often does NOT help.<br>
Because:<br>
You are not waiting for the result<br>
You already returned response<br>
You don’t care immediately whether async succeeds<br>
So using future.get() would defeat the purpose.<br>

When Does Future Actually Help?<br>
Future helps when:<br>
You need the result before proceeding<br>
You want to combine multiple async calls<br>
You want timeout control<br>
You want to cancel the task<br>

DLQ-> Dead Letter Queue is a secondary queue used to store messages that could not be processed successfully after a configured number of retry attempts, allowing failure isolation and operational monitoring without blocking the main processing flow.

## ScheduledExecutorService->

ScheduledExecutorService is a similar interface to ExecutorService, but it can perform tasks periodically.
Executor and ExecutorService‘s methods are scheduled on the spot without introducing any artificial delay. Zero or any negative value signifies that the request needs to be executed instantly.
We can use both Runnable and Callable interface to define the task.<br>
public void execute() {
    ScheduledExecutorService executorService
      = Executors.newSingleThreadScheduledExecutor();

    Future<String> future = executorService.schedule(() -> {
        // ...
        return "Hello world";
    }, 1, TimeUnit.SECONDS);

    ScheduledFuture<?> scheduledFuture = executorService.schedule(() -> {
        // ...
    }, 1, TimeUnit.SECONDS);

    executorService.shutdown();
}


Here, the scheduleAtFixedRate( Runnable command, long initialDelay, long period, TimeUnit unit ) method creates and executes a periodic action that is invoked firstly after the provided initial delay, and subsequently with the given period until the service instance shutdowns.<br>

The scheduleWithFixedDelay( Runnable command, long initialDelay, long delay, TimeUnit unit ) method creates and executes a periodic action that is invoked firstly after the provided initial delay, and repeatedly with the given delay between the termination of the executing one and the invocation of the next one.<br>


ScheduledExecutorService is an extension of ExecutorService that supports delayed and periodic task execution. While both manage thread pools and execute tasks, only ScheduledExecutorService provides time-based scheduling.

## Future->
Future is used to represent the result of an asynchronous operation. It comes with methods for checking if the asynchronous operation is completed or not, getting the computed result, etc.
What’s more, the cancel(boolean mayInterruptIfRunning) API cancels the operation and releases the executing thread. If the value of mayInterruptIfRunning is true, the thread executing the task will be terminated instantly.
Otherwise, in-progress tasks will be allowed to complete.
We can use below code snippet to create a future instance:<br>
public void invoke() {
    ExecutorService executorService = Executors.newFixedThreadPool(10);

    Future<String> future = executorService.submit(() -> {
        // ...
        Thread.sleep(10000l);
        return "Hello world";
    });
}<br>

We can use following code snippet to check if the future result is ready and fetch the data if the computation is done:<br>

if (future.isDone() && !future.isCancelled()) {
    try {
        str = future.get();
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}

We can also specify a timeout for a given operation. If the task takes more than this time, a TimeoutException is thrown:<br>
try {
    future.get(10, TimeUnit.SECONDS);
} catch (InterruptedException | ExecutionException | TimeoutException e) {
    e.printStackTrace();
}

## CountDownLatch->
A CountDownLatch is initialized with a counter(Integer type); this counter decrements as the dependent threads complete execution. But once the counter reaches zero, other threads get released.
*Usage in Concurrent Programming*
CountDownLatch has a counter field, which you can decrement as we require. We can then use it to block a calling thread until it’s been counted down to zero.

If we were doing some parallel processing, we could instantiate the CountDownLatch with the same value for the counter as a number of threads we want to work across. Then, we could just call countdown() after each thread finishes, guaranteeing that a dependent thread calling await() will block until the worker threads are finished.
but it's for one time usage, for reusable synchronization → use CyclicBarrier<br>

## CyclicBarrier->
CyclicBarrier works almost the same as CountDownLatch except that we can reuse it. Unlike CountDownLatch, it allows multiple threads to wait for each other using await() method(known as barrier condition) before invoking the final task.

## Semaphore->
A semaphore is a tool used in concurrent programming to control access to a shared resource.<br>
Think of it like a counter that tells how many threads are allowed to use something at the same time.<br>
Example shared resources:<br>
Database connections<br>
Printer<br>
Limited API connections<br>
Limited thread slots<br>

The initial value of a semaphore depends on the problem at hand. Usually, we use the number of resources available as the initial value.<br>

*Semaphore Operations->*
A semaphore has two indivisible (atomic) operations, namely: wait and signal.<br>

In Semaphore (programming), there are two main operations used to control access to shared resources.<br>
1️⃣ wait() Operation (P Operation)<br>
It decreases the semaphore value by 1.<br>
If the value becomes less than 0, the process must wait until the resource becomes available.<br>
Simple idea:<br>
➡️ A process asks for permission to use the resource.<br>
Example:<br>

wait(S)
{
   S = S - 1
   if (S < 0)
      wait in queue
}<br>
2️⃣ signal() Operation (V Operation)<br>
It increases the semaphore value by 1.<br>
If some processes are waiting, one waiting process is allowed to continue.<br>
Simple idea:<br>
➡️ A process releases the resource so others can use it.<br>
Example:<br>

signal(S)
{
   S = S + 1
   if (S <= 0)
      wake up a waiting process
}<br>


So, here one confusion may come like if S<=0 in signal function then it may not be the case that resources are available as that simply means some threads are waiting. in that case why we are awaking one waiting thread. The short answer is signal function will be called only when a resource is being released.

### Semaphore Types
**Binary Semaphore**
**Counting Semaphore**

### Binary Semaphore->
A binary semaphore is a semaphore that takes only two values (0 and 1) and is used to ensure that only one process accesses a critical section at a time.<br>
The semaphore acts like a lock or switch.<br>
Value    Meaning<br>
    1    Resource is free<br>
    0    Resource is in use (locked)<br>
So only one process can enter the critical section at a time.<br>

*Mutex->*
