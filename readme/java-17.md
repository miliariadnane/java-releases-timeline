# Java 17 Features (LTS)

Released on September 14, 2021.

* Java 17 is a **Long-Term Support (LTS)** release, making it one of the most important Java versions.
* It's the first LTS release since Java 11 (released in 2018), bridging a 3-year gap.
* Java 17 brings many features that were previewed in Java 12-16, now **finalized and production-ready**.
* This version is widely adopted in enterprise applications and is recommended for production use.

## 1. Sealed Classes [JEP 409](https://openjdk.org/jeps/409)

* **Sealed Classes** restrict which other classes or interfaces can extend or implement them.
* This feature provides more control over inheritance hierarchies and enables **exhaustive pattern matching**.
* Sealed Classes were previewed in Java 15-16 and **finalized in Java 17**.

### Why Sealed Classes?

* **Domain modeling**: Express that a class hierarchy is closed and finite
* **Security**: Prevent unauthorized extensions of sensitive classes
* **Pattern matching**: Enable exhaustive case analysis in switch expressions
* **API design**: Better control over which classes can implement your interfaces

### Basic Sealed Class Example

```java
// Shape is sealed - only Circle, Rectangle, and Triangle can extend it
public sealed class Shape permits Circle, Rectangle, Triangle {
    private final String name;
    
    public Shape(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}

// Option 1: final - cannot be extended further
public final class Circle extends Shape {
    private final double radius;
    
    public Circle(double radius) {
        super("Circle");
        this.radius = radius;
    }
    
    public double area() {
        return Math.PI * radius * radius;
    }
}

// Option 2: sealed - can be extended by specific classes
public sealed class Rectangle extends Shape permits Square {
    private final double width;
    private final double height;
    
    public Rectangle(double width, double height) {
        super("Rectangle");
        this.width = width;
        this.height = height;
    }
    
    public double area() {
        return width * height;
    }
}

// Option 3: non-sealed - can be extended by any class
public non-sealed class Triangle extends Shape {
    private final double base;
    private final double height;
    
    public Triangle(double base, double height) {
        super("Triangle");
        this.base = base;
        this.height = height;
    }
    
    public double area() {
        return 0.5 * base * height;
    }
}

public final class Square extends Rectangle {
    public Square(double side) {
        super(side, side);
    }
}
```

### Sealed Interfaces

```java
public sealed interface Payment permits CreditCard, DebitCard, Cash {
    double amount();
    boolean process();
}

public final class CreditCard implements Payment {
    private final double amount;
    private final String cardNumber;
    
    public CreditCard(double amount, String cardNumber) {
        this.amount = amount;
        this.cardNumber = cardNumber;
    }
    
    @Override
    public double amount() {
        return amount;
    }
    
    @Override
    public boolean process() {
        System.out.println("Processing credit card payment: $" + amount);
        return true;
    }
}

public final class DebitCard implements Payment {
    private final double amount;
    private final String cardNumber;
    
    public DebitCard(double amount, String cardNumber) {
        this.amount = amount;
        this.cardNumber = cardNumber;
    }
    
    @Override
    public double amount() {
        return amount;
    }
    
    @Override
    public boolean process() {
        System.out.println("Processing debit card payment: $" + amount);
        return true;
    }
}

public final class Cash implements Payment {
    private final double amount;
    
    public Cash(double amount) {
        this.amount = amount;
    }
    
    @Override
    public double amount() {
        return amount;
    }
    
    @Override
    public boolean process() {
        System.out.println("Processing cash payment: $" + amount);
        return true;
    }
}
```

### Exhaustive Pattern Matching with Sealed Classes

```java
// Compiler knows all possible subtypes - no default needed!
public double calculateArea(Shape shape) {
    return switch (shape) {
        case Circle c -> Math.PI * Math.pow(c.getRadius(), 2);
        case Rectangle r -> r.getWidth() * r.getHeight();
        case Triangle t -> 0.5 * t.getBase() * t.getHeight();
        // No default needed - compiler knows these are all possible cases!
    };
}

// If you add a new permitted class, compiler will force you to handle it
```

