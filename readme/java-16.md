## Java 16 Features

### 1. Vector API

- The Vector API is a new feature added to Java 16 as a standard feature. The idea behind this API is to provide a set of interfaces and classes that allow us to perform operations on vectors of data. 

````java	
// traditional way

int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int[] result = new int[numbers.length];

for (int i = 0; i < numbers.length; i++) {
    result[i] = numbers[i] * 2; // result = {2, 4, 6, 8, 10, 12, 14, 16, 18, 20}
}

// vector way

IntVector vector = IntVector.fromArray(
    VectorSpecies.of(int.class), numbers, 0);
IntVector resultVector = vector.mul(2);
int[] result = resultVector.toArray();
````

### 2. Add `Stream.toList()`

- `Stream.toList()` is a new method added to the `Stream` class in Java 16 that allows us to convert a `Stream` to a `List`. It aims to reduce the boilerplate code when we want to convert a steam to a list.

````java	
List<String> premierLeagueClubs = List.of(
    "Manchester United",
    "Manchester City",
    "Liverpool",
    "Chelsea",
    "Arsenal",
    "Tottenham Hotspur",
    "..." // 19 other clubs
);

List<String> premierLeagueClubsStartingWithLetterM = premierLeagueClubs.stream()
    .filter(club -> club.startsWith("M"))
    .toList();
````

### 3. Records

- Records are a new feature added to Java 16 as a standard feature.
- It is a new way to create immutable classes.
- Records give us getters, setters, equals, hashCode, toString are automatically generated.
- All fields in records are private and final.
- We cannot extend a record, abstract or final.

````java	
record Player(String name, int age, String club, String nationality, string position, String[] titles) {}

// create a new player
Player messi = new Player("Lionel Messi", 34, "PSG", "Argentina", "Forward", "🏆🏆🏆");

// get player name
String name = messi.name();
String club = messi.club();
````	

### 4. Sealed Classes (new changes)

### 5. Pattern Matching for instanceof (new changes)
