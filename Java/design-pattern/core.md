## Pillars
1. Abstraction :
- Helps in 
  - isolate implementation details.
  - Improve code maintainability and reusability.
- Achieved using Interfaces, Abstract classes
      
2. Encapsulation 
- Hides internal state using private variables and provides public methods to access them.
It prevents any unintended or unauthorised changes and maintain correctness of data within system by retricting direct access to variable and allowing update through methods where we can add validation logic.
- Examples: 
getBal()
setBal(double bal) if(bal>0) this.bal=bal;

3. Inheritance
- Mechanism where one class acquires properties (fields) and behaviours (methods) of another class.
- Helps in 
  - code reusability 
  - establish IS-A relationships between classes.
- Overuse can lead to tight coupling between classes.

4. Polymorphism 
- same interface, different implementations.
- Achieved through 
  - method overloading (compile-time polymorphism) 
  - method overriding (run-time polymorphism).
- Example:
  - Operations on different Trade Type is decided at runtime based on object type.

## SOLID
1. Single responsibility: A class should do only one thing, and have one reason to change.
   - can split the class based on different behaviour. And make sure every class has a single responsibility.
   - Example: 
     - TradeValidator, TradeCalculator, TradeNotifier, TradeLogger, TradePersistor
     - Bad Example: TradeProcessor (does all the above)
2. Open-close: Open for extension and close for modification.
   - Achieved by utilising the interface and strategy design pattern.
   - Instead of adding new behaviour to the existing class, create a new class (for new behaviour) and extend an existing class. 
3. Liskov Substitution (LSP)
   - Subclass should not break the behaviour of the base/parent class.
   - The derived class must always be substituted / or should be compatible with a base class or Interface.
      BaseClass obj = new DerivedClass();
```declarative
// Bad Example:
class Trade {
public void execute() {System.out.println("Executing trade"); }
}
class ReadOnlyTrade extends Trade { // Violates Liskov Substitution Principle
@Override
public void execute() {throw new UnsupportedOperationException("Read-only trade cannot be executed"); }
}
// Trade contract says: execute is allowed, but ReadOnlyTrade violates that contract by throwing an exception.

// Good Example:
interface ExecutableTrade {
void execute();
}
interface ViewOnlyTrade {
void view();
}

class EquityTrade implements ExecutableTrade {
@Override
public void execute() {System.out.println("Executing equity trade"); }
}
class AuditTrade implements ViewOnlyTrade {
@Override
public void view() {System.out.println("Viewing trade for audit"); }
}
```
4. Interface Segregation (ISP)
   - Create or Split Interfaces in such a way that all its Abstract methods should be implemented. 
   - We should not force clients to implement such methods that are not required.
```declarative
// Bad Example:
interface Trade {
void execute();
void cancel();
void view();
}
class EquityTrade implements Trade {
public void execute() {}
public void cancel() {}
public void view() {}
}
class EquityTrade implements Trade {
public void execute() { throw new UnsupportedOperationException("Execute not supported"); } // problem
public void cancel() { throw new UnsupportedOperationException("Cancel not supported"); }  // problem
public void view() { system.out.println("Viewing trade"); }
}

Good Example: In the above scenario, we can split the trade into smaller interfaces. so that we should not force unsupported operations
interface StandardTrade {
public void execute() {}
public void cancel() {}
public void view() {}
}
interface ViewableTrade {
void view();
}

```
5. Dependency Injection:
   - High-level modules should not depend on low-level modules. Both should depend on abstractions.
   - Achieved by injecting dependencies through constructors, setters, or interfaces rather than creating them within the class.
   - Example:
```declarative
// Bad Example: tight coupling with repository, hard to update DB changes, tests
class TradeService {
private TradeRepository tradeRepository = new TradeRepository();
public void processTrade(Trade trade) {tradeRepository.save(trade); }
}

// Good Example: loose coupling
interface TradeRepository {
void save(Trade trade);
}
class DbTradeRepository implements TradeRepository { // DB1
public void save(Trade trade) {System.out.println("Saving trade to DB"); }
}
class InMemoryTradeRepository implements TradeRepository { // DB2
public void save(Trade trade) {System.out.println("Saving trade in memory"); }
}
// actual
class TradeService {
    private final TradeRepository tradeRepository;
    public TradeService(TradeRepository tradeRepository) {this.tradeRepository = tradeRepository; }
    public void processTrade(Trade trade) {tradeRepository.save(trade); }
}

// Calls
TradeRepository repo = new DbTradeRepository();
TradeService service = new TradeService(repo);

```

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



