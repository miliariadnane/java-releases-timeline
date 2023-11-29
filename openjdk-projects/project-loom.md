# Project Loom



**Goal**: Make it easier to write concurrent and asynchronous programs in Java.


**How**? By introducing lightweight threads called fibers that are efficiently managed by the Java runtime.

* Provides lightweight user-mode threads called fibers to improve support for concurrency in Java.

```java
Fiber<Void> fiber = new Fiber<>((suspendContext) -> {
  System.out.println("Running on thread "+Thread.currentThread().getName());
});

fiber.start();
```

* Fibers provide efficient context switching and the ability to suspend/resume execution.
* Can be used with virtual threads to implement asynchronous and non-blocking programs more easily.

**Benefits**: Easier to write non-blocking code. Avoid issues with regular threads like deadlocks.