### Real-World Example: HTTP Response

```java
public sealed interface HttpResponse permits Success, ClientError, ServerError {
    int statusCode();
    String message();
}

public record Success(int statusCode, String message, String body) 
    implements HttpResponse {}

public record ClientError(int statusCode, String message, String details) 
    implements HttpResponse {}

public record ServerError(int statusCode, String message, Throwable cause) 
    implements HttpResponse {}

// Usage
public void handleResponse(HttpResponse response) {
    switch (response) {
        case Success s -> 
            System.out.println("Success: " + s.body());
        case ClientError ce -> 
            System.out.println("Client error: " + ce.details());
        case ServerError se -> 
            System.out.println("Server error: " + se.cause().getMessage());
    };
}
```

## 2. Pattern Matching for instanceof [JEP 406](https://openjdk.org/jeps/406)

* **Pattern Matching for instanceof** eliminates the need for explicit casting after type checking.
* This feature was previewed in Java 14-15 and **finalized in Java 16**, but it's important for Java 17 developers to know.
* Reduces boilerplate code and makes code more readable.

### Before Pattern Matching (Traditional)

```java
public void processObject(Object obj) {
    if (obj instanceof String) {
        String str = (String) obj;  // Explicit cast needed
        System.out.println("String length: " + str.length());
    } else if (obj instanceof Integer) {
        Integer num = (Integer) obj;  // Explicit cast needed
        System.out.println("Number squared: " + (num * num));
    } else if (obj instanceof List) {
        List<?> list = (List<?>) obj;  // Explicit cast needed
        System.out.println("List size: " + list.size());
    }
}
```

### With Pattern Matching (Java 17)

```java
public void processObject(Object obj) {
    if (obj instanceof String str) {  // Pattern variable 'str' is available
        System.out.println("String length: " + str.length());
    } else if (obj instanceof Integer num) {  // Pattern variable 'num' is available
        System.out.println("Number squared: " + (num * num));
    } else if (obj instanceof List<?> list) {  // Pattern variable 'list' is available
        System.out.println("List size: " + list.size());
    }
}
```

### Pattern Matching with Logical Operators

```java
public void validateEmployee(Object obj) {
    // Pattern variable available in the same expression
    if (obj instanceof Employee emp && emp.getAge() > 18) {
        System.out.println(emp.getName() + " is an adult employee");
    }
    
    // Pattern variable NOT available after OR operator
    if (obj instanceof Employee emp || obj instanceof Contractor) {
        // emp is NOT in scope here
    }
}
```

### Real-World Example: Processing Different Message Types

```java
public class MessageProcessor {
    public void processMessage(Object message) {
        if (message instanceof TextMessage txt && !txt.content().isEmpty()) {
            sendTextNotification(txt.content());
        } else if (message instanceof ImageMessage img && img.size() < 5_000_000) {
            processImage(img.data());
        } else if (message instanceof VideoMessage vid && vid.duration() < 300) {
            processVideo(vid.url());
        } else {
            System.out.println("Unsupported or invalid message");
        }
    }
}

record TextMessage(String content) {}
record ImageMessage(byte[] data, long size) {}
record VideoMessage(String url, int duration) {}
```

### Combining with Switch (Preview in Java 17)

```java
// Pattern matching for switch was preview in Java 17
public String describeObject(Object obj) {
    return switch (obj) {
        case String s -> "String with length: " + s.length();
        case Integer i -> "Integer: " + i;
        case Long l -> "Long: " + l;
        case null -> "It's null";
        default -> "Unknown type";
    };
}
```

## 3. Text Blocks (Standard Feature)

* **Text Blocks** were finalized in Java 15 but are essential for Java 17 developers.
* They provide a cleaner way to write multi-line strings without escape sequences.
* Particularly useful for HTML, JSON, SQL, and other multi-line text.

