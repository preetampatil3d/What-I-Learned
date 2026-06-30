# Behavioral Design Patterns 

> **Goal:** Behavioral patterns deal with **communication and responsibility between objects**.
> They define how objects interact and distribute responsibilities.

---

## 📋 Quick Overview Table

| Pattern | One-Line Purpose | Real-World Analogy |
|---|---|---|
| Strategy | Swap algorithms at runtime | GPS choosing route (fastest/shortest) |
| Observer | Notify multiple objects on change | YouTube subscriptions |
| Command | Encapsulate request as object | Remote control buttons |
| Chain of Responsibility | Pass request along a chain | IT helpdesk escalation |
| Template Method | Define skeleton, let subclasses fill in | Recipe with fixed steps |
| Iterator | Traverse collection without exposing internals | TV channel browsing |
| Mediator | Central hub for object communication | Air traffic controller |
| State | Change behavior when state changes | Traffic light |
| Memento | Save & restore object state | Ctrl+Z (undo) |
| Visitor | Add operations without changing classes | Tax calculator on items |

---

## 1. 🎯 Strategy Pattern

### What is it?
Defines a **family of algorithms**, encapsulates each one, and makes them interchangeable.
The client can **switch strategies at runtime** without changing its code.

### Key Principle
> *"Program to an interface, not an implementation"*

### Java Example — Sorting Strategy

```java
// Strategy Interface
interface SortStrategy {
    void sort(int[] data);
}

// Concrete Strategies
class BubbleSort implements SortStrategy {
    public void sort(int[] data) {
        System.out.println("Sorting using Bubble Sort");
    }
}

class QuickSort implements SortStrategy {
    public void sort(int[] data) {
        System.out.println("Sorting using Quick Sort");
    }
}

// Context
class Sorter {
    private SortStrategy strategy;

    public Sorter(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;  // swap at runtime!
    }

    public void sort(int[] data) {
        strategy.sort(data);
    }
}

// Usage
Sorter sorter = new Sorter(new BubbleSort());
sorter.sort(data);                       // Bubble Sort
sorter.setStrategy(new QuickSort());
sorter.sort(data);                       // Quick Sort
```

### Where it's used in Java/Spring
- `java.util.Comparator` — different comparison strategies
- Spring Security — `AuthenticationStrategy`
- `Collections.sort()` with custom comparators

### Interview Questions
- **Q: How is Strategy different from State pattern?**
  A: Strategy changes the *algorithm* based on client choice. State changes *behavior* based on internal state of the object.
- **Q: Can Strategy pattern replace inheritance?**
  A: Yes! Instead of subclassing for every variation, you compose with a strategy object — favoring composition over inheritance.

---

## 2. 👀 Observer Pattern

### What is it?
Defines a **one-to-many dependency** between objects. When one object (Subject) changes state, all its dependents (Observers) are **notified automatically**.

### Java Example — Stock Price Alert

```java
// Observer Interface
interface Observer {
    void update(String stockName, double price);
}

// Subject Interface
interface Subject {
    void addObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}

// Concrete Subject
class StockMarket implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String stockName;
    private double price;

    public void setPrice(String stockName, double price) {
        this.stockName = stockName;
        this.price = price;
        notifyObservers();  // auto-notify
    }

    public void addObserver(Observer o) { observers.add(o); }
    public void removeObserver(Observer o) { observers.remove(o); }

    public void notifyObservers() {
        for (Observer o : observers) {
            o.update(stockName, price);
        }
    }
}

// Concrete Observer
class StockAlert implements Observer {
    private String name;

    public StockAlert(String name) { this.name = name; }

    public void update(String stockName, double price) {
        System.out.println(name + " notified: " + stockName + " = ₹" + price);
    }
}

// Usage
StockMarket market = new StockMarket();
market.addObserver(new StockAlert("Investor A"));
market.addObserver(new StockAlert("Investor B"));
market.setPrice("INFY", 1800.50);  // both get notified
```

### Where it's used in Java/Spring
- `java.util.EventListener` (Swing, AWT)
- Spring `ApplicationEventPublisher` / `@EventListener`
- `java.util.Observable` (deprecated but classic)
- RxJava / Reactive Streams

