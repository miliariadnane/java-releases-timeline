## Java 11 Features (LTS)

### 1. String API Improvements

- String API has been improved in Java 11, we can now use the following methods:

#### 1.1. ``isBlank() ``

- This method checks if a string is empty or contains only white space characters.

````java
String name = "Youssouf";
String name2 = " ";
String name3 = "";

System.out.println(name.isBlank()); // false
System.out.println(name2.isBlank()); // true
System.out.println(name3.isBlank()); // true
````

#### 1.2. ``lines()``

- This method returns a stream of lines extracted from the string.

````java
String message = "Hello, \r \n This is a test email. \r \n Thank you.";

message.lines().forEach(System.out::println); // Hello, This is a test email. Thank you.
````

#### 1.3. ``strip()``, ``stripLeading()``, ``stripTrailing()``

- These methods allow you to remove leading and trailing whitespace from a string.

````java
String name = " DevInStyle Podcast ";
String name2 = " DevInStyle Podcast";

System.out.println(name.strip()); 
System.out.println(name2.stripLeading()); 
````

#### 1.4. ``repeat(int)``

- This method returns a new string consisting of the original string repeated a specified number of times.

````java
String fullName = "Anas Abdel"

System.out.println(fullName.repeat(3)); 
````

### 2. Collection.toArray() 

- In Java 11, the ``toArray()`` method of the Collection interface was updated to include an overload that allows you to specify the type of the array that should be returned. This new version of the method is as follows:

````java
<T> T[] toArray(IntFunction<T[]> generator);
````

- This new overload allows you to provide a lambda expression that will be used to create the array that will be returned. This is useful when you want to create an array of a specific type, such as an array of a custom class.

````java
List<String> names = List.of("Youssouf", "Anas", "Abdel");

String[] namesArray = names.toArray(String[]::new); // [Youssouf, Anas, Abdel]

// Example 2 with a custom class
public record SoftwareEngineer(String name, int age, String job) {}

List<Player> players = List.of(
        new Player("Youssouf", 25, "Full Stack Developer"),
        new Player("Anas", 25, "ML Engineer"),
        new Player("Oussama", 25, "Wordpress Developer")
);

Player[] playersArray = players.toArray(Player[]::new); 
````

### 3. Optional API Improvements

#### 3.1 ``isEmpty()`` 

- This method of the Optional class was added in Java 11. This method returns true if the Optional is empty, otherwise it returns false.

````java
Optional<String> car = Optional.of("BMW X5");
Optional<String> car2 = Optional.empty();

System.out.println(car.isEmpty()); // false
System.out.println(car2.isEmpty()); // true
````

#### 3.2 ``or()`` 

- or method with supplier, **orElseThrow()**, **orElseGet()** and **orElse()** methods, which provide more options for handling the case when an Optional is empty.

````java
Optional<String> onlineEvent = Optional.of("DevInStyle meeting in Zoom 28/02/2023");

String eventStatus1 = onlineEvent.orElse("No event scheduled at the moment");
String eventStatus2 = onlineEvent.orElseGet(() -> "No event scheduled at the moment");
String eventStatus3 = onlineEvent.orElseThrow(() -> new IllegalStateException("No event found"));
````

#### 3.3 ``stream()`` 

- This method returns a sequential Stream containing the value of the Optional if it is present, otherwise it returns an empty Stream.

````java
Optional<String> cars = Optional.of("BMW X5", "Mercedes Benz", "Audi A8");

car.stream().forEach(System.out::println); 
````

#### 3.4 ``ifPresentOrElse()`` 

- This method takes two arguments, one is a Consumer to be invoked if a value is present, and the other is a Runnable to be invoked if no value is present.

````java
Optional<String> newLaptop = Optional.of("MacBook Pro 2022");

newLaptop.ifPresentOrElse(
        laptop -> System.out.println("I have a new laptop: " + laptop),
        () -> System.out.println("I don't have a new laptop")
);
````

### 4. Files update and features

- Java 11 introduced several updates and new features to the ``java.nio.file`` package which provides powerful APIs for working with files system and file I/O. These updates and new features include:

#### 4.1 ``readString(Path)``

- This method allows for reading a string from a file, in a similar way to the FileReader, but with less verbosity.

````java
public class FileReadExample {
    public static void main(String[] args) {
        Path filePath = Paths.get("C:\\Users\\abdel\\Desktop\\test.txt");
        try {
            String fileContent = Files.readString(filePath);
            System.out.println(fileContent);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
````

#### 4.2 ``writeString(Path, String)``

- This method allows for writing a string to a file, in a similar way to the FileWriter, but with less verbosity.

````java
public class FileWriteExample {
    public static void main(String[] args) {
        String content = "Hello World !! This is an example of writing a string to a file using Java 11.";
        Path filePath = Paths.get("example.txt");
        try {
            Files.writeString(filePath, content); 
            System.out.println("File written successfully!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
````

### 5. HTTP Client API Improvements

- Java 11 introduced a new HTTP client called ``java.net.http.HttpClient`` which is designed to be more flexible and efficient than the previous java.net.URLConnection-based client. 

**The new HTTP API improves overall performance and provides support for both HTTP/1.1 and HTTP/2:**

````java	
// Example from : https://www.baeldung.com/java-11-new-features

HttpClient httpClient = HttpClient.newBuilder()
  .version(HttpClient.Version.HTTP_2)
  .connectTimeout(Duration.ofSeconds(20))
  .build();
HttpRequest httpRequest = HttpRequest.newBuilder()
  .GET()
  .uri(URI.create("http://localhost:" + port))
  .build();
HttpResponse httpResponse = httpClient.send(httpRequest, HttpResponse.BodyHandlers.ofString());
assertThat(httpResponse.body()).isEqualTo("Hello from the server!");
````

➡️ The following link of the official documentation provides more details about the new HTTP client: [link](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html)