### Before Text Blocks

```java
String html = "<html>\n" +
              "    <body>\n" +
              "        <h1>Hello, World!</h1>\n" +
              "    </body>\n" +
              "</html>";

String json = "{\n" +
              "  \"name\": \"Alice\",\n" +
              "  \"age\": 25,\n" +
              "  \"city\": \"New York\"\n" +
              "}";
```

### With Text Blocks (Java 17)

```java
String html = """
    <html>
        <body>
            <h1>Hello, World!</h1>
        </body>
    </html>
    """;

String json = """
    {
      "name": "Alice",
      "age": 25,
      "city": "New York"
    }
    """;
```

### SQL Queries with Text Blocks

```java
String query = """
    SELECT users.id, users.name, orders.total
    FROM users
    INNER JOIN orders ON users.id = orders.user_id
    WHERE orders.total > 100
    ORDER BY orders.total DESC
    LIMIT 10
    """;
```

### Text Block with String Formatting

```java
String name = "Alice";
int age = 25;
String city = "New York";

String formatted = """
    {
      "name": "%s",
      "age": %d,
      "city": "%s"
    }
    """.formatted(name, age, city);

System.out.println(formatted);
```

### Escaping in Text Blocks

```java
// Use \s to preserve trailing spaces
String withSpaces = """
    Line 1    \s
    Line 2    \s
    """;

// Use \ to join lines
String singleLine = """
    This is a long line \
    that continues here \
    and ends here.
    """;
```

## 4. Records (Standard Feature)

* **Records** were finalized in Java 16 but are fundamental for Java 17 applications.
* Records provide a compact syntax for declaring data carrier classes.
* Automatically generate constructor, getters, `equals()`, `hashCode()`, and `toString()`.

### Traditional Class vs Record

```java
// BEFORE: Traditional class (lots of boilerplate)
public class PersonOld {
    private final String name;
    private final int age;
    private final String email;
    
    public PersonOld(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getEmail() { return email; }
    
    @Override
    public boolean equals(Object o) {
        // ... lots of boilerplate code
    }
    
    @Override
    public int hashCode() {
        // ... boilerplate code
    }
    
    @Override
    public String toString() {
        // ... boilerplate code
    }
}

// AFTER: Record (concise and clean)
public record Person(String name, int age, String email) {}

// Usage - same as traditional class!
Person person = new Person("Alice", 25, "alice@example.com");
System.out.println(person.name());  // Alice
System.out.println(person.age());   // 25
```

### Records with Custom Methods

```java
public record Employee(String name, int age, String department, double salary) {
    
    // Compact constructor - validation
    public Employee {
        if (age < 18) {
            throw new IllegalArgumentException("Employee must be at least 18 years old");
        }
        if (salary < 0) {
            throw new IllegalArgumentException("Salary cannot be negative");
        }
    }
    
    // Custom methods
    public boolean isSenior() {
        return age >= 40;
    }
    
    public double annualBonus() {
        return salary * 0.1;
    }
    
    public String displayInfo() {
        return String.format("%s (%d years) - %s: $%.2f", 
            name, age, department, salary);
    }
}

// Usage
Employee emp = new Employee("Bob", 35, "Engineering", 75000);
System.out.println(emp.displayInfo());
System.out.println("Annual bonus: $" + emp.annualBonus());
```

### Records with Static Methods and Fields

```java
public record Point(int x, int y) {
    // Static field
    public static final Point ORIGIN = new Point(0, 0);
    
    // Static factory method
    public static Point of(int x, int y) {
        return new Point(x, y);
    }
    
    // Custom instance method
    public double distanceFrom(Point other) {
        int dx = this.x - other.x;
        int dy = this.y - other.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Usage
Point p1 = Point.ORIGIN;
Point p2 = Point.of(3, 4);
System.out.println("Distance: " + p1.distanceFrom(p2));  // 5.0
```

### Nested Records