### Interview Questions
- **Q: What's the difference between Observer and Pub-Sub?**
  A: Observer is synchronous and the subject knows its observers. Pub-Sub uses a message broker in between — subject and subscribers are decoupled.
- **Q: What is the risk of memory leaks in Observer pattern?**
  A: If observers are not removed properly, the subject holds references preventing garbage collection.

---

## 3. 📦 Command Pattern

### What is it?
Encapsulates a **request as an object**, allowing you to parameterize clients with different requests, support **undo/redo**, and queue operations.

### Java Example — Text Editor

```java
// Command Interface
interface Command {
    void execute();
    void undo();
}

// Receiver
class TextEditor {
    private StringBuilder text = new StringBuilder();

    public void write(String word) {
        text.append(word);
        System.out.println("Text: " + text);
    }

    public void erase(int length) {
        text.delete(text.length() - length, text.length());
        System.out.println("Text: " + text);
    }
}

// Concrete Command
class WriteCommand implements Command {
    private TextEditor editor;
    private String word;

    public WriteCommand(TextEditor editor, String word) {
        this.editor = editor;
        this.word = word;
    }

    public void execute() { editor.write(word); }
    public void undo()    { editor.erase(word.length()); }
}

// Invoker — holds history for undo
class CommandManager {
    private Deque<Command> history = new ArrayDeque<>();

    public void execute(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }

    public void undo() {
        if (!history.isEmpty()) history.pop().undo();
    }
}

// Usage
TextEditor editor = new TextEditor();
CommandManager manager = new CommandManager();

manager.execute(new WriteCommand(editor, "Hello "));
manager.execute(new WriteCommand(editor, "World"));
manager.undo();  // removes "World"
```

### Where it's used in Java/Spring
- `java.lang.Runnable` — a command without undo
- Spring Batch — `ItemProcessor` as commands
- Java's `javax.swing.Action`

### Interview Questions
- **Q: How does Command pattern support undo/redo?**
  A: Each command stores enough state to reverse itself. A stack of executed commands allows undo, and a redo stack restores them.
- **Q: How is Command different from Strategy?**
  A: Both encapsulate behavior, but Command is about *what to do (request)* + supports undo/queue. Strategy is about *how to do* something (algorithm selection).

---

## 4. 🔗 Chain of Responsibility Pattern

### What is it?
Passes a request along a **chain of handlers**. Each handler decides to process or pass it to the next one.

### Java Example — Support Ticket System

```java
// Abstract Handler
abstract class SupportHandler {
    protected SupportHandler nextHandler;

    public void setNext(SupportHandler next) {
        this.nextHandler = next;
    }

    public abstract void handleRequest(String issue, int level);
}

// Concrete Handlers
class Level1Support extends SupportHandler {
    public void handleRequest(String issue, int level) {
        if (level == 1) {
            System.out.println("Level 1 Support resolved: " + issue);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issue, level);
        }
    }
}

class Level2Support extends SupportHandler {
    public void handleRequest(String issue, int level) {
        if (level == 2) {
            System.out.println("Level 2 Support resolved: " + issue);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(issue, level);
        }
    }
}

class ManagerSupport extends SupportHandler {
    public void handleRequest(String issue, int level) {
        System.out.println("Manager resolved: " + issue);
    }
}

// Usage — build the chain
SupportHandler l1 = new Level1Support();
SupportHandler l2 = new Level2Support();
SupportHandler manager = new ManagerSupport();

l1.setNext(l2);
l2.setNext(manager);

l1.handleRequest("Password Reset", 1);     // Level 1 handles
l1.handleRequest("Server Down", 3);        // escalates to Manager
```

### Where it's used in Java/Spring
- **Spring Security Filter Chain** — the most famous example!
- Servlet `FilterChain` in Java EE
- Logging frameworks (`Logger.getParent()`)

### Interview Questions
- **Q: What's the real-world use in Spring you know?**
  A: Spring Security uses a chain of filters — each filter checks authentication, authorization, CSRF, etc. If one fails, it can reject the request or pass it along.
- **Q: What if no handler processes the request?**
  A: The request is dropped. Best practice is to add a default handler at the end to catch unhandled cases.

---

## 5. 📐 Template Method Pattern

### What is it?
Defines the **skeleton of an algorithm** in a base class and lets subclasses override specific steps **without changing the overall structure**.

### Java Example — Data Report Generator

