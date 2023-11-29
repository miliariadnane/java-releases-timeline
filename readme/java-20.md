# Java 20 Features

Release date: September 14, 2022

### 1. Scoped Values (Incubator) [JEP 429](https://openjdk.org/jeps/429)


### 2. Records (second standard) [JEP 432](https://openjdk.org/jeps/432)

Records can now explicitly declare canonical constructors. This allows more control over construction.

```java
public record Person(String name, int age) {
    public Person {
        if (name == null) {
            throw new NullPointerException("name");
        }
    }
}
```

### 3. Pattern Matching for instanceof (Preview) [JEP 433](https://openjdk.org/jeps/433)

New pattern matching syntax for instanceof using switch expression:

```java
public class PatternMatchingForInstanceOf {
    public static void main(String[] args) {
        Object o = "Hello, World!";
        String s = switch (o) {
            case String str -> str;
            case Integer i -> String.valueOf(i);
            default -> "unknown";
        };
        System.out.println(s);
    }
}
```

### 4. Foreign Memory Access API (Second Preview) [JEP 434](https://openjdk.org/jeps/434)

Continued incubation of API for safe and efficient access to memory outside Java heap.


### 5. Virtual Threads [JEP 436](https://openjdk.org/jeps/436)

### 6. Structured Concurrency [JEP 437](https://openjdk.org/jeps/437)

### 7. Vector API (Second Incubator) [JEP 438](https://openjdk.org/jeps/438)

Enhancements like support for variable vector sizes and vectorization of Loops.
