# Java 21 Features (LTS)

Released on September 19, 2023.

* Java 21 is a **Long-Term Support (LTS)** release, making it a major milestone in Java's evolution.
* It brings significant improvements in **performance**, **concurrency**, and **developer productivity**.
* This version introduces game-changing features like **Virtual Threads** and **Sequenced Collections**.

## 1. Record Patterns [JEP 440](https://openjdk.org/jeps/440)

* Record Patterns enhance pattern matching by allowing you to **destructure record values** directly.
* This feature makes code more concise and readable when working with records.

### Before Java 21 (Using instanceof)

```java
record Point(int x, int y) {}

public void printPoint(Object obj) {
    if (obj instanceof Point point) {
        int x = point.x();
        int y = point.y();
        System.out.println("x: " + x + ", y: " + y);
    }
}
```

### With Java 21 (Record Patterns)

```java
record Point(int x, int y) {}

public void printPoint(Object obj) {
    if (obj instanceof Point(int x, int y)) {
        System.out.println("x: " + x + ", y: " + y);
    }
}
```

### Nested Record Patterns

```java
record Point(int x, int y) {}
record Rectangle(Point upperLeft, Point lowerRight) {}

public void printRectangle(Object obj) {
    if (obj instanceof Rectangle(Point(int x1, int y1), Point(int x2, int y2))) {
        System.out.println("Rectangle from (" + x1 + "," + y1 + ") to (" + x2 + "," + y2 + ")");
    }
}

// Usage
Rectangle rect = new Rectangle(new Point(0, 5), new Point(10, 0));
printRectangle(rect); 
// Output: Rectangle from (0,5) to (10,0)
```

## 2. Pattern Matching for switch [JEP 441](https://openjdk.org/jeps/441)

* Pattern matching for switch has been **finalized** in Java 21 (was preview in previous versions).
* It allows you to use patterns in switch expressions and statements, making code more expressive.

### Traditional Switch (Before Java 21)

```java
public String getObjectType(Object obj) {
    String result;
    if (obj instanceof String) {
        result = "It's a String";
    } else if (obj instanceof Integer) {
        result = "It's an Integer";
    } else if (obj instanceof Long) {
        result = "It's a Long";
    } else {
        result = "Unknown type";
    }
    return result;
}
```

### Pattern Matching Switch (Java 21)

```java
public String getObjectType(Object obj) {
    return switch (obj) {
        case String s -> "It's a String: " + s;
        case Integer i -> "It's an Integer: " + i;
        case Long l -> "It's a Long: " + l;
        case null -> "It's null";
        default -> "Unknown type";
    };
}
```

### Advanced Pattern Matching with Guards

```java
record Employee(String name, int age, String department) {}

public String analyzeEmployee(Employee emp) {
    return switch (emp) {
        case Employee(String name, int age, String dept) when age < 25 ->
            name + " is a junior employee";
        case Employee(String name, int age, String dept) when age >= 25 && age < 40 ->
            name + " is a mid-level employee";
        case Employee(String name, int age, String dept) when age >= 40 ->
            name + " is a senior employee";
        default -> "Unknown employee status";
    };
}

// Usage
Employee emp1 = new Employee("Alice", 23, "Engineering");
System.out.println(analyzeEmployee(emp1)); 
// Output: Alice is a junior employee
```

### Pattern Matching with Null Handling

```java
public String processValue(String value) {
    return switch (value) {
        case null -> "Value is null";
        case String s when s.isEmpty() -> "Value is empty";
        case String s when s.length() < 5 -> "Short string: " + s;
        case String s -> "Regular string: " + s;
    };
}
```

## 3. Virtual Threads [JEP 444](https://openjdk.java.net/jeps/444)

* **Virtual Threads** are **lightweight threads** that dramatically reduce the effort of writing, maintaining, and observing high-throughput concurrent applications.
* Part of **Project Loom**, virtual threads are managed by the JVM rather than the operating system.
* They enable **millions of threads** to run concurrently without consuming excessive resources.

