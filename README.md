# Java Versions

This Book provides a comprehensive overview of the features and changes introduced in each version of Java, starting from Java 8 and up to the latest version, Java 25. The goal of this reference is to help Java developers stay up to date with the latest advancements in the Java programming language, as well as to provide a historical perspective on its evolution.

## Long-Term Support (LTS) Versions

The documentation includes **Long-Term Support (LTS)** versions of Java that receive extended support and updates:
- **Java 8 (LTS)** - March 2014 - Extended support until 2030
- **Java 11 (LTS)** - September 2018 - Extended support until 2026
- **Java 17 (LTS)** - September 2021 - Extended support until 2029
- **Java 21 (LTS)** - September 2023 - Extended support until 2031
- **Java 25 (LTS)** - September 2025 - Extended support until 2033+

## Version Documentation

## [Java 8 (LTS)](readme/java-8.md)

The first major version with functional programming support, introducing Lambda expressions, Stream API, and Optional.

## [Java 9](readme/java-9.md)

Introduction of the Java Platform Module System (JPMS/Project Jigsaw).

## [Java 10](readme/java-10.md)

Local variable type inference with the `var` keyword.

## [Java 11 (LTS)](readme/java-11.md)

HTTP Client API, String methods enhancements, and various performance improvements.

## [Java 12](readme/java-12.md)

Switch expressions (preview) and compact number formatting.

## [Java 13](readme/java-13.md)

Text blocks (preview) for multi-line strings.

## [Java 14](readme/java-14.md)

Records (preview), pattern matching for instanceof (preview), and helpful NullPointerExceptions.

## [Java 15](readme/java-15.md)

Sealed classes (preview) and hidden classes.

## [Java 16](readme/java-16.md)

Records and pattern matching become standard features.

## [Java 17 (LTS)](readme/java-17.md)

Sealed classes finalized, enhanced pseudo-random number generators, and strong encapsulation of JDK internals.

## [Java 18](readme/java-18.md)

Simple web server, code snippets in Java API documentation, and UTF-8 by default.

## [Java 19](readme/java-19.md)

Virtual threads (preview), structured concurrency (preview), and pattern matching for switch (preview).

## [Java 20](readme/java-20.md)

Scoped values (preview), record patterns (preview), and pattern matching enhancements.

## [Java 21 (LTS)](readme/java-21.md)

Virtual threads finalized, sequenced collections, pattern matching for switch finalized, and string templates (preview).

## [Java 25 (LTS)](readme/java-25.md)

Primitive types in patterns (preview), flexible constructor bodies, scoped values finalized, generational Shenandoah GC, and Vector API enhancements.

---

## 🎯 Focus Areas for Interview Preparation

### For LTS Versions (Most Important)

**Java 8 (LTS):**
- Lambda expressions & functional interfaces
- Stream API operations
- Optional for null handling

**Java 17 (LTS):**
- Sealed classes
- Pattern matching for instanceof
- Records
- Text blocks

**Java 21 (LTS):**
- Virtual threads (Project Loom)
- Sequenced collections
- Pattern matching for switch
- Structured concurrency

**Java 25 (LTS):**
- Primitive types in patterns
- Flexible constructor bodies
- Generational Shenandoah GC
- Vector API for SIMD operations

---

## 📖 How to Use This Documentation

1. **For Beginners**: Start with Java 8 to understand the functional programming foundation
2. **For Interview Prep**: Focus on LTS versions (8, 11, 17, 21, 25) as they're most commonly used in production
3. **For Migration**: Compare versions to understand what changed between your current and target version
4. **For Advanced Topics**: Explore preview features in recent versions to stay ahead of the curve

---

## 🔗 Additional Resources

- [OpenJDK Projects](openjdk-projects/README.md) - Deep dive into Project Loom, Valhalla, Panama, and Amber
- [Official Oracle Java Documentation](https://docs.oracle.com/en/java/)
- [OpenJDK JEP Index](https://openjdk.org/jeps/0)
