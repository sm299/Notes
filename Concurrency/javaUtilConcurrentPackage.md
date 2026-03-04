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



**ExecutorService->**
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

Without ExecutorService:

You create too many threads

System crashes (OutOfMemoryError)

Context switching overhead

Hard to manage lifecycle