```java
public record Order(
    String orderId,
    Customer customer,
    List<Item> items,
    double total
) {
    public record Customer(String name, String email) {}
    public record Item(String productId, int quantity, double price) {}
}

// Usage
Order.Customer customer = new Order.Customer("Alice", "alice@example.com");
Order.Item item1 = new Order.Item("P001", 2, 29.99);
Order.Item item2 = new Order.Item("P002", 1, 49.99);

Order order = new Order(
    "ORD123",
    customer,
    List.of(item1, item2),
    109.97
);
```

## 5. Enhanced Pseudo-Random Number Generators [JEP 356](https://openjdk.org/jeps/356)

* Introduces new interfaces and implementations for **pseudo-random number generators (PRNGs)**.
* Provides better algorithms and more flexibility for random number generation.
* Makes it easier to use different PRNG algorithms.

### New RandomGenerator Interface

```java
import java.util.random.RandomGenerator;
import java.util.random.RandomGeneratorFactory;

public class RandomExample {
    public static void main(String[] args) {
        // Use default random generator
        RandomGenerator random = RandomGenerator.getDefault();
        
        // Generate different types of random values
        int randomInt = random.nextInt();
        int randomInRange = random.nextInt(1, 101);  // 1 to 100
        long randomLong = random.nextLong();
        double randomDouble = random.nextDouble();
        boolean randomBoolean = random.nextBoolean();
        
        System.out.println("Random int (1-100): " + randomInRange);
        System.out.println("Random double: " + randomDouble);
        System.out.println("Random boolean: " + randomBoolean);
    }
}
```

### Different PRNG Algorithms

```java
import java.util.random.RandomGenerator;
import java.util.random.RandomGeneratorFactory;

public class DifferentAlgorithms {
    public static void main(String[] args) {
        // List all available algorithms
        RandomGeneratorFactory.all()
            .map(factory -> factory.name())
            .forEach(System.out::println);
        
        // Use specific algorithm - L64X128MixRandom (fast and high quality)
        RandomGenerator random1 = RandomGeneratorFactory.of("L64X128MixRandom").create();
        System.out.println("L64X128MixRandom: " + random1.nextInt(100));
        
        // Use Xoshiro256PlusPlus (very fast)
        RandomGenerator random2 = RandomGeneratorFactory.of("Xoshiro256PlusPlus").create();
        System.out.println("Xoshiro256PlusPlus: " + random2.nextInt(100));
        
        // Use SecureRandom for cryptographic purposes
        RandomGenerator random3 = RandomGeneratorFactory.of("SecureRandom").create();
        System.out.println("SecureRandom: " + random3.nextInt(100));
    }
}
```

### Streaming Random Numbers

```java
import java.util.random.RandomGenerator;
import java.util.stream.IntStream;

public class RandomStreams {
    public static void main(String[] args) {
        RandomGenerator random = RandomGenerator.getDefault();
        
        // Generate stream of 10 random integers between 1 and 100
        IntStream randomInts = random.ints(10, 1, 101);
        randomInts.forEach(System.out::println);
        
        // Generate infinite stream (use limit)
        random.ints(1, 101)
            .limit(5)
            .forEach(i -> System.out.println("Random: " + i));
        
        // Generate random doubles and filter
        random.doubles(100, 0.0, 1.0)
            .filter(d -> d > 0.8)
            .limit(5)
            .forEach(d -> System.out.printf("%.4f%n", d));
    }
}
```

### Real-World Example: Lottery Number Generator

```java
import java.util.random.RandomGenerator;
import java.util.Set;
import java.util.TreeSet;

public class LotteryGenerator {
    public static Set<Integer> generateLotteryNumbers(int count, int max) {
        RandomGenerator random = RandomGenerator.getDefault();
        Set<Integer> numbers = new TreeSet<>();
        
        while (numbers.size() < count) {
            numbers.add(random.nextInt(1, max + 1));
        }
        
        return numbers;
    }
    
    public static void main(String[] args) {
        // Generate 6 unique numbers between 1 and 49
        Set<Integer> lottery = generateLotteryNumbers(6, 49);
        System.out.println("Your lottery numbers: " + lottery);
    }
}
```