```java
// Abstract class with template method
abstract class ReportGenerator {

    // Template method — fixed order of steps
    public final void generateReport() {
        fetchData();
        processData();
        formatReport();
        exportReport();
    }

    abstract void fetchData();
    abstract void processData();

    void formatReport() {
        System.out.println("Default formatting applied");
    }

    abstract void exportReport();
}

// Concrete implementation — PDF Report
class PdfReport extends ReportGenerator {
    void fetchData()    { System.out.println("Fetching data from DB"); }
    void processData()  { System.out.println("Processing sales data"); }
    void exportReport() { System.out.println("Exporting as PDF"); }
}

// Concrete implementation — Excel Report
class ExcelReport extends ReportGenerator {
    void fetchData()    { System.out.println("Fetching data from API"); }
    void processData()  { System.out.println("Processing HR data"); }
    void exportReport() { System.out.println("Exporting as Excel"); }
}

// Usage
ReportGenerator report = new PdfReport();
report.generateReport();  // steps in fixed order, PDF style
```

### Where it's used in Java/Spring
- `java.io.InputStream` — `read()` is overridden, structure is fixed
- Spring's `JdbcTemplate`, `RestTemplate`, `HibernateTemplate`
- `AbstractList`, `AbstractMap` in Java Collections

### Interview Questions
- **Q: How is Template Method different from Strategy?**
  A: Template Method uses **inheritance** — subclasses override steps. Strategy uses **composition** — you inject a different algorithm object. Strategy is more flexible.
- **Q: What does `final` on the template method mean?**
  A: It prevents subclasses from changing the overall algorithm structure — they can only customize specific steps.

---

## 6. 🔁 Iterator Pattern

### What is it?
Provides a way to **sequentially access elements** of a collection without exposing its internal structure.

### Java Example — Custom Playlist Iterator

```java
// Iterator Interface
interface SongIterator {
    boolean hasNext();
    String next();
}

// Concrete Iterator
class PlaylistIterator implements SongIterator {
    private String[] songs;
    private int index = 0;

    public PlaylistIterator(String[] songs) {
        this.songs = songs;
    }

    public boolean hasNext() { return index < songs.length; }
    public String next()     { return songs[index++]; }
}

// Collection (Aggregate)
class Playlist {
    private String[] songs = {"Song A", "Song B", "Song C"};

    public SongIterator iterator() {
        return new PlaylistIterator(songs);
    }
}

// Usage
Playlist playlist = new Playlist();
SongIterator it = playlist.iterator();

while (it.hasNext()) {
    System.out.println("Playing: " + it.next());
}
```

### Where it's used in Java/Spring
- `java.util.Iterator` — the backbone of the entire Java Collections Framework
- Enhanced `for-each` loop uses `Iterable` interface internally
- `ResultSet` in JDBC is an iterator over database rows

### Interview Questions
- **Q: What's the difference between Iterator and ListIterator?**
  A: `Iterator` is forward-only. `ListIterator` supports forward, backward, and element modification.
- **Q: What happens if you modify a collection while iterating?**
  A: `ConcurrentModificationException` is thrown. Use `Iterator.remove()` or `CopyOnWriteArrayList` to safely remove elements.

---

## 7. 🚦 State Pattern

### What is it?
Allows an object to **alter its behavior when its internal state changes**. The object appears to change its class.

### Java Example — ATM Machine

