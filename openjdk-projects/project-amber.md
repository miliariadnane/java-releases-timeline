# Project Amber


**Goal**: Reduce boilerplate code in Java.


**How**? By adding helpful new language features like pattern matching, records, sealed classes.

* Focused on productivity, maintainability, and correctness.

```java
public record Person(String name, int age) {}

Person person = switch(getPerson()) {
  case (String n, int a) -> new Person(n, a);
};
```


**Benefits**: Focus more on business logic. Write less repetitive code.