### Benefits of Virtual Threads

* **Lightweight**: You can create millions of virtual threads (vs thousands of platform threads)
* **Simplified Concurrency**: Write simple blocking code that scales like asynchronous code
* **Better Resource Utilization**: No need for complex thread pools
* **Easier Debugging**: Stack traces are clearer and more intuitive

### Traditional Platform Threads

```java
// Creating 10,000 platform threads would be very expensive!
public void traditionalThreads() {
    for (int i = 0; i < 10_000; i++) {
        int taskId = i;
        Thread thread = new Thread(() -> {
            System.out.println("Task " + taskId + " running on " + Thread.currentThread());
            try {
                Thread.sleep(1000); // Blocks an OS thread
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        thread.start();
    }
}
```

### Virtual Threads (Java 21)

```java
// Creating millions of virtual threads is cheap and efficient!
public void virtualThreadsExample() {
    for (int i = 0; i < 1_000_000; i++) {
        int taskId = i;
        Thread.startVirtualThread(() -> {
            System.out.println("Task " + taskId + " running on virtual thread");
            try {
                Thread.sleep(1000); // Doesn't block an OS thread!
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
    }
}
```

### Virtual Threads with ExecutorService

```java
public void processRequestsWithVirtualThreads() throws InterruptedException {
    try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
        // Submit 100,000 tasks - each runs on its own virtual thread
        List<Future<String>> futures = new ArrayList<>();
        
        for (int i = 0; i < 100_000; i++) {
            int taskId = i;
            Future<String> future = executor.submit(() -> {
                // Simulate API call or database query
                Thread.sleep(100);
                return "Result from task " + taskId;
            });
            futures.add(future);
        }
        
        // Process results
        for (Future<String> future : futures) {
            System.out.println(future.get());
        }
    }
}
```

### Real-World Example: HTTP Server

```java
public class VirtualThreadHttpServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8080);
        System.out.println("Server started on port 8080");
        
        while (true) {
            Socket clientSocket = serverSocket.accept();
            
            // Handle each connection in a virtual thread
            Thread.startVirtualThread(() -> handleClient(clientSocket));
        }
    }
    
    private static void handleClient(Socket clientSocket) {
        try (var in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
             var out = new PrintWriter(clientSocket.getOutputStream(), true)) {
            
            // Simulate processing time
            Thread.sleep(100);
            
            // Send response
            out.println("HTTP/1.1 200 OK");
            out.println("Content-Type: text/plain");
            out.println();
            out.println("Hello from Virtual Thread: " + Thread.currentThread());
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 4. Sequenced Collections [JEP 431](https://openjdk.java.net/jeps/431)

* **Sequenced Collections** introduce new interfaces to represent collections with a **defined encounter order**.
* This feature adds consistent methods to access first/last elements and reverse the order.

### New Interfaces Hierarchy

```
SequencedCollection<E>
  ├── List<E> (already ordered)
  ├── Deque<E> (ordered from both ends)
  └── SortedSet<E> → SequencedSet<E>

SequencedMap<K,V>
  └── SortedMap<K,V>
```

### Key Methods Added

```java
interface SequencedCollection<E> extends Collection<E> {
    // Access first and last elements
    E getFirst();
    E getLast();
    
    // Add elements at both ends
    void addFirst(E e);
    void addLast(E e);
    
    // Remove elements from both ends
    E removeFirst();
    E removeLast();
    
    // Get reversed view
    SequencedCollection<E> reversed();
}
```

### SequencedCollection Examples

```java
// Working with List
List<String> players = new ArrayList<>();
players.addFirst("Messi");      // Add at the beginning
players.addLast("Ronaldo");     // Add at the end
players.addLast("Neymar");

System.out.println(players.getFirst());  // Output: Messi
System.out.println(players.getLast());   // Output: Neymar