```java
// State Interface
interface ATMState {
    void insertCard();
    void enterPin();
    void withdrawCash();
}

// Context
class ATMMachine {
    private ATMState currentState;

    public ATMMachine() {
        currentState = new NoCardState(this);
    }

    public void setState(ATMState state) { this.currentState = state; }
    public void insertCard()   { currentState.insertCard(); }
    public void enterPin()     { currentState.enterPin(); }
    public void withdrawCash() { currentState.withdrawCash(); }
}

// Concrete State — No Card
class NoCardState implements ATMState {
    private ATMMachine atm;
    public NoCardState(ATMMachine atm) { this.atm = atm; }

    public void insertCard() {
        System.out.println("Card inserted!");
        atm.setState(new HasCardState(atm));
    }
    public void enterPin()     { System.out.println("Insert card first!"); }
    public void withdrawCash() { System.out.println("Insert card first!"); }
}

// Concrete State — Has Card
class HasCardState implements ATMState {
    private ATMMachine atm;
    public HasCardState(ATMMachine atm) { this.atm = atm; }

    public void insertCard()   { System.out.println("Card already inserted!"); }
    public void enterPin() {
        System.out.println("PIN entered. Access granted!");
        atm.setState(new AuthenticatedState(atm));
    }
    public void withdrawCash() { System.out.println("Enter PIN first!"); }
}

class AuthenticatedState implements ATMState {
    private ATMMachine atm;
    public AuthenticatedState(ATMMachine atm) { this.atm = atm; }

    public void insertCard()   { System.out.println("Already authenticated!"); }
    public void enterPin()     { System.out.println("Already authenticated!"); }
    public void withdrawCash() {
        System.out.println("Cash dispensed!");
        atm.setState(new NoCardState(atm));
    }
}

// Usage
ATMMachine atm = new ATMMachine();
atm.withdrawCash();   // Insert card first!
atm.insertCard();     // Card inserted!
atm.enterPin();       // PIN entered. Access granted!
atm.withdrawCash();   // Cash dispensed!
```

### Where it's used in Java/Spring
- Spring State Machine (Spring Statemachine project)
- Order processing systems (Pending → Confirmed → Shipped → Delivered)
- TCP connection states

### Interview Questions
- **Q: How is State different from Strategy?**
  A: In State, the object **itself decides** when to transition to another state. In Strategy, the **client decides** which strategy to use.
- **Q: What problem does State solve vs using if-else?**
  A: Removes complex if-else/switch chains. Each state is a separate class — easier to add new states without modifying existing ones (Open/Closed Principle).

---

## 8. 💾 Memento Pattern

### What is it?
Captures and **saves an object's internal state** so it can be **restored later** — without violating encapsulation.

### Java Example — Text Editor Undo

```java
// Memento — holds the saved state
class EditorMemento {
    private final String content;

    public EditorMemento(String content) {
        this.content = content;
    }

    public String getContent() { return content; }
}

// Originator — creates and uses mementos
class Editor {
    private String content = "";

    public void write(String text) { content += text; }
    public String getContent()     { return content; }

    public EditorMemento save() {
        return new EditorMemento(content);  // save snapshot
    }

    public void restore(EditorMemento memento) {
        content = memento.getContent();     // restore snapshot
    }
}

// Caretaker — manages undo history
class History {
    private Deque<EditorMemento> history = new ArrayDeque<>();

    public void save(EditorMemento m) { history.push(m); }
    public EditorMemento undo()       { return history.isEmpty() ? null : history.pop(); }
}

// Usage
Editor editor = new Editor();
History history = new History();

editor.write("Hello ");
history.save(editor.save());

editor.write("World");
history.save(editor.save());

editor.write("!!!");
System.out.println(editor.getContent());  // Hello World!!!

editor.restore(history.undo());
System.out.println(editor.getContent());  // Hello World

editor.restore(history.undo());
System.out.println(editor.getContent());  // Hello
```

### Where it's used in Java/Spring
- Serialization in Java (saving object state)
- IDE undo history
- Game save/load systems

### Interview Questions
- **Q: How does Memento preserve encapsulation?**
  A: The Memento object is opaque to the Caretaker — it only stores state. Only the Originator knows what the state means and how to use it.
- **Q: What's the downside of Memento?**
  A: High memory usage if you save many states. Useful to limit history depth.

---

## 9. 🤝 Mediator Pattern

### What is it?
Defines an object that **centralizes communication** between multiple objects, reducing direct dependencies between them.

### Java Example — Chat Room

```java
// Mediator Interface
interface ChatMediator {
    void sendMessage(String message, User sender);
    void addUser(User user);
}

// Concrete Mediator
class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();

    public void addUser(User user) { users.add(user); }

    public void sendMessage(String message, User sender) {
        for (User user : users) {
            if (user != sender) {
                user.receive(sender.getName() + ": " + message);
            }
        }
    }
}

// Colleague
class User {
    private String name;
    private ChatMediator mediator;

    public User(String name, ChatMediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }

    public String getName() { return name; }

    public void send(String message) {
        System.out.println(name + " sends: " + message);
        mediator.sendMessage(message, this);
    }

    public void receive(String message) {
        System.out.println(name + " receives: " + message);
    }
}

// Usage
ChatRoom room = new ChatRoom();
User alice = new User("Alice", room);
User bob   = new User("Bob", room);
User carol = new User("Carol", room);

room.addUser(alice);
room.addUser(bob);
room.addUser(carol);

alice.send("Hello everyone!");  // Bob and Carol receive it
```

