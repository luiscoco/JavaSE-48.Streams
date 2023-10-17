# JavaSE-Streams

The java.util.stream package in Java provides a set of classes to perform operations on sequences of elements, such as collections. It introduces the concept of streams, which represent a sequence of elements and allow you to express common operations (like filtering, mapping, or reducing) in a concise and expressive way.

Let's go through some basic examples:

Example 1: Filtering elements
Suppose you have a list of numbers and you want to filter out the even numbers:

java
Copy code
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> evenNumbers = numbers.stream()
                                           .filter(n -> n % 2 == 0)
                                           .collect(Collectors.toList());

        System.out.println(evenNumbers);
    }
}
In this example, numbers.stream() converts the list into a stream, .filter(n -> n % 2 == 0) filters out the even numbers, and .collect(Collectors.toList()) collects the results into a new list.

Example 2: Mapping elements
Let's say you want to square each number in the list:

java
Copy code
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        List<Integer> squaredNumbers = numbers.stream()
                                             .map(n -> n * n)
                                             .collect(Collectors.toList());

        System.out.println(squaredNumbers);
    }
}
Here, .map(n -> n * n) applies the square function to each element in the stream.

Example 3: Reducing elements
Suppose you want to find the sum of all numbers in the list:

java
Copy code
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        int sum = numbers.stream()
                        .reduce(0, Integer::sum);

        System.out.println(sum);
    }
}
In this case, .reduce(0, Integer::sum) accumulates the elements by summing them up, starting with an initial value of 0.

These are just basic examples, and the java.util.stream package offers many more powerful features for working with collections in a functional and declarative style.




User
can you provide me more samples
ChatGPT
Absolutely! Let's explore some more examples with java.util.stream:

Example 4: Finding the Average
Suppose you want to find the average of a list of numbers:

java
Copy code
import java.util.Arrays;
import java.util.List;
import java.util.OptionalDouble;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        OptionalDouble average = numbers.stream()
                                       .mapToInt(Integer::intValue)
                                       .average();

        if (average.isPresent()) {
            System.out.println("Average: " + average.getAsDouble());
        } else {
            System.out.println("List is empty");
        }
    }
}
In this example, .mapToInt(Integer::intValue) converts the stream of integers to an IntStream, and .average() calculates the average.

Example 5: Filtering and Limiting
Filtering and limiting the number of elements in a stream:

java
Copy code
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "orange", "grape", "watermelon");

        List<String> shortWords = words.stream()
                                      .filter(word -> word.length() <= 5)
                                      .limit(2)
                                      .collect(Collectors.toList());

        System.out.println(shortWords);
    }
}
Here, .filter(word -> word.length() <= 5) filters words with length less than or equal to 5, and .limit(2) restricts the result to the first 2 elements.

Example 6: Grouping by Property
Grouping a list of people by their age:

java
Copy code
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return name + " - " + age;
    }
}

public class StreamExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
                new Person("Alice", 25),
                new Person("Bob", 30),
                new Person("Charlie", 25),
                new Person("David", 30)
        );

        Map<Integer, List<Person>> peopleByAge = people.stream()
                                                      .collect(Collectors.groupingBy(Person::getAge));

        System.out.println(peopleByAge);
    }
}
In this example, .collect(Collectors.groupingBy(Person::getAge)) groups people by their age.

These examples illustrate some of the powerful capabilities of the java.util.stream package in Java for processing collections in a functional and expressive manner.




User
can you explain me more advance topics about "java.util.stream" in Java
ChatGPT
Certainly! Let's delve into some more advanced topics related to java.util.stream in Java.

1. FlatMap for Nested Collections:
If you have a stream of collections and you want to flatten them into a single stream, you can use flatMap. This is particularly useful when dealing with nested data structures:

java
Copy code
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<List<Integer>> nestedNumbers = Arrays.asList(
                Arrays.asList(1, 2, 3),
                Arrays.asList(4, 5, 6),
                Arrays.asList(7, 8, 9)
        );

        List<Integer> flattenedNumbers = nestedNumbers.stream()
                                                      .flatMap(List::stream)
                                                      .collect(Collectors.toList());

        System.out.println(flattenedNumbers);
    }
}
Here, flatMap(List::stream) transforms each list in the stream into a stream of its elements, and then flattens these streams into a single stream of integers.

2. Parallel Streams:
Java streams can easily be parallelized to take advantage of multi-core processors. You can convert a sequential stream to a parallel stream using the parallel() method:

java
Copy code
import java.util.Arrays;

public class StreamExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        int sum = Arrays.stream(numbers)
                        .parallel()
                        .sum();

        System.out.println("Sum: " + sum);
    }
}
However, keep in mind that not all operations are suitable for parallelization, and parallel streams come with some overhead. It's crucial to test and profile your application to ensure that parallelization provides a performance benefit.

3. Custom Collectors:
You can create your own custom collectors using the Collector interface. This is useful when you want to perform a custom reduction operation or combine the elements in a specific way:

java
Copy code
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collector;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "orange", "grape", "watermelon");

        String concatenated = words.stream()
                                  .collect(CustomCollectors.joining("; "));

        System.out.println(concatenated);
    }
}

class CustomCollectors {
    public static Collector<CharSequence, ?, String> joining(CharSequence delimiter) {
        return Collectors.joining(delimiter);
    }
}
In this example, a custom collector named CustomCollectors.joining is created to join elements with a custom delimiter.

4. Infinite Streams:
Streams can be infinite, meaning they have no fixed size. You can create infinite streams using methods like Stream.iterate or Stream.generate. Be cautious when working with infinite streams to avoid infinite loops:

java
Copy code
import java.util.stream.Stream;

public class StreamExample {
    public static void main(String[] args) {
        // Generate an infinite stream of even numbers
        Stream.iterate(0, n -> n + 2)
              .limit(10)
              .forEach(System.out::println);
    }
}
In this example, Stream.iterate(0, n -> n + 2) generates an infinite stream of even numbers starting from 0.

These advanced topics showcase the flexibility and power of the java.util.stream package in Java. Experimenting with these features can lead to more concise and expressive code when working with collections.

