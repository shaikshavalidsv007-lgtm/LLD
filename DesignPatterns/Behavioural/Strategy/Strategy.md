1) Definition & Core Intent

Strategy Pattern is a behavioral design pattern that lets you define a family of algorithms, encapsulate each one, and make them interchangeable at runtime.

👉 Core idea:

“Encapsulate what varies and make it interchangeable.”

Instead of hardcoding logic using if-else or switch, you delegate behavior to separate classes (strategies).

2) When & Why to Use It

✅ Use when:

You have multiple ways to perform a task
You see large if-else / switch logic
Behavior needs to change at runtime
You want to follow Open/Closed Principle (OCP)

❌ Problem it solves:

if (paymentType.equals("CARD")) { ... }
else if (paymentType.equals("UPI")) { ... }
else if (paymentType.equals("PAYPAL")) { ... }

🚫 Issues:

Hard to extend
Violates OCP
Becomes messy quickly

✅ Strategy fixes this by:

Moving each logic into separate classes
Allowing runtime swapping

3) Structure & Key Participants

🔹 Components
Strategy (Interface)
Defines common behavior
ConcreteStrategy
Different implementations
Context
Uses a strategy (doesn’t care which one)

📌 UML Mental Model
       Strategy (interface)
             ↑
   ------------------------
   |          |           |
Concrete1  Concrete2  Concrete3

         Context
           |
      uses Strategy
	  
4) Pros & Cons

✅ Pros
Eliminates if-else chains
Runtime flexibility
Easy to extend (OCP)
Promotes composition over inheritance
❌ Cons
Increases number of classes
Client must know which strategy to use
Slight overhead of abstraction

5) Real-World Examples

💡 1. Payment Strategy

Java

interface PaymentStrategy {
    void pay(int amount);
}

class CardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid using Card: " + amount);
    }
}

class UpiPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid using UPI: " + amount);
    }
}

class PaymentContext {
    private PaymentStrategy strategy;

    public void setStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void pay(int amount) {
        strategy.pay(amount);
    }
}


💡 2. Sorting Strategy

Different sorting algorithms:
QuickSort
MergeSort
BubbleSort

💡 3. Comparator Strategy (Real Java Example)

Collections.sort(list, Comparator.comparing(Employee::getSalary));

👉 Comparator acts as a strategy

6) Common Pitfalls

❌ Too many small classes

Avoid over-engineering

❌ Client tightly coupled to strategies

Use Factory if needed

❌ Unnecessary abstraction

Don’t use Strategy if only 1 behavior exists

7) Best Practices & Design Considerations

🔹 1. Use Dependency Injection
public PaymentContext(PaymentStrategy strategy) {
    this.strategy = strategy;
}
🔹 2. Prefer Immutability
Make strategies stateless where possible
🔹 3. Use Lambda (Java 8+)
PaymentStrategy strategy = amount -> System.out.println(amount);
🔹 4. Testing
Test each strategy independently
Mock strategies in unit tests
🔹 5. Naming

Use clear names:

CompressionStrategy

DiscountStrategy

8) Comparisons with Other Patterns

🔁 Strategy vs State
Strategy	State
Behavior chosen by client	Behavior changes automatically
Independent algorithms	State-driven transitions
🔁 Strategy vs Template Method
Strategy	Template Method
Uses composition	Uses inheritance
Runtime flexibility	Compile-time structure
🔁 Strategy vs Function Objects (Lambdas)
Lambdas = lightweight strategy
Strategy classes = better for complex logic

9) Quick-Start Blueprint

✅ Minimal Example

interface Strategy {
    void execute();
}

class A implements Strategy {
    public void execute() {
        System.out.println("A logic");
    }
}

class B implements Strategy {
    public void execute() {
        System.out.println("B logic");
    }
}

class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public void run() {
        strategy.execute();
    }
}

🔄 Runtime Switching

Context ctx = new Context(new A());
ctx.run(); // A logic

ctx = new Context(new B());
ctx.run(); // B logic

🚀 Extension Example

Add new strategy:

class C implements Strategy {
    public void execute() {
        System.out.println("C logic");
    }
}

👉 No change to existing code → OCP satisfied

10) References & Further Reading

GoF Design Patterns Book (Gang of Four)
Head First Design Patterns
Effective Java (for Strategy via interfaces & lambdas)
Java Collections Framework (Comparator, Comparable)

🧠 Interview Summary (Quick Recall)

Type: Behavioral Pattern
Purpose: Replace if-else with interchangeable algorithms
Key Idea: Encapsulation + Composition
Real Use: Payment systems, sorting, validation, pricing logic
Modern Use: Lambdas (Java 8+)