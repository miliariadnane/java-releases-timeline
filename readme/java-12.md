## Java 12 Features

Released on March 19, 2019.

### 1. ``Collectors.teeing()`` in Stream API

- This method was added to stream API in Java 12, as a way to combine multiple collectors into a single collector. This method allows you to apply multiple collectors to the elements of a stream, then combine the results of those collectors into a single result.

````java
public class CollectorsTeeing {
    public static void main(String[] args) {
        List<FootballPlayer> footballPlayers = List.of(
                new FootballPlayer("Cristiano Ronaldo", "Juventus", 36),
                new FootballPlayer("Lionel Messi", "Barcelona", 34),
                new FootballPlayer("Kylian Mbappe", "Paris Saint-Germain", 22),
                new FootballPlayer("Neymar", "Paris Saint-Germain", 29),
                new FootballPlayer("Robert Lewandowski", "Bayern Munich", 32),
                new FootballPlayer("Sadio Mane", "Liverpool", 29)
        );

        Map<String, List<FootballPlayer>> map = footballPlayers.stream()
                .collect(Collectors.teeing(
                        Collectors.filtering(p -> p.age() > 30, Collectors.toList()),
                        Collectors.filtering(p -> p.age() <= 30, Collectors.toList()),
                        (list1, list2) -> {
                            Map<String, List<FootballPlayer>> map1 = new HashMap<>();
                            map1.put("players with age > 30", list1);
                            map1.put("players with age <= 30", list2);
                            return map1;
                        }
                ));
        System.out.println(map);
    }
}
````

### 2. ``Files.mismatch(Path, Path)``

- This method compares the contents of two files and returns the position of the first mismatch, or -1 if the files are the same. It takes in two Path objects as arguments, representing the two files to be compared.

````java
public class FilesMismatch {
    public static void main(String[] args) throws IOException {
        Path path1 = Path.of("C:\\Users\\user\\Desktop\\file1.txt");
        Path path2 = Path.of("C:\\Users\\user\\Desktop\\file2.txt");

        long mismatch = Files.mismatch(path1, path2);
        if (mismatch == -1) {
            System.out.println("The files are the same");
        } else {
            System.out.println("The files are different at position: " + mismatch);
        }
    }
}
````

### 3. Compact Number Formatting (Preview) 

- Java 12 comes with a new method, ``NumberFormat.formatCompactLong()``. This feature allows allows developers to format numbers using the compact number format style, which is a shorter, more localized form of formatting numbers.

````java
// In this example, the number 1,000,000 is being formatted using the US locale and the SHORT style, which results in the output "1M".
public class CompactNumberFormatExample {
    public static void main(String[] args) {
        long number = 1000000;
        NumberFormat formatter = NumberFormat.getCompactNumberInstance(Locale.US, NumberFormat.Style.SHORT);
        String formattedNumber = formatter.format(number);
        System.out.println(formattedNumber);
    }
}
````

### 4. Java Strings New Methods

#### 4.1. ``indent()``

- This method allows you to indent a string by a specified number of spaces. It takes in an integer as an argument, representing the number of spaces to indent the string.

````java
public class IndentExample {
    public static void main(String[] args) {
        String str = "Hello\nWorld";
        String indentedStr = str.indent(4);
        System.out.println(indentedStr); // prints "    Hello\n    World"
    }
}
````

#### 4.2. ``transform()``

- This method allows you to apply a function to each character in a string. It takes in a function as an argument, which is applied to each character in the string.

````java
public class TransformExample {
    public static void main(String[] args) {
        String str = "Hello World";
        String transformedStr = str.transform(s -> s.toUpperCase());
        System.out.println(transformedStr); // prints "HELLO WORLD"
    }
}
````

#### 4.3. ``describeConstable()``

- This is a new method that was added to the ``java.util.Optional``. It is used to provide a way to describe the value of an Optional instance in a way that can be serialized.

#### 4.4. ``resolveConstantDesc()`` 

### 5. Preview Features

#### 5.1. Switch Expressions (Preview) 

[JEP 325](https://openjdk.java.net/jeps/325)

- It is a new way to replace if / else statements.
- Java 12 has enhanced the switch statement for pattern matching. The new syntax is ``case <pattern> ->``. 

````java
// Example from : https://www.baeldung.com/java-12-new-features 

// Old syntax
DayOfWeek dayOfWeek = LocalDate.now().getDayOfWeek();
String typeOfDay = "";
switch (dayOfWeek) {
    case MONDAY:
    case TUESDAY:
    case WEDNESDAY:
    case THURSDAY:
    case FRIDAY:
        typeOfDay = "Working Day";
        break;
    case SATURDAY:
    case SUNDAY:
        typeOfDay = "Day Off";
}

// New syntax
typeOfDay = switch (dayOfWeek) {
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "Working Day";
    case SATURDAY, SUNDAY -> "Day Off";
};
````

- There's also a new possibility to execute code in switch expressions without returning a value.

````java
// Example from : https://www.baeldung.com/java-12-new-features
switch (dayOfWeek) {
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> System.out.println("Working Day");
    case SATURDAY, SUNDAY -> System.out.println("Day Off");
}
````
- You can also use switch expressions with enums.

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

#### 5.2. Pattern Matching for instanceof (Preview)

- **Pattern matching for instanceof** is a feature that was introduced in Java 12 as a preview feature. It allows developers to use a more concise and readable syntax when checking the type of an object and then performing an action based on that type. 

````java	
// Example from : https://www.baeldung.com/java-12-new-features

// old syntax
// -> In previous Java versions, when using, for example, if statements together with instanceof, we would have to explicitly typecast the object to access its features

if (obj instanceof String) {
    String str = (String) obj;
    // do something with str
}

// new syntax
// -> With Java 12, we can declare the new typecasted variable directly in the statement

if (obj instanceof String s) {
    int length = s.length();
}
````
