## Generics

- Generics allow you to write classes, interfaces, and methods that work with different data types, without having to specify the exact type in advance.


### 1. Generic Classes
```declarative
// Example:
class Box<T> {
    T value; // T is a placeholder for any data type
    void set(T value) {
        this.value = value;
    }
    T get() {
        return value;
    }
}
// ----
Box<String> stringBox = new Box<>();
stringBox.set("Hello");

Box<Integer> intBox = new Box<>();
intBox.set(50);
```

#### 2. Generic Methods
```declarative
// Example:
public <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}

// ---
String[] names = {"Jenny", "Liam"};
printArray(names);

Integer[] numbers = {1, 2, 3};
printArray(numbers);
```

#### 3. Bounded Type Parameters
- You can use the extends keyword to limit the types a generic class or method can accept.
- For example, you can require that the type must be a subclass of Number:
```declarative
// Example:
public <T extends Number> void printNumber(T number) {
    System.out.println(number);
}
// ---
printNumber(10); // Integer
printNumber(10.5); // Double
```

### 4. Wildcards 
```declarative
// Example:
public void printList(List<?> list) {
    for (Object element : list) {
        System.out.println(element);
    }
}
// ---
List<String> stringList = Arrays.asList("A", "B", "C");
List<Integer> intList = Arrays.asList(1, 2, 3);
printList(stringList);
printList(intList);
```

