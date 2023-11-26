## Java Stream Example

Java Streams are a powerful feature introduced in Java 8 that allows you to process collections of data in a functional programming style. 
They enable you to express complex data manipulations with concise and readable code.

Java Streams operate on sequences of elements, typically from collections like lists, sets, or arrays. 
The key abstraction is the `Stream` interface. A stream pipeline consists of a source, zero or more intermediate operations, 
and a terminal operation.

- Intermediate operations are operations that transform a stream into another stream. They are lazy, meaning they don't process the data until a terminal operation is encountered- like `.filter()`.
- Terminal operations are operations that produce a result or a side-effect. They trigger the processing of the Stream- like `.collect()`.

### Learn By Example:

```java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        List<String> countryList = Arrays.asList("Bangladesh", "Canada", "India", "Pakistan", "USA");

        // Creating a stream from a collection/ list.
        Stream<String> countryStream = countryList.stream();

        // Example-1: Filter elements that start with 'B' and convert to uppercase
        List<String> result = countryList.stream()
                .filter(s -> s.startsWith("B"))
                .map(String::toUpperCase)
                .collect(Collectors.toList()); // Utilizing Stream Collector Class to collect the result as list.

        System.out.println(result);  // Output: [BANGLADESH]


        // Example-2: Count elements that start with 'a'
        long count = countryStream
                .filter(s -> s.startsWith("B"))
                .count();

        System.out.println("Count of countries start with `B`: " + count);  // Output: 1


        // Example-3: Terminal Operations. There are several useful terminal operations.
        // forEach(): Works like normal collection forEach.
        countryList.stream().forEach(System.out::println);

        // toArray(): Converts the stream elements into an array.
        String[] countryArray = countryList.stream().toArray(String[]::new);
        System.out.println(Arrays.toString(countryArray));

        // reduce(): Performs a reduction on the elements of the stream using an associative accumulation function and returns an Optional.
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        Optional<Integer> sum = numbers.stream().reduce(Integer::sum);
        System.out.println("Sum of 1 to 5: " + sum);

        // collect(): Performs a mutable reduction on the elements of the stream into a different form, such as a List, Set, or Map.
        Set<String> collectedSet = countryList.stream().collect(Collectors.toSet());
        System.out.println("Country Set Size: " + collectedSet.size());

        // anyMatch(), allMatch(), noneMatch(): Check if any, all, or none of the elements match a given predicate.
        boolean anyMatch = countryList.stream().anyMatch(x -> x.startsWith("U"));
        System.out.println("Found a country which starts with `U`: " + anyMatch);

        // findFirst(), findAny(): Returns the first or any element of the stream, or an empty Optional if the stream is empty.
        Optional<String> firstElement = countryList.stream().findFirst();
        System.out.println("First country in the stream: " + firstElement);

        // min(), max(): Returns the minimum or maximum element of the stream based on a comparator.
        List<Integer> intNumbers = Arrays.asList(1, 2, 3, 4, 5);
        Optional<Integer> min = intNumbers.stream().min(Comparator.naturalOrder());
        System.out.println("Min number in the list: " + min);


        // Example-4: Stream on HashMap
        Map<String, Integer> countryToGDPMap = new HashMap<>();
        countryToGDPMap.put("Bangladesh", 416);
        countryToGDPMap.put("India", 3176);
        countryToGDPMap.put("USA", 25462);

        // Collect entries into a new map where the key is the original key in uppercase
        Map<String, Integer> upperCaseMap = countryToGDPMap.entrySet().stream()
                .collect(Collectors.toMap(entry -> entry.getKey().toUpperCase(), Map.Entry::getValue));
        System.out.println(upperCaseMap);

        // Double the GDP of each country
        Map<String, Integer> doubledGdpMap = countryToGDPMap.entrySet().stream()
                .map(entry -> Map.entry(entry.getKey(), entry.getValue() * 2))
                .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
        System.out.println(doubledGdpMap);


        // Example-5: A complex example with a customer `Person` object.
        List<Person> people = Arrays.asList(
                new Person("Ava", 25, "Female"),
                new Person("John", 30, "Male"),
                new Person("Meghan", 22, "Male"),
                new Person("Megha", 28, "Female"),
                new Person("Biral", 35, "Female"),
                new Person("Goru", 28, "Male")
        );

        // Filter and collect female persons aged 20 or older into a new list
        List<Person> femaleOlderThan20 = people.stream()
                .filter(person -> person.getGender().equals("Female"))
                .filter(person -> person.getAge() >= 20)
                .collect(Collectors.toList());

        System.out.println(femaleOlderThan20);
    }
}
```

**Person Class:**

```java
public class Person {
    private String name;
    private int age;
    private String gender;

    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getGender() {
        return gender;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }
}

```

