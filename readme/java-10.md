## Java 10 Features

Released on March 20, 2018.

### 1. Local Variable Type Inference (var keyword)

* var keyword is introduced in Java 10.
* We can use var keyword **only for local variables**.
* Allows us to declare a variable without specifying the type.
* Type inference : compiler infers the type of the variable based on the value.

````java	
for (var club : List.of("Barcelona", "Real Madrid", "Atletico Madrid")) {
    System.out.println(club);
}
````

### 2. ``Optional.orElseThrow()`` Method

````java	
Optional<String> optional = Optional.of("value");
optional.orElseThrow(IllegalStateException::new);
// or
optional.orElseThrow(() -> new IllegalStateException("exception message"));
````

### 3. Collection Changes | API for creating immutable collections

* `copyOf()`

The List.copyOf, Set.copyOf, and Map.copyOf factory methods **allow creating an immutable copy** of a collection (List, Set, or Map).

````java	
List<String> players = List.copyOf(Arrays.asList("Messi", "Ronaldo", "Neymar"));

Map<String, Integer> players = Map.copyOf(Map.of("Messi", 10, "Ronaldo", 7, "Neymar", 10));
````

### 4. Garbage-Collector Interface 

- Java 10 introduced a new Garbage Collector (GC) interface, which is designed to provide a more flexible and extensible GC framework. The new GC interface allows for the creation of custom GC algorithms, which can be plugged into the JVM.

``void initialize()``: This method is called when the GC is first created, and it can be used to perform any necessary initialization.

``void notifyLowMemory()``: This method is called when the JVM detects low memory conditions.

``void recordGCEnd()``: This method is called when a GC cycle ends, and it can be used to record information about the GC cycle.

### 5. Performance Improvements

[Java 10 Performance Improvements](https://www.baeldung.com/java-10-performance-improvements)

### 6. Time-Based Release Versioning

- Resouces :

    * [Link 1](https://www.baeldung.com/java-time-based-releases)
    * [Link 2](https://www.tutorialspoint.com/java10/java10_time_based_release_versioning.htm)
