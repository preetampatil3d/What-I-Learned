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
- Sometimes, you want to have variables that are **common to all objects**. This is accomplished with the static modifier. Fields that have the static modifier in their declaration are called static fields or class variables.
- static variables are **stored in data segment**. you may create any number of instance the same variable from same location is used.
that is only one copy of static variable will be created at the time of class loading no matter how many object you create.
- lifetime of static variable is throughout execution of program.
> Static method
- **Static Method cannot override**
- static method can access only static variables or static methods.
- static methods are faster than instance methods.
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
> Static class: All static classes are nested classes.
- Outer class can not be static.
- Access only static member of Outer class.
- Why we need Static nested class
   - Create Singlton Class
   - Helper Class: Classes that assist in various tasks like logging, validation, or configuration. These helper classes often don't need to maintain state.
   - Utility Classes: utility classes typically contain methods that perform common tasks and don't require any instance-specific data
- Cons: Overusing static classes can lead to tightly coupled code, which can be harder to maintain and test
  
- Terms:  
   - Nested Class: Static or non-static class defined inside Class.
   - Inner class: Non-static class defined inside class.
   - Outer class: Class which has nested class.

## Final
- Final Variable: Used to create constants
  - Variables created with final **cannot be reassigned**.
  - But If Object declared with final, Its attribute can be modified
  ```
  final int a = 5;
  a = 6; // Compilation issue, Cannot reaasign final, Either initial a with null or remove final

  // Below example will work as we are modifying its attributes
  final StringBuilder str = new StringBuilder("a");
  final Employee emp = new Employee("name");
  str.append("b");
  emp.setName("new Name");
  ```
- final method: **Restrict method overriding**, Gives compilation issue if we try to override.
- final class: **Restrict class for extending**. Gives compilation issue if child class try to extend final class.

## Abstract: Used with Class & Method
- abstract keyword is used to achieve abstraction, which is the process of hiding certain details and showing only the essential information.
- To create an Abstract method, We need to have an abstract class. However abstract classes may have a non-abstract method as well.
- Abstract Class: Cannot be instatiated directly, They must extend by another class
- Abstract Method: are methods that do not have a body. They must be implemented by the concrete class that extends the abstract class.
- Some Rules:
- Don’t:
   - We can not create abstract methods in non-abstract classes.
   - We can not declare Abstract with final, private & static.
   - Can’t be Synchronized.
   - Abstract class, can’t be instantiated.
- Do’s
   - Applicable to only method & class
   - If the Class extends the Abstract class, It must implement at least one abstract method.
   - can contain overloaded methods , constructor, inner abstra

## Interface
- Used to Achieve Abstraction , Multiple inheritance & loose coupling.
- Variable: By default, they are ‘public static final’. Can not changed!
	- Variables can not be declared non-static & non non-final. Because Interface are contracts that implemented class must follow. Non-static / non-final variables might violate that contract. Like by modifying the value from Interface.
- Abstract Method: By default, they are ‘public abstract’
- Java 8 - static & default methods are introduced, By default they are public.
- Java 9 - private & static private method are introduced.
  ```
  interface InterfaceName{
	  private void method1(){
		// Code
	  }
	  private static void method2(){
		// Code
	  }
  }
  ```
```
Interface InterfaceName{
	// declare Constants:
	int MIN = 2; // 'public static final' (can not be changed)
	
	// declare abstract methods 
	void print(); // by default it is 'public abstract'
	
	// default methods, By default its public, we can explicitly declare private
	static void staticMethod(){
		//
	}
	//static methods, By default its public
	default void defaultMethod(){
		//
	}
}

public class ConcreteClass implements InterfaceName{
	@override
	public void print(){
		InterfaceName.staticMethod(); // call static method
		InterfaceName.super().defaultMethod(); // call default method
	}
	@override
	public void defaultMethod(){ // We can override default method
	}
	@Override // Compilation issue , It will ask to remove @override
	public void staticMethod(){ // 
		// we can have this method without @override, This is not overriden method, This is separate method.
		// If we try to do same in clild class which extends Parent Class, It will not allow even by removing @override
	}

}

// from outside
InterfaceName.staticMethod(); // call static method
ConcreteClass obj = new ConcreteClass(); 
obj.defaultMethod();
```

## Abstract Class vs Interface
| Abstract Class | Interface |
| --- | --- |
| Used for Abstraction | Used for Abstraction | 
| Can be instantiated | can not instantiated , as it dont have constructor |
| Contains Abstract and non-abstract methods | contains only abstract methods |
| Contains final, non-final, static, non static variables | Contains final & static variables |
| As its class, it doesn’t support multiple inheritance | support multiple inheritance | 
| support all access modifiers | by Default its public |

## Types of relationship in OOPS
1. IS-A (Inheritance): Relationship that is established between two classes using inheritance.
2. Association (HAS-A): This is the relation between two separate classes. One-to-One, One-to-Many, Many-to-One, Many-to-Many.
   Type of Associations:
	1. Aggregation: Belongs-to
	- UniDirection relation: One-way relation.
	- Department -> Student, student belongs to Department , But Department  doesn't belongs to Student.
	Both Objects can Survive individually
	2. Composition: Part-Of
	- Restricted form of Aggregation, Where both entries are highly dependent.
	- The composed object can not exist without another Object.
	- Company -> Department, A Company can not exist without a department and a department can not exist without a Company
	Car -> Engine

| Aggregation | Composition |
| --- | --- |
| Belongs-to | Part-Of |
| Object can Survive Individually | Highly Dependent, Can not exist without another |
| employee belongs to company | Department is Part-Of Company, Engine is Part-of Car | 


