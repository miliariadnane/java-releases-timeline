# Project Loom

![loom_project.jpeg](assets/loom_project.jpeg?t=1701382166350)

Project Loom is an initiative by the OpenJDK community to make it easier to write concurrent and asynchronous programs in Java. The main idea behind Project Loom is to introduce a new kind of threading model called "fibers". These fibers are lightweight threads that are managed by the Java runtime rather than the operating system. This makes them much more efficient in terms of memory and context-switching overhead.

**Goal**: Make it easier to write concurrent and asynchronous programs in Java.

**How**? By introducing lightweight threads called fibers that are efficiently managed by the Java runtime.

* Provides lightweight user-mode threads called fibers to improve support for concurrency in Java.
* Fibers provide efficient context switching and the ability to suspend/resume execution.
* Can be used with virtual threads to implement asynchronous and non-blocking programs more easily.

**Benefits**: Easier to write non-blocking code. Avoid issues with regular threads like deadlocks.

## Keep in mind those key concepts:

1. **`Fibers`**: Fibers are lightweight threads. They are managed by the Java runtime, not the operating system. This means that creating, destroying, and switching between fibers is much cheaper than doing the same with traditional threads.
2. **`Concurrency`**: Concurrency is the ability of a program to execute multiple tasks at the same time. With fibers, you can write concurrent programs that are easier to reason about and debug, because each task can be written as a sequential piece of code.
3. **`Asynchronous Programming`**: Asynchronous programming is a style of programming where tasks are executed without blocking the execution of other tasks. With fibers, you can write asynchronous code in a more straightforward, synchronous style.

Now, let's look at a simple example of how to use fibers in Java:

### Create new thread in Java Old Way

```java
public class ThreadExample {
    public static void main(String[] args) {
        // Define a task to be run in a separate thread
        Runnable task = () -> {
            System.out.println("Running on thread: " + Thread.currentThread().getName());
        };

        // Create a new thread with the defined task
        Thread thread = new Thread(task);
        // Start the execution of the thread
        thread.start();

        try {
            // Wait for the thread to finish execution
            thread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

> In this example, we create a **Runnable** task that prints the name of the current thread. We then create a new **Thread** with this task and start it with **thread.start()**. We wait for the thread to finish with **thread.join()**, handling any InterruptedException that might occur.

### Create new thread in Java with Fibers Loom

```java
import java.util.concurrent.Callable;
import java.lang.Thread;

public class FiberExample {
    public static void main(String[] args) {
        // Define a task to be run in a separate fiber
        Callable<Void> task = () -> {
            System.out.println("Running on fiber: " + Thread.currentThread().getName());
            return null;
        };

        // Open a new FiberScope
        try (var scope = FiberScope.open()) {
            // Create a new fiber with the defined task within the opened FiberScope
            Fiber<Void> fiber = new Fiber<>(scope, task).start();
            // Wait for the fiber to finish execution
            fiber.join();
        }
    }
}
```

> In this example, we create a **Callable** task that prints the name of the current fiber (which is also a **Thread**). We then create a new **Fiber** with this task and start it with **fiber.start()**. We wait for the fiber to finish with **fiber.join()**. Note that fibers are started and joined within a **FiberScope**, which is a context that manages the lifecycle of fibers.

> The main difference is that fibers are much more lightweight and efficient, making them a better choice for concurrent and asynchronous programming in Java.
