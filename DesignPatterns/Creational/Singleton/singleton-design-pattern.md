# Singleton Design Pattern

## 📌 Definition
Singleton is a **Creational Design Pattern** that ensures:
- Only **one instance** of a class exists
- Provides a **global access point**

---

## 🎯 Real-Time Analogy (Quick Recall)
- One **CEO** of a company
- One **Printer Spooler**
- One **Database Connection Pool**
- One **Logger instance**

👉 Whenever you see "ONLY ONE shared resource" → Think Singleton

---

## 🏗️ Key Characteristics
- Private constructor
- Static instance
- Public static access method

---

## 💻 Best Implementation (Recommended)

### Bill Pugh Singleton
```java
class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}



✔ Lazy
✔ Thread-safe
✔ No synchronization overhead

🔥 Breaking Singleton (VERY IMPORTANT FOR INTERVIEW)
❌ 1. Breaking using Reflection API
import java.lang.reflect.Constructor;

public class ReflectionBreak {
    public static void main(String[] args) throws Exception {
        Singleton instance1 = Singleton.getInstance();

        Constructor<Singleton> constructor =
            Singleton.class.getDeclaredConstructor();
        constructor.setAccessible(true);

        Singleton instance2 = constructor.newInstance();

        System.out.println(instance1 == instance2); // false ❌
    }
}

👉 Why it breaks?

Reflection bypasses private constructor
✅ Fix:
Use Enum Singleton
❌ 2. Breaking using Serialization
import java.io.*;

class SerializationBreak {
    public static void main(String[] args) throws Exception {
        Singleton instance1 = Singleton.getInstance();

        ObjectOutputStream out = new ObjectOutputStream(
            new FileOutputStream("file.ser"));
        out.writeObject(instance1);
        out.close();

        ObjectInputStream in = new ObjectInputStream(
            new FileInputStream("file.ser"));
        Singleton instance2 = (Singleton) in.readObject();

        System.out.println(instance1 == instance2); // false ❌
    }
}

👉 Why it breaks?

Deserialization creates a new object
✅ Fix:
protected Object readResolve() {
    return getInstance();
}
❌ 3. Breaking using Cloning
class Singleton implements Cloneable {

    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
Singleton s1 = Singleton.getInstance();
Singleton s2 = (Singleton) s1.clone();

System.out.println(s1 == s2); // false ❌

👉 Why it breaks?

Clone creates a new instance
✅ Fix:
@Override
protected Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException();
}
❌ 4. Multithreading Issue (Race Condition)
if (instance == null) {
    instance = new Singleton();
}

👉 Two threads can create two objects

✅ Fix:
Double Checked Locking
Bill Pugh
Enum
🏆 Safest Implementation (Enum Singleton)
enum Singleton {
    INSTANCE;
}

✔ Prevents Reflection
✔ Prevents Serialization issues
✔ Thread-safe by default

🌍 Real-World Examples
1. Logger
One logger instance across app
Example: log4j / slf4j

2. Database Connection
Shared connection pool
Avoids expensive object creation

3. Configuration Manager
Reads config file once
Shared everywhere

4. Cache (Redis / In-memory)
One cache manager

5. Thread Pool
Controls number of threads globally

⚖️ Advantages
Memory efficient
Controlled access
Global availability

❌ Disadvantages
Hard to unit test
Global state (bad design sometimes)
Violates Single Responsibility Principle

🚫 When NOT to Use
Microservices (prefer Dependency Injection)
When multiple instances needed
Highly testable systems

🧠 Interview Quick Summary
Singleton ensures a class has only one instance and provides a global access point. It can be broken using Reflection, Serialization, and Cloning. The safest way to implement Singleton is using Enum, while Bill Pugh is the best practical approach.

🔥 Final Takeaway

👉 Use Enum Singleton → Maximum safety
👉 Use Bill Pugh → Best practical implementation
👉 Always mention breaking cases in interviews (this gives extra points)
