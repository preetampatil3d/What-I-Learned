# Functional Interface
- Interface with only one abstract method declaration.
- Can have any number of default methods
- Class annotate with @FunctionalInteface , Not mandatory

> [!NOTE]
> Functional Interface only extends another interface that does not have any abstract methods.

## Predicate : Used to test / evaluate, return boolean based on condition
```
Predicate<Integer> lessthan = (a) -> a < 100;
lessthan.test(5); // true
```
> Custom
```
public class CustomPredicate impletents Predicate<Integer>{
  @override
  boolean test(Integer a){
    return a < 100;
  }
}
Class MainClass{
  public static void main(String args[]){
      CustomPredicate p1 = new CustomPredicate();
      p1.test();
  }
}
```

## Function: Accepts one Argument and returns a result.
use andThen when you want to apply the functions in the order they are defined and use compose when you want to apply the functions in the opposite order.
```
Function<Integer, Double> f1 = (a) -> a / 2.0;
f1 = f1.andThen( a -> a*2)
f1 = f1.compose( a -> a*2)

f1.apply(5);
```

## Consumer: Accepts One Argument, does process and returns nothing.
```
Consumer<Integer> c1 = a -> System.out.println(a);
consumer.accept(10);

Consumer<Integer> c2 = a -> System.out.println(a * a);
c1.andThen(c2).accept(10)
```

## Supplier: Accept nothing and return some value;
```
Supplier<Double> s1 = () -> Math.random();
s1.get()
```