## Java 9 Features

Released on September 21, 2017.
  
### 1. JShell 

- JShell is a new tool that comes with Java 9. It is an interactive tool that allows you to write and execute Java code.
- It allows you to write and execute Java code using a command line interface.

![JShell beginnersbook](https://beginnersbook.com/wp-content/uploads/2018/04/JShell_methods.jpg)

````java
// JShell
jshell> int a = 10;
a ==> 10

jshell> int b = 20;
b ==> 20

jshell> int c = a + b;
c ==> 30

jshell> System.out.println(c);

jshell> /exit
````

### 2. Java Platform Module System (JPMS) [Java modularity]

- Java Platform Module System (JPMS) is a new feature added to Java 9. It is a new way to organize Java code into modules.

#### 2.1. What is java module?

- Similar to Java package, modules introduce another level of Java code organization. Each module is a collection of packages. A module is declared by adding a module-info.java file to the root of the module.

````java
module abc.xyz {

}
````

#### 2.2. Export modules

````java
module abc.xyz {
    exports com.demo.common;
}
````

[more details](https://www.baeldung.com/java-9-modularity)

### 3. Collection API updates

- Java 9 introduced some new methods in the Collection API. Here some of the notable changes include:

#### 3.1. `of()`

The List.of, Set.of, and Map.of factory methods allow `creating immutable collections` with a small number of elements.

````java
List<String> players = List.of("Messi", "Ronaldo", "Neymar");
Set<String> cars = Set.of("BMW", "Audi", "Mercedes");
Map<String, Integer> players = Map.of("Messi", 10, "Ronaldo", 7, "Neymar", 10);
````

#### 3.2. `ofEntries`

The Set.ofEntries and Map.ofEntries factory methods allow creating immutable maps and sets with a small number of entries.

````java
Map<String, Integer> players = Map.ofEntries(
    entry("Messi", 10),
    entry("Ronaldo", 7),
    entry("Neymar", 10)
);
````

#### 3.3. `Iterable.forEach`

The Iterable.forEach method is now a default method, allowing it to be used with older classes that implement the Iterable interface.

````java
List.of("Messi", "Ronaldo", "Neymar").forEach(System.out::println);
````

#### 3.4. `Arrays.compare()`

The Arrays.compare method allows comparing two arrays lexicographically.

````java
int[] a = {1, 2, 3};
int[] b = {4, 5, 6};

int result = Arrays.compare(a, b);
````

#### 3.5 `Arrays.mismatch()`

The Arrays.mismatch method allows finding the first index at which two arrays differ.

````java
int[] intArray1 = {1, 2, 3, 4, 5};
int[] intArray2 = {1, 2, 3, 8, 9};

int result = Arrays.mismatch(intArray1, intArray2);

System.out.println(result); // 3
````

#### 3.6. `Arrays.equals()`

The Arrays.equals method has been overloaded to allow comparing two arrays for equality.

````java
String[] playerList1 = {"Messi", "Ronaldo", "Neymar"};
String[] playerList2 = {"Messi", "Ronaldo", "Neymar"};

boolean result = Arrays.equals(player1, player2);

System.out.println(result); // true
````


### 4. Stream API updates

- Java 9 introduced some new methods in the Stream API. Here some of the notable changes include:

#### 4.1. `takeWhile() and dropWhile()`

The takeWhile method allows taking elements from a stream while a given predicate is true.

````java
List.of("Messi", "Ronaldo", "Neymar").stream()
    .takeWhile(player -> player.length() > 5) // or dropWhile : droping elements from a stream while a given predicate is true.
    .forEach(System.out::println);
````

#### 4.2. `ofNullable()`

The Stream.ofNullable method allows creating a stream from a single element that may be null.

````java
Stream.ofNullable("nanodev").forEach(System.out::println);
````

#### 4.4. `iterate()`

The Stream.iterate method allows creating an infinite stream by repeatedly applying a function to the previous element.

````java
Stream.iterate(1, n -> n + 1)
    .limit(10)
    .forEach(System.out::println);
````

#### 4.5 `IntStream`, `LongStream`, and `DoubleStream` methods

These streams now have methods for calculating the sum, average, min, and max of elements in the stream.

````java
IntStream.of(1, 2, 3, 4, 5)
    .sum(); // 15

IntStream.iterate(1, n -> n + 1)
    .limit(10)
    .average(); // 5.5

IntStream.of(2, 4, 6, 8, 10, 15, 20)
    .dropWhile(i -> i%2 == 0)
    .forEach(System.out::println); 
````

### 5. Http 2 Client

- Java 9 introduced a new HTTP client API, which is built on top of the new HTTP/2 protocol. 
- This new API provides a more efficient and powerful way to send and receive HTTP requests and responses. It supports full-duplex communication, allowing multiple requests and responses to be sent and received simultaneously over a single connection. 

### 6. Platform and JVM Logging

[JEP 264: Platform Logging API](https://openjdk.java.net/jeps/264)

### 7. Multi-release JAR Files

Java 9 enhanced the JAR file format which can contain multiple Java-release-specific versions of our class files to coexist in a single archive. Using this feature, we can upgrade our application/library to new version of Java without forcing the users to upgrade to the same Java version.

* To create a multi-release JAR file, we need to declare the `Multi-Release` attribute in the JAR manifest file **META-INF/MANIFEST.MF**.

````MF
Multi-Release: true
````

Suppose later in the future Java 10 release, it is decided to upgrade class A and class B to a new version. We can create a new directory named **META-INF/versions/10** in the JAR file and place the new version of class A in it.

````MF
jar root
    - classA
    - classB
    - META-INF
        - versions
            - 10
                - classA
            - 10
                - classB
````

### 9. Stack Walking

Resouces:

- [Link 1](https://docs.oracle.com/javase/9/docs/api/java/lang/StackWalker.html)
- [Link 2](https://www.baeldung.com/java-9-stackwalking-api)
