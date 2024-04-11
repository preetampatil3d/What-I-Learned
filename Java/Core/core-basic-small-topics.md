
- Can we override the private & static methods?
  - No

- Private methods are not visible so they will not be overridden.
  - If we create a static method in a derived class, It will not be treated as an overridden method. Instead, it will method based on Type (Left-hand side)

- Does, finally execute always?
  - Not in the case of, System.exit() , system crash

- Object Class Methods
  - equals, hashCode, getClass, clone, finalize (called by GC when no reference is found), toString, notify, notifyAll, wait

- Fail-fast & fail-safe
  - Fail-fast: throw exception
    - Throw concurrentModificationException when one thread iterates collection and another tries to modify
  - Fail-safe: works without exception
    - It will not throw an exception for the above case as it operates on a clone object.

- Call By Value vs Call By Reference
  - Call By Value: Pass Copy of Variable.
  - Call By Reference: Passing Memory Address.

  - In java always its pass by value
  - When Passing an Object, Java Passes reference as a Value. But it feels like we are passing reference

- Equals() & HashCode()
  - Shallow compare: Compare memory address
  - Deep Compare: compare values

- to compare two objects, you need to override equals & hashcode. By making use of both we need to compare
  - equals (add deep compare) 
  - hashcode method, two objects must have the same hashcode if they are identical.
  - But as we are comparing two objects we must implement hashCode as well.

- Can the visibility of the overridden method be lower than the Parent method? 
  - No.

- Shadowing of static Method
  - Always methods are executed from the class which is created with a new keyword. Memory is allocated for the same class.
  - But if we are assigning to an Object where the method is static, then it always points to a static method and ignores the overridden method.


- Association: ?
1. Aggregation: Weak: HAS-A: class HAS-A property

2. Composition: Strong: IS-A: child class IS-A parent class


- Covariant return Type:
  - Changing the return type of the overridden method. 
  - But the return type of overridden method in the child class must be a child of the return type of method from the parent class 
```
Public class ParentClass{

	public Object printMethod(){
		return null;
	}
}
Public class ChildClass extends Parent Class{ // String is child of Object
	public String printMethod(){
		return “String Type”;
	}

}

````

- Type Promotion
  - A smaller data type is promoted to a bigger one if no matching type is found
```
// Case1
add(int a, Long b) // <- add(4,5) here 5 is promoted to Long data type

// Case 2
// add(4,5) // ambiguity compile type issue, confuses which one to call
add(int a, Long b)
add(Long a, int b)
```
