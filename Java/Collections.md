

## Default methods present in Collecrtion

| Method | Description |
| --- | ---|
|	public boolean add(E e) |	It is used to insert an element in this collection. |
|	public boolean addAll(Collection<? extends E> c)  |	It is used to insert the specified collection elements in the invoking collection. |
|	public boolean remove(Object element) |	It is used to delete an element from the collection. |
|	public boolean removeAll(Collection<?> c) |	It is used to delete all the elements of the specified collection from the invoking collection. |
|	default boolean removeIf(Predicate<? super E> filter) |	It is used to delete all the elements of the collection that satisfy the specified predicate. |
|	public boolean retainAll(Collection<?> c) |	It is used to delete all the elements of invoking collection except the specified collection. |
|	public int size() |	It returns the total number of elements in the collection. |
|	public void clear() |  It removes the total number of elements from the collection. |
|	public boolean contains(Object element) |	It is used to search an element. |
|	public boolean containsAll(Collection<?> c) |	It is used to search the specified collection in the collection. |
|	public Iterator iterator() |	It returns an iterator. |
|	public Object[] toArray() |	It converts collection into array. |
|	public <T> T[] toArray(T[] a) |	It converts collection into array. Here, the runtime type of the returned array is that of the specified array. |
|	public boolean isEmpty() |	It checks if collection is empty. |
|	default Stream<E> parallelStream() |	It returns a possibly parallel Stream with the collection as its source. |
|	default Stream<E> stream() |	It returns a sequential Stream with the collection as its source. |
|	default Spliterator<E> spliterator() |	It generates a Spliterator over the specified elements in the collection. |
|	public boolean equals(Object element) |	It matches two collections. |
|	public int hashCode() |	It returns the hash code number of the collection. |

# List : ArrayList , LinkedList , Victor
## 1. ArrayList 
- Default size is 10 , When threshold reached increase size by 50%
- Duplicate and Null insertion are allowed
- Insertion order preserved

> Creation
```
ArrayList arr = new ArrayList();
ArrayList arr = new ArrayList(CollectionObj);
ArrayList arr = new ArrayList(Size);
```

>!NOTE 
Arrays.asList() - Fixed size list is created 
List.of() - Immutable list is created 

```
// Cannot add/remove above list.
// Inorder to created Mutable list
List<Integer> numberList = new ArrayList<>(Arrays.asList(1,2,3));

numberList.remove(1);
```

## 2. LinkedList 
- 
Head -> points to first
Node [DATA,Pointer] - Pointer points to memory of next Node
[DATA, null] Last node pointer is always null
- Methods specific to Linked list 
	- AddFirst, AddLast , removeFirst, removeLast, getFirst, getLast


| ArrayList | LinkedLIst |
| --- | --- |
| Internally uses ** Dynamic array ** to store data | Internally use ** Doubly  linked list ** to store data |
| Manipulation is ** slow ** , As it internally uses Array. If any elements is removed , All other elements are shifted in memory | ** faster ** , as it uses Doubly Linked list. So no bit shifting is done when elements are removed |
| Implements List | Implements List & Queue
| Better for sorting & Accessing | Better for manipulation | 
| Default size is 10 |  No default size | 


## 3. Vector 

| ArrayList | Vector |
| --- | --- |
| Not Sychronized | Sychronized (Thread-safe) | 
| Multiple Thread are allowed | Single thread is allowed |
| Increase size by 50% When reach threshold | Increase size by 100% When reach threshold | 
| Jdk 1.2 | Legacy |
| fast , because Not Sychronized | Slow because of Sychronized behaviour. In case of multithreading, It holds other Thread in waiting   | 


# Set : Store unique values 
-  HashSet, LinkedHashSet implements Set
-  Set <- SortedSet (I) <- NavigableSet (I) <- TreeSet 

## 1. HashSet 
- No insertion order maintained
- uses ** Hash Table ** data structure 


# Coparator / Comparable



