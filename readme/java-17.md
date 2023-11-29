## Java 17 Features (LTS)

Release date: September 14, 2021

### 1. Sealed Classes [JEP 409](https://openjdk.org/jeps/409)

[Sealed Classes Details](java-15.md#41-sealed-classes-preview)

* Sealed Classes were proposed by [JEP 360](https://openjdk.org/jeps/360) and delivered in JDK 15 as a preview feature.
* They were proposed again, with refinements, by [JEP 397](https://openjdk.org/jeps/397) and delivered in JDK 16 as a preview feature.
* Now in JDK 17, Sealed Classes are being finalized and delivered as a standard feature.

* Sealed classes and interfaces restrict which other classes or interfaces may extend or implement them. In other words, a sealed class or interface can specify which types are permitted to extend or implement it. This feature was introduced as a final feature in Java 17.

Here's a basic example:

````java
public sealed class Shape permits Circle, Rectangle {
    // class body
}

public final class Circle extends Shape {
    // class body
}

public non-sealed class Rectangle extends Shape {
    // class body
}
````

> In this example, Shape is a sealed class that only permits Circle and Rectangle to extend it. Circle is a final class, meaning it can't be extended further. Rectangle is a non-sealed class, meaning it can be extended by other classes.

### 2. New macOS Rendering Pipeline [JEP 382](https://openjdk.org/jeps/382)

### 3. Enhanced Pseudo-Random Number Generators [JEP 356](https://openjdk.org/jeps/356)

This feature introduces new interfaces and implementations for pseudorandom number generators (PRNGs) in the JDK. Here's an example of how to use the new RandomGenerator interface:

````java
import java.util.random.RandomGenerator;

public class RandomExample {
    public static void main(String[] args) {
        RandomGenerator randomGenerator = RandomGenerator.getDefault();
        int randomInt = randomGenerator.nextInt();
        System.out.println(randomInt);
    }
}
````

### 4. Preview Features

#### Switch Expressions (Preview) [JEP 406](https://openjdk.org/jeps/406)

- [Switch Expressions (Preview)](java-12.md#5-preview-features) Introduced in Java 12 as a preview feature.
- Switch expressions are a new feature added to Java 14 as a standard feature.

- This is a new pattern matching feature for switch expressions, aimed to reduce the amount of boilerplate code.

````java
// Example from : https://www.baeldung.com/ 

static record Human (String name, int age, String profession) {}

public String checkObject(Object obj) {
    return switch (obj) {
        case Human h -> "Name: %s, age: %s and profession: %s".formatted(h.name(), h.age(), h.profession());
        case Circle c -> "This is a circle";
        case Shape s -> "It is just a shape";
        case null -> "It is null";
        default -> "It is an object";
    };
}

public String checkShape(Shape shape) {
    return switch (shape) {
        case Triangle t && (t.getNumberOfSides() != 3) -> "This is a weird triangle";
        case Circle c && (c.getNumberOfSides() != 0) -> "This is a weird circle";
        default -> "Just a normal shape";
    };
}
````

### 5. Deprecated & removed features

#### 5.1. Deprecate the Applet API for Removal [JEP 398](https://openjdk.org/jeps/398)

#### 5.2. Strongly Encapsulate JDK Internals [JEP 403](https://openjdk.org/jeps/403)
