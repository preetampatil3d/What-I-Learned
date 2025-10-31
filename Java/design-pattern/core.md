## Pillars
1. Abstraction :
Identifying essential parts of objects and hiding the complexity of how they are implemented.
Achieved using Interfaces, Abstract classes
Essential details
Hiding complexity
Public interface

2. Encapsulation 
Hiding information: Decide what an object should expose to the world

3. Inheritance

4. Polymorphism 


## SOLID
1. Single responsibility: Class should do only one thing, and have one reason to change.
- can split the class based on different behaviour. And make sure every class is with a single responsibility.
2. Open-close: Open for extension and close for modification.
make use of inheritance, abstraction and encapsulation.
Instead of adding new behaviour to the existing class, create a new class (for new behaviour) and extend an existing class. 
3. Liskov Substitution
The derived class must always be substituted / or should be compatible with a base class or Interface.
BaseClass obj = new DerivedClass();
4. Interface Segregation 
Create or Split Interfaces in such a way that, All its Abstract methods should be implemented. We should not force clients to implement such methods which is not required.
5. Dependency Injection:


## Design Pattern
1. Creational
  - Singleton: *
  - Builder: *
  - Factor: *
  - Abstract Factory
  - Prototype
2. Behavioural
  - State: *
  - Strategy: *
  - Command
  - Interpreter 
  - Memento
3. Structural
  - Adapter: *
  - Decorator: *
  - Facade: *
  - Bridge
  - Composite
  - Proxy



