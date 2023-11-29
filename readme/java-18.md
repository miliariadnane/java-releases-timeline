## Java 18 Features

Released March 2022

### 1. UTF-8 by default [JEP 400](https://openjdk.org/jeps/400)

This feature changes the default charset of the Java SE APIs to UTF-8. This means that the APIs that convert between bytes and strings will use UTF-8 as the default charset. This change is aimed at addressing the incompatibility between the default charset used by Java SE APIs and the charset used by many other APIs and libraries.

### 2. Simple Web Server [JEP 408](https://openjdk.org/jeps/408)

This feature introduces a simple web server in the JDK. This web server is not intended for production use, but it can be very useful for prototyping, testing, and debugging. It supports HTTP/1.1 and HTTP/2, and it can serve static and generated content.

```java
import com.sun.net.httpserver.HttpServer;
import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;

import java.io.OutputStream;
import java.io.IOException;
import java.net.InetSocketAddress;

public class SimpleWebServer {
    public static void main(String[] args) throws Exception {
        HttpServer server = HttpServer.create(new InetSocketAddress(8000), 0);
        server.createContext("/", new MyHandler());
        server.setExecutor(null); // creates a default executor
        server.start();
    }

    static class MyHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange t) throws IOException {
            String response = "This is the response";
            t.sendResponseHeaders(200, response.length());
            OutputStream os = t.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}
```

### 3. Code Snippets in Java API Doc [JEP 413](https://openjdk.org/jeps/413)

This feature enhances the Java API documentation with small examples of code, or code snippets, to illustrate the use of the API. This doesn't require a code example as it's a change in the documentation

### 4. Internet-Address Resolution SPI [JEP 418](https://openjdk.org/jeps/418)

It introduces a service provider interface (SPI) by which alternate domain name and address resolution mechanisms can be used. This is more of a system-level change and doesn't have a straightforward code example.

### 5. Preview Features

#### 5.1. Vector API [JEP 417](https://openjdk.org/jeps/417)

This feature provides an initial iteration of an incubator module, jdk.incubator.vector, to express vector computations. Here's a basic example:

```java
import jdk.incubator.vector.*;

public class VectorExample {
    public static void main(String[] args) {
        var a = Vector.of(1, 2, 3, 4);
        var b = Vector.of(2, 2, 2, 2);
        var c = a.mul(b);
        System.out.println(c);
    }
}
```

#### 5.2. Foreign Function & Memory API (Second Incubator) [JEP 419](https://openjdk.org/jeps/419)

An APIthat allows Java programs to safely and efficiently access foreign memory outside of the Java heap. Here's a basic example:

```java
import jdk.incubator.foreign.*;

public class ForeignMemoryExample {
    public static void main(String[] args) {
        try (MemorySegment segment = MemorySegment.allocateNative(100)) { // Allocate 100 bytes of native memory
            MemoryAccess.setIntAtIndex(segment, 0, 123);  // Set the first 4 bytes to 123
            System.out.println(MemoryAccess.getIntAtIndex(segment, 0));  // Get the first 4 bytes
        }
    }
}
```

#### 5.3. Pattern Matching for switch (second preview) [JEP 420](https://openjdk.org/jeps/420)


```java
public class PatternMatchingExample {
    public static void main(String[] args) {
        Object obj = "Hello, World!";
        switch (obj) {
            case Integer i -> System.out.println("Integer: " + i);
            case String s -> System.out.println("String: " + s);
            default -> System.out.println("Unknown type");
        }
    }
}
```