// Reverse the list
List<String> reversedPlayers = players.reversed();
System.out.println(reversedPlayers);  // Output: [Neymar, Ronaldo, Messi]
```

### SequencedSet Examples

```java
// Working with LinkedHashSet (maintains insertion order)
SequencedSet<String> cities = new LinkedHashSet<>();
cities.addFirst("Casablanca");
cities.addLast("Rabat");
cities.addLast("Marrakech");

System.out.println(cities.getFirst());   // Output: Casablanca
System.out.println(cities.getLast());    // Output: Marrakech

// Remove from both ends
cities.removeFirst();
System.out.println(cities);  // Output: [Rabat, Marrakech]
```

### SequencedMap Examples

```java
// Working with LinkedHashMap
SequencedMap<String, Integer> scores = new LinkedHashMap<>();
scores.putFirst("Alice", 95);
scores.putLast("Bob", 87);
scores.putLast("Charlie", 92);

// Access first and last entries
Map.Entry<String, Integer> firstEntry = scores.firstEntry();
Map.Entry<String, Integer> lastEntry = scores.lastEntry();

System.out.println("First: " + firstEntry.getKey() + " = " + firstEntry.getValue());
// Output: First: Alice = 95

System.out.println("Last: " + lastEntry.getKey() + " = " + lastEntry.getValue());
// Output: Last: Charlie = 92

// Get reversed view
SequencedMap<String, Integer> reversed = scores.reversed();
System.out.println(reversed);  // Output: {Charlie=92, Bob=87, Alice=95}
```

### Before Java 21 vs After

```java
// BEFORE Java 21: Getting first/last elements was inconsistent
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
String first = list.get(0);                    // List way
String last = list.get(list.size() - 1);       // List way

Deque<String> deque = new ArrayDeque<>(Arrays.asList("A", "B", "C"));
String firstDeque = deque.getFirst();          // Deque way
String lastDeque = deque.getLast();            // Deque way

// AFTER Java 21: Consistent API across all sequenced collections
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
String first = list.getFirst();    // Unified way
String last = list.getLast();      // Unified way

// Works the same for any SequencedCollection!
```

## 5. String Templates (Preview) [JEP 430](https://openjdk.java.net/jeps/430)

* **String Templates** provide a safer and more expressive way to compose strings with embedded expressions.
* This feature helps **prevent injection attacks** and makes string interpolation more readable.
* **Note**: This is a **preview feature** in Java 21, use `--enable-preview` flag.

### Traditional String Concatenation

```java
String name = "Alice";
int age = 25;
String department = "Engineering";

// Old way - error-prone and hard to read
String message1 = "Employee " + name + " is " + age + " years old and works in " + department;

// Using String.format - verbose
String message2 = String.format("Employee %s is %d years old and works in %s", name, age, department);
```

### String Templates (Java 21)

```java
String name = "Alice";
int age = 25;
String department = "Engineering";

// New way - clean and expressive
String message = STR."Employee \{name} is \{age} years old and works in \{department}";
System.out.println(message);
// Output: Employee Alice is 25 years old and works in Engineering
```

### String Templates with Expressions

```java
int x = 10;
int y = 20;

String result = STR."The sum of \{x} and \{y} is \{x + y}";
System.out.println(result);
// Output: The sum of 10 and 20 is 30

// Works with method calls
record User(String name, int age) {}
User user = new User("Bob", 30);

String info = STR."User: \{user.name()}, Age: \{user.age()}, Adult: \{user.age() >= 18}";
System.out.println(info);
// Output: User: Bob, Age: 30, Adult: true
```

### Multi-line String Templates

```java
String name = "Alice";
String role = "Software Engineer";
int experience = 5;

String report = STR."""
    Employee Report
    ================
    Name: \{name}
    Role: \{role}
    Experience: \{experience} years
    Status: \{experience >= 3 ? "Senior" : "Junior"}
    """;

