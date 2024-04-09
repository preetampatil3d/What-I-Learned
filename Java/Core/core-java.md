## String/ array of character/ Immutable Object
- String is refered as immutable because its value cannot be changed. 
	- Instead whenever value changed, new object is created and old value is copied.
- All Warpper classes are immutable and Thread safe.

- Mutable String : Create String using StringBuilder(**Not Thread-Safe**) and StringBuffer (**Thread-Safe**).
- Advantages : 
	- Security: sensitive data like username, password, connection can be stored and which can not be manupulated by hackers.
	- Sychronization: Can be shared with other threads and be asured data can not be modified
	- Performance: Due to String Pool
 - Example
```
String a1 = "Hi"; // Stored at new memory/String Pool
String a2 = "Hi"; // Search for 'Hi' in String Pool and points to previously created pool.
String a3 = new String("Hi"); // It will Not search in Pool instead it will be stored at new memory location.
String a4 = new String("Hi").itern(); // intern will will search string in pool

if(a1 == a2) // true
if(a1 == a3) // false
if(a1 == a4) // true
if(a2 == a3) //
if(a2 == a3) // 
```
   


## Static Keyword
> Static Variable
- Sometimes, you want to have variables that are common to all objects. This is accomplished with the static modifier. Fields that have the static modifier in their declaration are called static fields or class variables.
- static variables are stored in data segment. you may create any number of instance the same variable from same location is used.
that is only one copy of static variable will be created at the time of class loading no matter how many object you create.
- lifetime of static variable is throughout execution of program.
> Static method
- static method can access only static variables or static methods.
-static methods are faster than instance methods.
[reason: instance methods and variables use pointer internally while static does not. it accesses data directly that's why its faster ]
```
public class staticexample{
	int i=10;
	static int is=20;
	public void disp(){
	     s.o.p("instance variable : " + i);   //display value of i = 10
	     s.o.p("static variable : " + is);     //instance method can access static, displays var is=20
	}
	public static void staticsisplay(){
	     s.o.p("instance variable : " + i);   //ERROR static method access only static var & method
	     s.o.p("static variable : " + is);     //static method can access static, displays var is=20
	}
}
```
> Static block vs Regular block
- If we want to write some code which will execute only once when class is loaded we add such code in static block like db connecrion
```
public class Demo{
	static { // This block when run application no matter whether we create instance or no
		sop("static block");
	}
	{	// This block execute only when we create instance of this class
		sop("Non static block");
	}
}
public class MainClass{
	public static void main(String args[]){
		Demo d = new Demo(); // non-static block executes
		Demo d2 = new Demo();
	}
}
Output:
static block
Non static block
Non static block
```

## Abstract Class vs Interface
| Abstract Class | Interface |
| --- | --- |
| Contains Abstract and non-abstract methods | contains only abstract methods |
| Contains final, non-final, static, non static variables | Contains final & static variables |
| As its class, it doesn’t support multiple inheritance | support multiple inheritance | 
| support all access modifiers | by Default its public |



