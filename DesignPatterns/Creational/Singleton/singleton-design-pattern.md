# 🧠 Singleton Design Pattern

## 📌 Definition
Singleton is a **Creational Design Pattern** that ensures:
- Only **one instance** of a class exists
- Provides a **global access point** to that instance

---

## 🎯 When to Think of Singleton (Quick Recall)
Use Singleton when:
- Only **one object is needed globally**
- Shared resource across application

### 🧩 Real-Life Examples
- One **CEO** of a company
- One **Printer Manager**
- One **Logger**
- One **Database Connection Pool**
- One **Cache Manager**

👉 Keyword trigger: **“Single Shared Resource”**

---

## 🏗️ Key Characteristics
- `private constructor` → Prevents object creation
- `static instance` → Stores single object
- `public static method` → Global access

---

# 💻 Implementations

---

## 1️⃣ Eager Initialization
```java
class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}

✔ Thread-safe
❌ Memory waste (object created even if unused)

2️⃣ Lazy Initialization (Not Thread-safe)
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

✔ Memory efficient
❌ Not thread-safe

3️⃣ Synchronized Method
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

✔ Thread-safe
❌ Performance issue

4️⃣ Double Checked Locking
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

✔ Thread-safe
✔ Better performance

🏆 5️⃣ Bill Pugh Singleton (Best Practice)
class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}

✔ Lazy loading
✔ Thread-safe
✔ No synchronization overhead

🔥 6️⃣ Enum Singleton (Safest)
enum Singleton {
    INSTANCE;
}

✔ Prevents:

Reflection
Serialization issues
Thread problems

🚨 Breaking Singleton (Most Important Interview Section)

❌ 1. Reflection Attack
import java.lang.reflect.Constructor;

Singleton s1 = Singleton.getInstance();

Constructor<Singleton> cons =
    Singleton.class.getDeclaredConstructor();

cons.setAccessible(true);

Singleton s2 = cons.newInstance();

System.out.println(s1 == s2); // false ❌
🔍 Why?
Reflection bypasses private constructor
✅ Fix

👉 Use Enum Singleton

❌ 2. Serialization Attack
ObjectOutputStream out = new ObjectOutputStream(
    new FileOutputStream("file.ser"));
out.writeObject(s1);

ObjectInputStream in = new ObjectInputStream(
    new FileInputStream("file.ser"));

Singleton s2 = (Singleton) in.readObject();

System.out.println(s1 == s2); // false ❌
🔍 Why?
Deserialization creates new object
✅ Fix
protected Object readResolve() {
    return getInstance();
}

❌ 3. Cloning Attack
Singleton s2 = (Singleton) s1.clone();

System.out.println(s1 == s2); // false ❌
🔍 Why?
Clone creates new object
✅ Fix
@Override
protected Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException();
}

❌ 4. Multithreading Issue
if (instance == null) {
    instance = new Singleton();
}
🔍 Why?
Two threads create two objects
✅ Fix
Double Checked Locking
Bill Pugh
Enum


🌍 Real-World Examples (High Recall Section)

1️⃣ Logger
Single logging instance
Example: Log4j, SLF4J

2️⃣ Database Connection Pool
Expensive to create connections
Shared globally

3️⃣ Configuration Manager
Load config once
Used everywhere

4️⃣ Cache Manager
Redis / In-memory cache
One instance for consistency

5️⃣ Thread Pool
Controls thread usage globally
⚖️ Advantages
Memory efficient
Controlled access
Global availability

❌ Disadvantages
Hard to test (global state)
Tight coupling
Violates Single Responsibility Principle

🚫 When NOT to Use
Microservices (use Dependency Injection)
When multiple instances needed
Highly testable applications

🧠 Interview Answer (Perfect Version)
Singleton is a creational design pattern that ensures only one instance of a class exists and provides a global access point. It can be implemented using lazy initialization, double-checked locking, or Bill Pugh method. However, it can be broken using Reflection, Serialization, and Cloning. The safest implementation is Enum Singleton.

🔥 Final Takeaways
👉 Best Practical → Bill Pugh Singleton
👉 Safest → Enum Singleton
👉 Always mention → Breaking Scenarios in interviews

https://chatgpt.com/backend-api/estuary/content?id=file_00000000748c720b9af8116d715f090f&ts=493499&p=fs&cid=1&sig=403a542436ca9ce3da75a6a97df46f5ae5c4ca4d339f3544d65f93d8fa7ebf5e&v=0


