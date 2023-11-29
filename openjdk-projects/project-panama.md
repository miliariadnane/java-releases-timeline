# Project Panama


**Goal**: Make working with non-Java code easier.


**How**? By allowing developers to directly call C code from Java. Without needing complex glue code.

* Provides an efficient foreign function interface (FFI) and native interoperability.
* Allows calling native C libraries directly from Java without JNI.
* Key goals are performance, ease of use, and safety.

```java
try (LibraryLibrary lib = Library.of("c");
     MemorySegment segment = lib.allocateSegment(100)) {  
  lib.putInt(segment, 0, 42);
}
```


**Benefits**: Work with existing C libraries easily. Avoid complex JNI code.
