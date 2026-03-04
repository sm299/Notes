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

You give the task to the Executor, and it handles:
Creating threads
Reusing threads
Managing lifecycle
Queueing tasks

Creating threads manually is:
Expensive
Hard to manage
Hard to scale
Hard to control
Executor solves this by using thread pools.

👉 Executor only defines how to submit a task
👉 It does NOT guarantee the task runs in a new thread
👉 It may run in the same thread
So Executor ≠ always multi-threading.

A simple executor example:
Executor executor = command -> {
    command.run();   // directly calling run()
};

If current thread only is being used:
executor.execute(() -> {
    System.out.println(Thread.currentThread().getName());
});
That means:
No new thread created
No async execution
Just a method call
This is called synchronous execution.

Executor executor = command -> {
    new Thread(command).start();
};
Now it runs in a new thread.
This is asynchronous.

new Thread(task).start(); //task logic and thread creation logic tightly coupled

executor.execute(task); //task submission and execution mechanism are different

Executor does not strictly require the task execution to be asynchronous->
Means:
Executor is just an interface:
void execute(Runnable command);
It doesn’t say:
Must create new thread
Must run asynchronously
Must use thread pool
Implementation decides.


This Design Is Powerful
Because:
In production → use ThreadPoolExecutor
In testing → use direct executor (same thread)
In debugging → change execution strategy easily
Without changing business logic.


*Does Executor automatically have a thread pool?*
No.
Executor is just an interface:
void execute(Runnable command);
It does NOT create a thread pool automatically.
You must provide an implementation.
2️⃣ Who Provides Thread Pool?
Usually we use:
ExecutorService executor = Executors.newFixedThreadPool(5);
Behind the scenes, this creates a:
👉 ThreadPoolExecutor
So yes — Java provides built-in thread pool implementations, but you must explicitly create them.
Common ones:
Executors.newFixedThreadPool(n)
Executors.newCachedThreadPool()
Executors.newSingleThreadExecutor()
Executors.newScheduledThreadPool(n)

All internally use ThreadPoolExecutor.
3️⃣ If I specify thread pool, how does it know Prod vs Lower env?
It DOESN'T know automatically.
You control it via configuration.
Example:
In application.properties (Spring Boot)
thread.pool.size=5   // lower env
thread.pool.size=50  // prod
Then:
@Value("${thread.pool.size}")
private int poolSize;
@Bean
public ExecutorService executorService() {
    return Executors.newFixedThreadPool(poolSize);
}

Now:
Dev → 5 threads
Prod → 50 threads

Same code.
Different config.
4️⃣ How Enterprise Systems Handle This
In real systems:
Thread pool size depends on:
CPU cores
Traffic load
Memory
Blocking vs Non-blocking tasks
Sometimes we calculate:
For CPU-bound tasks:
Threads = CPU cores
For IO-bound tasks:
Threads = CPU cores × 2
or more
5️⃣ What Actually Happens Internally

When you submit tasks:
Task goes into a queue
If free thread available → executes immediately
If all threads busy → task waits in queue
If queue full → RejectedExecutionHandler triggers
This is managed by ThreadPoolExecutor.

6️⃣ In Production Best Practice
Never use:
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
You control queue size
You control rejection policy
Avoid memory leaks