System.out.println(report);
/*
Output:
Employee Report
================
Name: Alice
Role: Software Engineer
Experience: 5 years
Status: Senior
*/
```

### FMT Template Processor (Formatted Output)

```java
record Product(String name, double price, int quantity) {}

Product product = new Product("Laptop", 999.99, 5);

// FMT processor for formatted output
String formatted = FMT."""
    Product: %-20s\{product.name()}
    Price:   $%10.2f\{product.price()}
    Qty:     %5d\{product.quantity()}
    Total:   $%10.2f\{product.price() * product.quantity()}
    """;

System.out.println(formatted);
```

## 6. Unnamed Classes and Instance Main Methods (Preview) [JEP 443](https://openjdk.java.net/jeps/443)

* This feature makes Java more **beginner-friendly** by allowing simpler program structures.
* Reduces boilerplate code for simple programs and learning scenarios.
* **Note**: This is a **preview feature** in Java 21.

### Traditional Java Program (Before)

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### Simplified Program (Java 21 Preview)

```java
// No class declaration needed!
void main() {
    System.out.println("Hello, World!");
}
```

### Instance Main Methods

```java
// Instance main method (non-static)
void main() {
    String message = getMessage();
    System.out.println(message);
}

String getMessage() {
    return "Hello from instance method!";
}
```

### Benefits for Beginners

```java
// Example 1: Simple calculator
void main() {
    int a = 10;
    int b = 20;
    System.out.println("Sum: " + (a + b));
}

// Example 2: Working with collections
void main() {
    var numbers = List.of(1, 2, 3, 4, 5);
    var sum = numbers.stream()
                    .mapToInt(Integer::intValue)
                    .sum();
    System.out.println("Total: " + sum);
}
```

## 7. Structured Concurrency (Preview) [JEP 453](https://openjdk.java.net/jeps/453)

* **Structured Concurrency** treats multiple tasks running in different threads as a **single unit of work**.
* Simplifies error handling and cancellation in concurrent code.
* Works perfectly with **Virtual Threads**.

### Traditional Concurrent Code (Complex)

```java
public User fetchUserData(String userId) throws ExecutionException, InterruptedException {
    ExecutorService executor = Executors.newCachedThreadPool();
    
    Future<UserProfile> profileFuture = executor.submit(() -> fetchProfile(userId));
    Future<UserOrders> ordersFuture = executor.submit(() -> fetchOrders(userId));
    Future<UserPreferences> prefsFuture = executor.submit(() -> fetchPreferences(userId));
    
    try {
        UserProfile profile = profileFuture.get();
        UserOrders orders = ordersFuture.get();
        UserPreferences prefs = prefsFuture.get();
        
        return new User(profile, orders, prefs);
    } finally {
        executor.shutdown();
    }
}
```

### Structured Concurrency (Java 21)

```java
public User fetchUserData(String userId) throws ExecutionException, InterruptedException {
    try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
        // Fork multiple tasks
        Supplier<UserProfile> profile = scope.fork(() -> fetchProfile(userId));
        Supplier<UserOrders> orders = scope.fork(() -> fetchOrders(userId));
        Supplier<UserPreferences> prefs = scope.fork(() -> fetchPreferences(userId));
        
        // Wait for all to complete or fail
        scope.join()           // Wait for all tasks
             .throwIfFailed(); // Throw if any failed
        
        // All tasks succeeded - get results
        return new User(profile.get(), orders.get(), prefs.get());
    }
}
```

### ShutdownOnSuccess Pattern

```java
// Get the first successful result from multiple sources
public String fetchFromFastestServer(List<String> serverUrls) throws ExecutionException, InterruptedException {
    try (var scope = new StructuredTaskScope.ShutdownOnSuccess<String>()) {
        // Fork tasks to query all servers
        for (String url : serverUrls) {
            scope.fork(() -> fetchFromServer(url));
        }
        
        // Wait for first success and cancel others
        scope.join();
        
        // Return the first successful result
        return scope.result();
    }
}
```

### Benefits

* **Automatic cleanup**: All subtasks are completed or cancelled when scope exits
* **Clear hierarchy**: Parent-child relationship between tasks
* **Error propagation**: Failures are handled consistently
* **Cancellation**: If one task fails, others are automatically cancelled

## 8. Scoped Values (Preview) [JEP 446](https://openjdk.java.net/jeps/446)

* **Scoped Values** provide a way to share immutable data within and across threads.
* A safer and more performant alternative to **ThreadLocal**.
* Perfect for use with Virtual Threads.

### ThreadLocal (Traditional Approach)

```java
// ThreadLocal - mutable and can leak
public class UserContext {
    private static final ThreadLocal<User> CURRENT_USER = new ThreadLocal<>();
    
