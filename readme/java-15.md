# Java 15 Features

### 1. Text Blocks (Standard Feature)

- Text Blocks become a standard feature in Java 15.
- "text blocks" feature allows developers to create multi-line string literals in a more convenient way. Prior to Java 15, creating multi-line strings required the use of escape characters, such as "\n", to create new lines. With text blocks, you can create multi-line strings by enclosing them in triple quotes ( """ ).

#### 1.1. Text blocks : simple usage

````java
// before Java 15
String email = "Hello, \n" +
               "This is a test email. \n" +
               "Thank you.";

// >  Java 15
String email = """
               Hello,
               This is a test email.
               Thank you.
               """;
````

#### 1.2. Text blocks : indentation

````java
String name = "John";
String email = "j.doe@gmail.com"

String message = """
                 Hello %s,
                 This is a test email sent to %s.
                 Thank you.
                 """.formatted(name, email);
````

#### 1.3. Text blocks : Json

````java
String jsonMessage = """
    {
        "firstName": "John",
        "lastName": "Doe",
        "job": "Software Engineer",
        "github": "j.doe"
    }
    """;

String jsonMessage = """
    {
        "firstName": "%s",
        "lastName": "%s",
        "job": "%s",
        "github": "%s"
    }
    """.formatted("Youssouf", "AZz", "Software Engineer", "josef");
````

### 2. Hidden Classes

[A new feature being introduced in Java 15 is known as hidden classes. While most developers won't find a direct benefit from them, anyone who works with dynamic bytecode or JVM languages will likely find them useful. The goal of hidden classes is to allow the runtime creation of classes that are not discoverable. This means they cannot be linked by other classes, nor can they be discovered via reflection. Classes such as these typically have a short lifecycle, and thus, hidden classes are designed to be efficient with both loading and unloading.](https://www.baeldung.com/java-15-new)

### 3. Removed Nashorn JavaScript Engine

### 4. Preview Features

#### 4.1. Sealed Classes (Preview)

- Sealed classes are a new feature added to Java 15 as a preview feature. Sealed classes are a restricted form of classes that can only be extended by classes that are explicitly permitted. This is useful when you want to restrict the inheritance of a class to a specific set of classes.

````java
public sealed class Vehicle permits Car, Truck, Bus {
    // ...
}
````	

- The above code snippet shows a sealed class named Vehicle. The class is sealed, which means only classes after the keyword **permits** are allowed extend the Vehicle class.

- In case that you have already defined **car**, **truck**, and **bus** classes in the same file as **Vehicle**, you can omit the **permits** keyword and the compiler will take care of it implicitly as shown below:

````java
sealed class Vehicle {...}

final class Car extends Vehicle {...}
final class Bike extends Vehicle {...}
final class Bus extends Vehicle {...}
````

- `non-sealed` classes are classes that can be extended by any class. This is the default behavior of classes in Java.

#### 4.2. Records (Second Preview)

- With Java 15, Records get their second preview.
- Records are a special kind of class that is designed to hold data. They are immutable and are intended to replace the need for classes that only hold data.

````java
public record Person(String name, int age) {}
````

#### 4.3. Pattern Matching for instanceof (Second Preview)
