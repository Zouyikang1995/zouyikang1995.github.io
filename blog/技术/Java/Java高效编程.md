# 剑指高效编程，告别996

## 函数式编程
### 函数编程演变史
* 实战案例：根据实际场景演示，为了应对不断变化的业务需求，代码的多个改进版本。  
1. 

* Lambda表达式简介
1. Java8引入函数式编程风格
2. 可以理解为一种匿名函数的代替
3. 通过行为参数化传递代码
4. 书写方式：
   * (parameters) -> expression
   * (parameters) -> {statement;}

* 函数式接口
1. 接口中只有一个抽象方法
2. Java8的函数式接口注解：@FunctionInterface
3. 函数式接口的抽象方法签名：函数描述符

## 流式编程

## Java Functional Programming
### Imperative Programming and Declarative Programming
`Imperative Programming`:        
It is a type of programming paradigm that describes how the program executes. Developers are more concerned with how to get an answer step by step. It comprises the sequence of command imperatives. In this, the order of the execution is very important and uses both mutable and immutable data. Fortran, Java, C, C++ programming languages are examples of imperative programming.  
* Procedural programming Paradigm
* Object Oriented Programming
* Parallel Processing Approach

`Declarative Programming`:      
It is a type of programming paradigm that describes what programs to be executed. Developers are concerned with the answer that is received. It declares what kind of results we want and leave programming language aside focusing on simply figuring out how to produce them. In simple words, it mainly focus on end result. It expresses the logic of computation. Miranda, Erlang, Haskell, Prolog are a few popular examples of declarative programming. 
* Logic Programming Paradigm
* Functional Programming 
* Database Processing Approach