    public static void setUser(User user) {
        CURRENT_USER.set(user);
    }
    
    public static User getUser() {
        return CURRENT_USER.get();
    }
    
    public static void clear() {
        CURRENT_USER.remove(); // Easy to forget!
    }
}
```

### Scoped Values (Java 21)

```java
public class UserContext {
    public static final ScopedValue<User> CURRENT_USER = ScopedValue.newInstance();
    
    public static void processRequest(User user) {
        // Bind value for the scope
        ScopedValue.where(CURRENT_USER, user)
                   .run(() -> {
                       // Value is available in this scope and child threads
                       handleRequest();
                   });
        // Value automatically cleared after scope
    }
    
    private static void handleRequest() {
        User user = CURRENT_USER.get();
        System.out.println("Processing request for: " + user.name());
        
        // Value is also available in virtual threads spawned here
        Thread.startVirtualThread(() -> {
            User sameUser = CURRENT_USER.get();
            System.out.println("Virtual thread sees: " + sameUser.name());
        });
    }
}
```

### Real-World Example: Web Request Context

```java
public class RequestContext {
    public static final ScopedValue<String> REQUEST_ID = ScopedValue.newInstance();
    public static final ScopedValue<String> USER_ID = ScopedValue.newInstance();
    
    public void handleWebRequest(String requestId, String userId) {
        ScopedValue.where(REQUEST_ID, requestId)
                   .where(USER_ID, userId)
                   .run(() -> {
                       processRequest();
                   });
    }
    
    private void processRequest() {
        // Available everywhere in the call stack
        log("Processing request");
        businessLogic();
    }
    
    private void businessLogic() {
        // No need to pass parameters!
        log("Executing business logic");
        databaseOperation();
    }
    
    private void databaseOperation() {
        log("Database operation");
    }
    
    private void log(String message) {
        String requestId = REQUEST_ID.get();
        String userId = USER_ID.get();
        System.out.println("[" + requestId + "][" + userId + "] " + message);
    }
}
```

### Benefits of Scoped Values

* **Immutable**: Once set, cannot be changed
* **Automatic cleanup**: No need to manually remove values
* **Better performance**: More efficient than ThreadLocal with virtual threads
* **Inheritance**: Automatically inherited by child threads
* **Safety**: Prevents accidental mutation

## 9. Foreign Function & Memory API (Third Preview) [JEP 442](https://openjdk.java.net/jeps/442)

* The **Foreign Function & Memory API** allows Java programs to interoperate with **native code and memory** outside the JVM.
* A modern replacement for **JNI (Java Native Interface)** - safer, simpler, and more efficient.
* Part of **Project Panama**.

### Why Foreign Function & Memory API?

* **Safety**: Better than JNI, prevents many common errors
* **Performance**: Direct access to native memory without copying
* **Ease of use**: No need to write C/C++ glue code
* **Interoperability**: Call native libraries directly from Java

### Calling Native Functions

```java
import java.lang.foreign.*;
import java.lang.invoke.MethodHandle;

