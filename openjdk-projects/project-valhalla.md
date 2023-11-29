# Project Valhalla



**Goal**: Improve performance and memory efficiency.

* Aims to improve Java's type system and make the language more flexible.

**How**? By having new "value types" that avoid expensive memory allocations. And having "specialized" code that runs faster for specific cases.

1. Example value type:

   ```java
   record Point(int x, int y) {} // Avoids allocation
   ```
2. Example specialization:

   ```java
   @Specialization(on = 0)  
   static int div(int x, int y) {
     return 0;
   }

   @Specialization
   static int div(int x, int y) {
     return x / y; 
   }
   ```

**Benefits**: Faster code execution. Reduced memory usage.