## 6. Context-Specific Deserialization Filters [JEP 415](https://openjdk.org/jeps/415)

* Allows applications to configure **context-specific deserialization filters** via JVM-wide filter factory.
* Improves security by preventing deserialization attacks.
* Provides fine-grained control over which classes can be deserialized.

### Setting Up Deserialization Filters

```java
import java.io.*;
import java.util.function.BinaryOperator;

public class DeserializationFilterExample {
    public static void main(String[] args) {
        // Set up filter factory
        ObjectInputFilter.Config.setSerialFilterFactory(
            (current) -> ObjectInputFilter.Config.createFilter(
                "example.trusted.*;!*"  // Only allow classes from example.trusted package
            )
        );
        
        // Now all deserialization will use this filter
    }
}
```

## 7. Strong Encapsulation of JDK Internals [JEP 403](https://openjdk.org/jeps/403)

* **Strongly encapsulates** all internal elements of the JDK.
* Internal APIs (like `sun.misc.Unsafe`) are no longer accessible by default.
* Improves security and maintainability of the JDK.

### Impact on Applications

```java
// BEFORE Java 17: This would work (but was never recommended)
// import sun.misc.Unsafe;  // Internal API

// AFTER Java 17: This will fail unless you use --add-opens
// You should migrate to official APIs instead
```

## 8. Deprecate the Applet API for Removal [JEP 398](https://openjdk.org/jeps/398)

* The **Applet API** has been deprecated for removal.
* Web browsers no longer support Java plugins.
* Modern alternatives: WebAssembly, JavaScript, or standalone applications.

## 9. New macOS Rendering Pipeline [JEP 382](https://openjdk.org/jeps/382)

* Implements Java 2D internal rendering pipeline for **macOS using Apple Metal API**.
* Replaces the deprecated OpenGL API on macOS.
* Improves performance and future-proofs macOS support.

## 10. Key Takeaways for Interview Preparation

### Most Important Features to Master

1. **Sealed Classes** ⭐⭐⭐
   - Understand permitted subclasses
   - Know final, sealed, and non-sealed modifiers
   - Practice exhaustive pattern matching
   - Explain benefits for API design

2. **Pattern Matching for instanceof** ⭐⭐⭐
   - Eliminate explicit casting
   - Understand pattern variable scope
   - Combine with logical operators
   - Write cleaner, safer code

3. **Records** ⭐⭐⭐
   - Data carrier classes with minimal boilerplate
   - Automatic equals, hashCode, toString
   - Compact constructors for validation
   - Immutability by default

4. **Text Blocks** ⭐⭐
   - Multi-line strings without escape sequences
   - Perfect for SQL, JSON, HTML
   - String formatting with text blocks
   - Understand escape sequences

5. **Enhanced Random Generators** ⭐
   - New RandomGenerator interface
   - Different PRNG algorithms
   - Stream-based random generation
   - Better performance and quality

### Interview Discussion Points

* **LTS Release**: Java 17 is the first LTS after Java 11 (3-year gap)
* **Production Ready**: All preview features from Java 12-16 are now finalized
* **Security**: Strong encapsulation of JDK internals
* **Modern Language**: Records, sealed classes, and pattern matching make Java more expressive
* **Migration**: Many organizations are moving from Java 8/11 to Java 17

### Common Interview Questions

1. **"What's new in Java 17?"**
   - Lead with Sealed Classes (unique to Java 17)
   - Mention Records and Text Blocks (finalized)
   - Pattern matching for instanceof
   - Emphasize it's an LTS release

2. **"Explain Sealed Classes and their benefits"**
   - Restricted inheritance hierarchy
   - Three options: final, sealed, non-sealed
   - Exhaustive pattern matching
   - Better domain modeling

