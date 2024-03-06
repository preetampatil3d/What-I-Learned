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


# Maker/tagging Interface : Interface that dont have any methods / Empty
- It Provides run time information about object, So compiler and JVM have addtional information about object.
- Example : Clonable , Serialisable
- If we try to clone/Serialize Object which has not implemented clonable/serializable. Then JVM will throw and error.

- Custom
```
// create maker ineterface
public interface deletable{} 

// Implement to add this tag/metadata to indicate that this is deletable
public Class UserEntity implements Deletable{ 
}

// Add check in Dao to skip delete if deletable in implemented
public Class DataDao{
  public boolean delete(Object obj){
    if(! obj.isInstanceOf(deletable)){
      return false;
    }
    // delete implementaion
    return true;
  }
}
```

## Serialization / Deserialization : By converting byteStream we can send over network & save in DB.
- Serialization : Converting state of object into ByteStream. Derialization : ByteStream is converted into actual object.
- serialversionUID : This version Id is associated with each serializable class which is used while deserialization to check and verify compatability between sender and receiver. In case of missmatch , It will encounter 'InvalidClassExcepption'
```
  private static final long serialversionUID = 129348938L;
```

> [!NOTE]
> Only non-static member are serialized
> static and transient data members are not saved in Serialization process.
> Class must implement Serializable, If Parent class has implemented. It is not required for child.

  ![serialize-deserialize-java](https://github.com/preetampatil3d/What-I-Learned/assets/21255598/f1983259-26ce-45e1-97c6-1283a738c28a)

  
```
public class UserData implement Serializable{
  String name;
  Int Age;
}

public class MainClass{
  public static void main(String arr[]){
    // Serialize - write object to file
    UserData data = new userData("Name", 25);
    FileOutputStream fo = new FileOutputStream("file-path.txt")
    ObjectOutputStream oo = new ObjectOutputStream(fo);

    oo.writeObject(data);
    fo.close();
    oo.close();

    // Deserialize - read and create actual object
    FileInputStream fi = new FileInputStream("file-path.txt");
    ObjectInputStream oi = new ObjectInputStream(fi);

    UserData data1 = (UserData) oi.readObject(oi);
    fi.close();
    oo.close(); 
  }
}
```


 
