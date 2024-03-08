# Stream 
- Sequence of objects which operates on data source such as Array, Collection.

1. Sequencial Stream 
	- Objects are pipelined in single Stream and Use single thread to process pipeling / request on single code.
	- By default all stream operations are sequencial , Unless it is specified as parallel
	
2. Parallel Stream 
	- Object are pipelined into multiple Streams , which executed parallely on separate core.
	- which will increase performance.
	- Disadvantages: Order of the result is unpredictable , Error-prone.
	- Ways to use
		- parallelStream() from Collection
		- parallel() from Stream
	
| Sequencial | Parallel |
| --- | --- |
| Run on Single core (System/Computer) | Divide into multiple Core
| Order maintained | No Order maintained |
| Single Iteration at time | Multiple iteration simultaniously on multiple Core |
| Reliable and less error | Less Reliable and Error-prone |


## Stream Creation explicitly
```
Stream<Integer> stream = Stream.of(1,2,3);
Stream<Integer> stream = Stream.of(new Integer[]{1,2,3});

//sequential stream
Stream<Integer> sequentialStream = data.stream();
		
//parallel stream
Stream<Integer> parallelStream = data.parallelStream();

// Create Stream Array
String[] array = {"a","b"}
Arrays.stream(array) 
```	
	
# Stream API 

input -> 
Operation 1 -> Operation 2 (Intermediate) ->
Intermediate Operations 
-> Output

## Operations:
- Intermediate Operations
- Terminate Operations

### Intermediate Operations
> filter() - filter input stream/Collection with given filter 
```
data.stream().filter(n -> n < 3 ).collect(Collectors.toList()); // 1 , 2
```

> sorted() - 
```
data.stream().sorted().collect(Collectors.toList()); // Default ASC
data.stream().sorted(Comparator.reverseOrder()).collect(Collectors.toList()); // Reverse Order - DESC

// EmployeePojo: id, name, age
List<Employee> empData;
empData.stream().sorted(Comparator.comparing(Employee::getName)).collect(Collectors.toList()); // Sort by name - ASC
empData.stream().sorted(Comparator.comparing(Employee::getName).reversed()).collect(Collectors.toList()); // Sort by name - DESC

empData.stream()
.sorted(Comparator.comparing(Employee::getName).thenComparing(Employee::getAge)).collect(Collectors.toList()) // Sort by name and age - ASC

```

> map() : Apply given function to elements of stream,  Apply changes back to individual item
- It produces a new stream for each element. It transforms all the streams into a single stream to provide the result. therefore, each element of the stream gets converted into a new stream
```
List<Integer> data = Array.asList(1,2,3,4,5);
List<Integer> squar = data.stream(n -> n*n).collect(Collectors.toList());
```

> flatmap() : 
- Used to convert a Stream of Stream into a list of values
- Each iteration of each element the map() method creates a separate new stream. By using the flattening mechanism
- flatMap() = Flattering (flat) + mapping (map)
	- Flattering : Process of converting serveral list of list and merge all those list to create a single list containing all elements from all list 
		- [(1,2),(2,3)] => [1,2,3,4]
- We can use a flatMap() method on a stream with the mapper function List::stream. On executing the stream terminal operation, each element of flatMap() provides a separate stream. In the final phase, the flatMap() method transforms all the streams into a new stream. In the below stream, we observe that it does not contain duplicate values.
```
List country = Stream.of(Arrays.asList(1,2,3,4), Arrays.asList(3,4,5,6))
.flatMap(List::stream) .collect(Collectors.toList()); 

```

| map | flatMap |
| --- | --- |
| process stream of values | process stream of stream's value |
| Perform mapping only | performs mapping along with flattering |
| uses one-to-one mapping | uses one-to-many mapping |
| Its mapper function produces single value for each input value | its mapper function produces multiple values (stream of values) for each input value |
| use map() when mapper produces single value for each input value of stream | when producing multiple values for each input value |


### Terminate Operations
> collect() Return the result of intermediate operation 
```
.collect(Collectors.toList());
.collect(Collectors.toSet());
```

> forEach()
```
.forEach(x -> System.out::println(x));
```

> reduce() Perform operation on each element to get Single result. return Optional - https://www.javatpoint.com/reduce-java
- first argument operator : return the value of the previous application 
- second argument : return the current stream element.
- Arguments
	- identity - initial value of reduction operation.
	- accumulator - combine two values i.e. partial result of the reduction operation and the next element of the stream
	- combiner: It combines the partial result of the reduction operation
```
// Example 1 - find max length word (?)
// reduce(BinaryOperator<T> accumulator)
ArrayList<String> words = Arrays.asList("aa","aaaa","a"); 
Optional<String> longestString = words.stream()
                             .reduce((word1, word2) 
                             -> word1.length() > word2.length() 
                                           ? word1 : word2); 
										   
// Addition of numbers
// reduce(T identity, BinaryOperator<T> accumulator)
List<Integer> numbers = Arrays.asList(1,2,3);
sum = numbers.stream().reduce(0, 
						(element1,element2) -> element1 + element2);
						
// Add factorial Example 

```

## IntStream : A sequence of primitive int-values
- static IntStream of(int... values)
- 

```
// Create 
IntStream stream = IntStream.of(-7, -9, -11);

//Will iterate from 0 to 5
IntStream.range(0,6)
            .forEach(System.out::println);
 
//Will iterate from 0 to 6
IntStream.rangeClosed(0,6)
	.forEach(System.out::println);
 
//rangeSum=0+1+2+3+...+99=4950 (sum[0,100])
int rangeSum=IntStream.range(0,100).sum();
out.println("sum[0,100) : "+rangeSum);
 
//rangeClosedSum=0+1+2+3+...+100=5050
int rangeClosedSum=IntStream.rangeClosed(0,100).sum();
out.println("sum[0,100] : "+rangeClosedSum);

```

# Comparable Vs Comparator

|Comparable | Comparator |
| --- | --- |
| 1) Comparable provides a single sorting sequence. In other words, we can sort the collection on the basis of a single element such as id, name, and price. | The Comparator provides multiple sorting sequences. In other words, we can sort the collection on the basis of multiple elements such as id, name, and price etc. |
| 2) Comparable affects the original class, i.e., the actual class is modified. | Comparator doesn't affect the original class, i.e., the actual class is not modified. |
| 3) Comparable provides compareTo() method to sort elements. | Comparator provides compare() method to sort elements. |
| 4) Comparable is present in java.lang package. | A Comparator is present in the java.util package. |
| 5) We can sort the list elements of Comparable type by Collections.sort(List) method. | We can sort the list elements of Comparator type by Collections.sort(List, Comparator) method. |




# Common Examples
```
List<Integer> numberList = new ArrayList(Arrays.asList(1,2,3));
int sum = numberList.stream().mapToInt(x -> x).sum();
```