### Where it's used in Java/Spring
- Spring's `ApplicationEventPublisher` — decoupled event communication
- Java's `java.util.concurrent.Executor`
- MVC pattern — Controller acts as a mediator

### Interview Questions
- **Q: When would you use Mediator vs Observer?**
  A: Use Mediator when objects need **two-way** complex communication. Use Observer for **one-way** notifications (subject → observers).
- **Q: What's the downside of Mediator?**
  A: The Mediator itself can become a "God Object" — a complex monolith if not managed carefully.

---

## 10. 🚶 Visitor Pattern

### What is it?
Lets you **add new operations to existing object structures** without modifying those classes. Separates algorithm from object structure.

### Java Example — Shopping Cart Tax Calculator

```java
// Visitor Interface
interface TaxVisitor {
    double visit(Electronics item);
    double visit(Food item);
}

// Element Interface
interface Item {
    double accept(TaxVisitor visitor);
}

// Concrete Elements
class Electronics implements Item {
    private double price;
    public Electronics(double price) { this.price = price; }
    public double getPrice()         { return price; }
    public double accept(TaxVisitor visitor) { return visitor.visit(this); }
}

class Food implements Item {
    private double price;
    public Food(double price) { this.price = price; }
    public double getPrice()  { return price; }
    public double accept(TaxVisitor visitor) { return visitor.visit(this); }
}

// Concrete Visitor — Tax Calculator
class TaxCalculator implements TaxVisitor {
    public double visit(Electronics item) {
        return item.getPrice() * 0.18;  // 18% GST
    }
    public double visit(Food item) {
        return item.getPrice() * 0.05;  // 5% GST
    }
}

// Usage
List<Item> cart = List.of(new Electronics(10000), new Food(500));
TaxVisitor taxCalc = new TaxCalculator();

double totalTax = 0;
for (Item item : cart) {
    totalTax += item.accept(taxCalc);
}
System.out.println("Total Tax: ₹" + totalTax);  // ₹1825.0
```

### Where it's used in Java/Spring
- Java compiler's AST traversal
- Spring's `BeanDefinitionVisitor`
- XML/JSON parsers visiting nodes

### Interview Questions
- **Q: What is double dispatch in Visitor?**
  A: Two method calls determine the final behavior — first `accept()` on the element (dispatch 1), then `visit()` on the visitor (dispatch 2). This selects the right method for each type pair.
- **Q: When would you NOT use Visitor?**
  A: When your element hierarchy changes frequently — every new element type requires updating all visitors. It's best when elements are stable but operations change often.

---

## 🔥 Top Interview Questions — Quick Reference

### Common Cross-Pattern Questions

| Question | Answer Hint |
|---|---|
| Strategy vs State | Strategy = client chooses algorithm; State = object auto-transitions |
| Observer vs Mediator | Observer = broadcast; Mediator = two-way centralized |
| Command vs Strategy | Command = what + undo; Strategy = how |
| Template Method vs Strategy | Template = inheritance; Strategy = composition |
| Iterator vs for-each | for-each uses Iterator internally via `Iterable` |

### Must-Know Real-World Mappings

```
Strategy       →  java.util.Comparator, Collections.sort()
Observer       →  EventListener, Spring @EventListener
Command        →  java.lang.Runnable, Spring Batch
Chain of Resp  →  Spring Security Filter Chain, Servlet FilterChain
Template       →  JdbcTemplate, RestTemplate, AbstractList
Iterator       →  java.util.Iterator, ResultSet
State          →  Spring State Machine, Order workflows
Mediator       →  Spring ApplicationContext event bus
```

---

## 📌 How to Answer in Interviews (STAR Format)

When asked *"Explain [Pattern] with an example"*:

1. **Define** — "This pattern is about..."
2. **Problem it solves** — "Without it, we'd have..."
3. **Show the structure** — "There's a Subject, Observer, and..."
4. **Give a real example** — "In my project / in Spring Security..."
5. **Mention trade-offs** — "The downside is..."

---
