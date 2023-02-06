## Java 17 Features (LTS)

### 1. Switch Expressions

- Switch expressions are a new feature added to Java 14 as a standard feature.

- It is a new way to replace if / else statements.

#### 1.1. Switch expressions : simple usage

````java
switch (day) {
    case "1" -> System.out.println("Monday");
    case "2" -> System.out.println("Tuesday");
    case "3" -> System.out.println("Wednesday");
    case "4" -> System.out.println("Thursday");
    case "5" -> System.out.println("Friday");
    case "6" -> System.out.println("Saturday");
    case "7" -> System.out.println("Sunday");
    default -> System.out.println("Invalid day");
}
````

#### 1.2. String switch expression

````java
switch (day) {
    case "1" -> "Monday";
    case "2" -> "Tuesday";
    case "3" -> "Wednesday";
    case "4" -> "Thursday";
    case "5" -> "Friday";
    case "6" -> "Saturday";
    case "7" -> "Sunday";
    default -> "Invalid day";
}
````

#### 1.3. Switch expressions : yield

````java
String result = switch (day) {
    case "1" -> "Monday";
    case "2" -> "Tuesday";
    case "3" -> "Wednesday";
    case "4" -> "Thursday";
    case "5" -> "Friday";
    case "6" -> "Saturday";
    case "7" -> "Sunday";
    default -> {
        System.out.println("Invalid day");
        yield "Invalid day"; // yield is used to return a value 
    }
};
````

#### 1.4. Switch expressions : Enum

````java
enm DaysOfWeek {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

public static void main(String[] args) {
    DaysOfWeek day = DaysOfWeek.MONDAY; 
    String result = switch (day) {
        case MONDAY -> "Monday";
        case TUESDAY -> "Tuesday"; 
        case WEDNESDAY -> "Wednesday";
        case THURSDAY -> "Thursday";
        case FRIDAY -> "Friday";
        case SATURDAY -> "Saturday";
        case SUNDAY -> "Sunday";
    };
    System.out.println(result); 
}
````

#### 1.5. Switch expressions (JEP 406 -> Preview pattern matching for switch)

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

### 2. Restore Always-Strict Floating-Point Semantics

[JEP 306](https://openjdk.org/jeps/306)

### 3. Enhanced Pseudo-Random Number Generators 

[JEP 356](https://openjdk.org/jeps/356)

### 4. New macOS Rendering Pipeline 

[JEP 382](https://openjdk.org/jeps/382)

### 5. Deprecate the Applet API for Removal

[JEP 398](https://openjdk.org/jeps/398)

### 6. Strongly Encapsulate JDK Internals

[JEP 403](https://openjdk.org/jeps/403)