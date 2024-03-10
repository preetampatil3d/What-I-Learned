# Exception

![types-of-exception-in-java](https://github.com/preetampatil3d/What-I-Learned/assets/21255598/b751ef0f-54f3-4078-9978-f834636906a7)

1. Checked Exception: Compile Time
   - Exception that are checked at compile time. and must handle in order to compile.
   - IDE will force to add try-catch block or throw exception
2. Unchecked Exception: Run Time
   - Exception that are not checked at compile time. It will occure at Run Time
   - With some bad data we will encounter Unchecked Exception.
  

> Custom Exception
```
public class CustomException extends Exception{
  public void CustomException(String error){
    super(error);
  }
}

public class MainClass{
  piblic static void main(String args[]){
    int i = 0;
    if(i == 0)
        throw new CustomException("Custom Error message");

  }
}

```

# Throw , Throws , Throwable
## Throw
- Used to **throw Exception manually** from inside any method, But that Exception must of type java.lang.Exception
```
public void demo() throws Exception{
   Exception e = new Exception();
   throw e;
}
```
## Throws
- Used in method signature to indicate that method may **throw** .
```
public void demo() throws Exception
```

## Throwable 
- Throwable is super class of all Exception and Errors.
- If we want create our own custom Exception then need to extend Throwable.
- Throwable is super class of Error & Exception. We can create Custom exception by extending Exception/Throwable
```
public class CustomException extends Throwable{
    public void CustomException(String error){
    super(error);
  }
}

public class MainClass{
  piblic static void main(String args[]){
    int i = 0;
    if(i == 0)
        throw new CustomException("Custom Error message");
  }
}
```