Data Source: [Difference Between Imperative and Declarative Programming](https://www.geeksforgeeks.org/difference-between-imperative-and-declarative-programming/)            

### Functional interfaces: java.util.function
1. `Predicate<T>`: Represents a predicate (boolean-valued function) of one argument.      
   - T : the type of the input to the predicate  
2. `Function<T,R>`: Represents a function that accepts one argument and produces a result.  
   - T : the type of the input to the function   
   - R : the type of the result of the function    
3. `BiFunction<T, U, R>`: Represents a function that accepts two arguments and produces a result. This is the two-arity specialization of Function.   
   - T - the type of the first argument to the function  
   - U - the type of the second argument to the function  
   - R - the type of the result of the function   
4. `Consumer<T>`: Represents an operation that accepts a single input argument and returns no result. Unlike most other functional interface, Consumer is expected to operate via side-effects.   
   - T - the type of the input to the operation  
5. `BiConsumer<T, U>`: Represents an operation that accepts two input arguments and returns no result. This is the two-arity specialization of Consumer.  
   - T - the type of the first argument to the operation 
   - U - the type of the second argument to the operation  
6. 


JDK documentation: [java.util.function](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/function/package-summary.html)        

### Examples  
#### imperative programming and declarative programming.     
```java
public static void main(String[] args) {
        List<Person> people = List.of(
                new Person("John", Gender.MALE),
                new Person("Maria", Gender.FEMALE),
                new Person("Aisha", Gender.FEMALE),
                new Person("Alex", Gender.MALE),
                new Person("Alice", Gender.FEMALE)
        );

        // Imperative approach
        System.out.println("// Imperative approach");
        List<Person> females = new ArrayList<>();

        for (Person person : people) {
            if (Gender.FEMALE.equals(person.gender)) {
                females.add(person);
            }
        }

        for (Person female : females) {
            System.out.println(female);
        }

        // Declarative approach
        System.out.println("// Declarative approach");
        people.stream()
                .filter(
                        person -> person.gender.equals(Gender.FEMALE)
                )
                .forEach(System.out::println);
    }

    static class Person{
        private final String name;
        private final Gender gender;

        Person(String name, Gender gender) {
            this.name = name;
            this.gender = gender;
        }

        @Override
        public String toString() {
            return "Person{" +
                    "name='" + name + '\'' +
                    ", gender=" + gender +
                    '}';
        }
    }

    enum Gender{
        MALE,FEMALE
    }
```
1. The basic way to use Function filter().   
   Function filter() needs a Predicate<T> class. We can use `Anonymous Inner Class`.
```java
// Declarative approach
        System.out.println("// Declarative approach");
        people.stream()
                .filter(
                        new Predicate<Person>() {
                            @Override
                            public boolean test(Person person) {
                                return person.gender.equals(Gender.FEMALE);
                            }
                        }
                )
                .forEach(System.out::println);
```

2. To modify the anonymous inner class in `lambda expression`.   
* The first way to write in lambda expression.     
```java
// Declarative approach
        System.out.println("// Declarative approach");
        people.stream()
                .filter(
                        (Person person) -> {return  person.gender.equals(Gender.FEMALE);}
                )
                .forEach(System.out::println);
```

* The second way to write in lambda expression.     
```java
// Declarative approach 
people.stream()
                .filter(
                        (Person person) -> person.gender.equals(Gender.FEMALE)
               )
                .forEach(System.out::println);
```

* The third way to write in lambda expression.   
```java
// Declarative approach
System.out.println("// Declarative approach");
        people.stream()
                .filter(
                         person -> person.gender.equals(Gender.FEMALE)
                )
                .forEach(System.out::println);
```

#### Examples of Function<T,R>      
```java
public static void main(String[] args) {
        // Function takes 1 argument and produces 1 result
        int increment = incrementByOne(1);
        System.out.println(increment);

        int increment2 = incrementByOneFunction.apply(1);
        System.out.println(increment2);

        int multiply = multiplyBy10Function.apply(increment2);
        System.out.println(multiply);

        Function<Integer, Integer> addBy1AndThenMultiplyBy10 = incrementByOneFunction.andThen(multiplyBy10Function);
        System.out.println(addBy1AndThenMultiplyBy10.apply(1));

        System.out.println(incrementByOneFunction.andThen(multiplyBy10Function).apply(4));
    }

    static Function<Integer, Integer> incrementByOneFunction = new Function<Integer, Integer>() {
        @Override
        public Integer apply(Integer integer) {
            return integer + 1;
        }
    };

    static Function<Integer, Integer> multiplyBy10Function = number -> number * 10;

    static int incrementByOne(int number) {
        return number + 1;
    }
```

#### Examples of BiFunction<T, U, R>
```java
public static void main(String[] args) {
        // Function takes 2 arguments and produces 1 result
        System.out.println(incrementByOneAndMultiply(4, 100));
        System.out.println(incrementByOneAndMultiplyBiFunction.apply(4, 100));
    }

    static BiFunction<Integer, Integer, Integer> incrementByOneAndMultiplyBiFunction = new BiFunction<Integer, Integer, Integer>() {
        @Override
        public Integer apply(Integer integer, Integer integer2) {
            return (integer + 1) * integer2;
        }
    };

    static int incrementByOneAndMultiply(int number, int numToMultiplyBy) {
        return (number + 1) * numToMultiplyBy;
    }
```

#### Examples of Consumer<T>
```java
public static void main(String[] args) {
        // Normal Java function
        Customer maria = new Customer("Maria", "99999");
        greetCustomer(maria);

        // Consumer Functional interface
        greetCustomerConsumer.accept(maria);
    }

    static Consumer<Customer> greetCustomerConsumer = customer -> System.out.println("Hello " + customer.customerName + ", thanks for registering phone number " + customer.customerPhoneNumber);

    static void greetCustomer(Customer customer) {
        System.out.println("Hello " + customer.customerName + ", thanks for registering phone number " + customer.customerPhoneNumber);
    }

    static class Customer{
        private final String customerName;
        private final String customerPhoneNumber;

        Customer(String customerName, String customerPhoneNumber) {
            this.customerName = customerName;
            this.customerPhoneNumber = customerPhoneNumber;
        }
    }
```

## Streams Tutorial: Streams, Filter, Map, Reduce   
### Description  
* Streams bring functional programming to Java, and are supported starting in Java 8. 
* Advantages of Streams:
  * Will make you a more efficient Java programmer
  * Make heavy use of *lambda expressions*
  * ParallelStreams make it very easy to multi-thread operations
* A stream pipeline consists of a source, followed by zero or more intermediate operations; and a terminal operation. 

```graph LR
Source --> Filter --> Sort --> Map --> Collect -->
```

* Stream Source 
  * Streams can be created from Collections, Lists, Sets, ints, longs, doubles, arrays, lines of a file

* Stream operations are either intermediate or terminal.
  * *Intermediate operations* such as filter, map or sort return a stream so we can chain multiple intermediate operations.
  * *Terminal operations* such as forEach, collect or reduce are either void or return a non-stream result. 

### Intermediate Operations  
* Zero or more intermediate operations are allowed.    
* Order matters for large datasets: *filter first*, then sort or map.     
* For very large datasets use ParallelStream to enable multiple threads.    
* Intermediate operations include:      
  * anyMath(), flatmap(), distinct(), map(), filter(), skip(), findFirst(), sorted().

### Terminal Operations    
One terminal operation is allowed.   
* *forEach* applies the same function to each element.   
* *collect* saves the elements into a collection.   
* other options *reduce* the stream to a single summary element.   
  * count(), min(), max(), reduce(), summaryStatistics(). 

### Examples
#### How to use
1. Integer Stream
```java
IntStream
                .range(1, 10)
                .forEach(System.out::print);
        System.out.println();
```
2. Integer Stream with skip
```java
IntStream.range(5,10)
                .skip(2)
                .forEach(x-> System.out.print(x));
        System.out.println();
```
3. Integer Stream with sum
```java
System.out.println(
                IntStream.range(3,5)
                .sum());
```
4. Stream of, sorted and findFirst
```java
Stream.of("Ava","Aneri","Alberto")
                .sorted()
                .findFirst()
                .ifPresent(System.out::println);
```
5. Stream from Array, sort, filter and print
```java
String[] names = {"Al", "Ankit", "Kushal", "Brent", "Sarika", "amanda", "Hans", "Shivika", "Sara"};
        Arrays.stream(names)
                .filter(x -> x.startsWith("S"))
                .sorted()
                .forEach(System.out::println);
```
6. average of squares of an int array
```java
Arrays.stream(new int[]{1, 2, 3, 4, 5})
                .map(x -> x * x)
                .average()
                .ifPresent(System.out::println);
```
7. Stream from List, filter and print
```java
List<String> people=Arrays.asList("Al", "Ankit", "Kushal", "Brent", "Sarika", "amanda", "Hans", "Shivika", "Sara");
        people.stream()
                .map(String::toLowerCase)
                .filter(x -> x.startsWith("a"))
                .forEach(System.out::println);
```
8. Stream rows from text file, sort, filter, and print
```java
Stream<String> bands = Files.lines(Paths.get("bands.txt"));
        bands.sorted()
                .filter(x -> x.length() > 13)
                .forEach(System.out::println);
        bands.close();
```
9.  Stream rows from text file and save to List
```java
List<String> bands2 = Files.lines(Paths.get("bands.txt"))
                .filter(x -> x.contains("Ja"))
                .collect(Collectors.toList());
        bands2.forEach(System.out::println);
```
10. Stream rows from CSV file and count
```java
Stream<String> rows1 = Files.lines(Paths.get("data.txt"));
        int rowCount = (int) rows1.map(x -> x.split(","))
                .filter(x -> x.length == 3)
                .count();
        System.out.println(rowCount + " rows.");
        rows1.close();
```
11. Stream rows from CSV file, parse data from rows
```java
Stream<String> rows2 = Files.lines(Paths.get("data.txt"));
        rows2.map(x -> x.split(","))
                .filter(x -> x.length == 3)
                .filter(x -> Integer.parseInt(x[1]) > 15)
                .forEach(x -> System.out.println(x[0] + "   " + x[1] + "   " + x[2]));
        rows2.close();
```
12. Stream rows from CSV file, store fields in HashMap
```java
Stream<String> rows3 = Files.lines(Paths.get("data.txt"));
        Map<String, Integer> map = new HashMap<>();
        map = rows3.map(x -> x.split(","))
                .filter(x -> x.length == 3)
                .filter(x -> Integer.parseInt(x[1]) > 15)
                .collect(Collectors.toMap(x -> x[0], x -> Integer.parseInt(x[1])));
        rows3.close();
        for (String key : map.keySet()) {
            System.out.println(key + "   " + map.get(key));
        }
```
13. Reduction - sum
```java
double total = Stream.of(7.3, 1.5, 4.8)
                .reduce(0.0, (Double a, Double b) -> a + b);
        System.out.println("Total = " + total);
```
14. Reduction - summary statistics
```java
IntSummaryStatistics summaryStatistics = IntStream.of(7, 2, 19, 88, 73, 4, 10)
                .summaryStatistics();
        System.out.println(summaryStatistics);
```

#### data format
1. `bands.txt`
```txt
Polling Joes
Lady Gaga
Jackson Browne 
Maroon 5
Elton John 
John Mayer
CCR
Eagles
Pink 
Aerosmith
Adele
Taylor Swift
Faye WOng
Bob Seger
CodlePlay
Boston
The Cars
Cheap Trick
Def Lepard
Ed Sheeran
Dire Straint
Train
Tom Petty
Jack Johnson
Jimmy Buffett
Mumford and Sons
aPhil Collins
Pod Stewart
The Script
The end
```
2. `data.txt`
```txt
A,12,3.7
B,17,2.8
C,14,1.9
D,23,2.7
E
F,18,3.4
```

## 线程池精进

## 资源关闭

## 工具集

## 验证框架

## 使用工具

## 开发神器

## 自测工具