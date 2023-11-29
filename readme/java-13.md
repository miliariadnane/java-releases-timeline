## Java 13 Features

Released on September 17, 2019.

### 1. Text Blocks (Preview) [JEP 355](https://openjdk.java.net/jeps/355)

[Text Blocks](java-15.md#1-text-blocks-standard-feature) become a preview feature in Java 15

### 2. Switch Expressions Enhancements [JEP 354](https://openjdk.java.net/jeps/354)

[Switch Expressions (Preview)](java-12.md#5-preview-features) Introduced in Java 12 as a preview feature.

* The word **break** is no longer needed in switch expressions. It replaced by **yield**.

#### 2.1. Switch expressions : yield

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

[Switch Expressions](java-17.md#1-switch-expressions)

### 3. Reimplement the Legacy Socket API

### 4. ZGC: Uncommit Unused Memory

### 5. FileSystems.newFileSystem() Method

### 6. DOM and SAX Factories with Namespace Support