3. **"What are the advantages of Records?"**
   - Immutable data carriers
   - Less boilerplate (no getters/setters/equals/hashCode)
   - Clear intent (data class)
   - Works well with pattern matching

4. **"How does pattern matching improve code?"**
   - Eliminates redundant casting
   - More readable and concise
   - Type-safe
   - Reduces errors

5. **"Why is Java 17 important for enterprise?"**
   - LTS release (long-term support)
   - 3 years of improvements since Java 11
   - Production-ready features
   - Better performance and security

### Migration from Java 11 to Java 17

**Key Changes:**
- Strong encapsulation: May need `--add-opens` for reflection
- Removed APIs: Applet API, Nashorn JavaScript engine
- New features: Sealed classes, records, pattern matching, text blocks
- Performance improvements: Better garbage collection, faster startup

**Benefits:**
- Modern language features
- Better security
- Improved performance
- LTS support until 2029

### Code Comparison: Java 11 vs Java 17

```java
// JAVA 11 Style
public class UserService {
    public void processUser(Object obj) {
        if (obj instanceof User) {
            User user = (User) obj;
            if (user.getAge() >= 18) {
                System.out.println("Adult user: " + user.getName());
            }
        }
    }
}

// JAVA 17 Style - much cleaner!
public sealed interface User permits AdultUser, MinorUser {
    String name();
    int age();
}

public record AdultUser(String name, int age) implements User {
    public AdultUser {
        if (age < 18) throw new IllegalArgumentException();
    }
}

public record MinorUser(String name, int age) implements User {
    public MinorUser {
        if (age >= 18) throw new IllegalArgumentException();
    }
}

public class UserService {
    public void processUser(User user) {
        if (user instanceof AdultUser adult && adult.age() >= 21) {
            System.out.println("Adult user: " + adult.name());
        }
    }
}
```

### Real-World Example: Complete Application

```java
// Domain model using Java 17 features
public sealed interface Vehicle permits Car, Truck, Motorcycle {
    String licensePlate();
    int year();
    double calculateTax();
}

public record Car(
    String licensePlate,
    int year,
    String model,
    int seats
) implements Vehicle {
    @Override
    public double calculateTax() {
        return seats * 50.0;
    }
}

public record Truck(
    String licensePlate,
    int year,
    String model,
    double cargoCapacity
) implements Vehicle {
    @Override
    public double calculateTax() {
        return cargoCapacity * 10.0;
    }
}

public record Motorcycle(
    String licensePlate,
    int year,
    String model
) implements Vehicle {
    @Override
    public double calculateTax() {
        return 100.0;
    }
}

// Service class using pattern matching
public class VehicleService {
    public String generateReport(Vehicle vehicle) {
        return """
            Vehicle Report
            ==============
            License: %s
            Year: %d
            Type: %s
            Tax: $%.2f
            """.formatted(
                vehicle.licensePlate(),
                vehicle.year(),
                getVehicleType(vehicle),
                vehicle.calculateTax()
            );
    }
    
    private String getVehicleType(Vehicle vehicle) {
        return switch (vehicle) {
            case Car c -> "Car (" + c.seats() + " seats)";
            case Truck t -> "Truck (" + t.cargoCapacity() + " tons)";
            case Motorcycle m -> "Motorcycle";
        };
    }
}
```

## Summary

Java 17 is a **major milestone** as an LTS release that brings together 3 years of innovation:

✅ **Sealed Classes** - Better control over inheritance  
✅ **Pattern Matching** - Cleaner, safer code  
✅ **Records** - Minimal boilerplate for data classes  
✅ **Text Blocks** - Beautiful multi-line strings  
✅ **Enhanced Random** - Better random number generation  
✅ **Strong Encapsulation** - Improved security  
✅ **LTS Support** - Production-ready until 2029

**Perfect for interviews because:**
- It's widely adopted in enterprise
- Combines multiple preview features now finalized
- Shows evolution from Java 11
- Demonstrates modern Java capabilities