public class NativeExample {
    public static void main(String[] args) throws Throwable {
        // Load C standard library
        Linker linker = Linker.nativeLinker();
        SymbolLookup stdlib = linker.defaultLookup();
        
        // Find the 'strlen' function in C standard library
        MemorySegment strlenAddress = stdlib.find("strlen").orElseThrow();
        
        // Define the function signature: size_t strlen(const char *s)
        FunctionDescriptor strlenDescriptor = FunctionDescriptor.of(
            ValueLayout.JAVA_LONG,  // return type: size_t
            ValueLayout.ADDRESS     // parameter: const char*
        );
        
        // Create a method handle for strlen
        MethodHandle strlen = linker.downcallHandle(
            strlenAddress,
            strlenDescriptor
        );
        
        // Allocate native memory for a string
        try (Arena arena = Arena.ofConfined()) {
            MemorySegment str = arena.allocateUtf8String("Hello, Native World!");
            
            // Call native strlen function
            long length = (long) strlen.invoke(str);
            System.out.println("String length: " + length);  // Output: 20
        }
    }
}
```

### Working with Native Memory

```java
public class MemoryExample {
    public static void main(String[] args) {
        // Allocate native memory
        try (Arena arena = Arena.ofConfined()) {
            // Allocate 100 bytes of native memory
            MemorySegment segment = arena.allocate(100);
            
            // Write data to native memory
            for (int i = 0; i < 10; i++) {
                segment.setAtIndex(ValueLayout.JAVA_INT, i, i * 10);
            }
            
            // Read data from native memory
            for (int i = 0; i < 10; i++) {
                int value = segment.getAtIndex(ValueLayout.JAVA_INT, i);
                System.out.println("Value at index " + i + ": " + value);
            }
            
            // Memory automatically freed when arena closes
        }
    }
}
```

### Benefits

* **No JNI overhead**: Direct access to native code
* **Type safety**: Compile-time checks for native function signatures
* **Memory safety**: Automatic memory management with Arena
* **Performance**: Zero-copy access to native memory

## 10. Key Takeaways for Interview Preparation

### Most Important Features to Master

1. **Virtual Threads** ⭐⭐⭐
   - Understand lightweight concurrency
   - Know when to use vs platform threads
   - Practice with ExecutorService

2. **Pattern Matching for Switch** ⭐⭐⭐
   - Master guard conditions
   - Understand null handling
   - Practice with records

3. **Sequenced Collections** ⭐⭐
   - Know the new methods (getFirst, getLast, reversed)
   - Understand the interface hierarchy
   - Compare with pre-Java 21 approaches

4. **Record Patterns** ⭐⭐
   - Practice destructuring records
   - Understand nested patterns
   - Combine with switch expressions

5. **Structured Concurrency** ⭐
   - Understand task scoping
   - Know error handling patterns
   - Combine with Virtual Threads

### Interview Discussion Points

* **Performance**: Virtual Threads enable massive scalability (millions of threads)
* **Simplicity**: Pattern matching reduces boilerplate code
* **Safety**: Structured concurrency prevents resource leaks
* **Modern Java**: These features make Java competitive with modern languages
* **Migration Path**: How to adopt these features in existing codebases

### Common Interview Questions

1. **"What's new in Java 21?"**
   - Lead with Virtual Threads and Sequenced Collections
   - Mention pattern matching enhancements
   - Discuss preview features you've tried

2. **"How do Virtual Threads differ from Platform Threads?"**
   - Lightweight (millions vs thousands)
   - JVM-managed vs OS-managed
   - Better for I/O-bound tasks

3. **"When would you use Scoped Values over ThreadLocal?"**
   - Immutability requirements
   - Virtual thread compatibility
   - Automatic cleanup needs

4. **"What are the benefits of Sequenced Collections?"**
   - Consistent API across collection types
   - First/last element access
   - Reversible views
