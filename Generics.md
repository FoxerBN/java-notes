## 📘 Java Generics – Full Notes

---

### 🧠 What Are Generics?

Generics allow you to write **type-safe** and **reusable** code.  
They let you work with **any object type**, while still maintaining **compile-time type checking**.

---

### ✅ Benefits

- **Type safety** – prevents `ClassCastException`
    
- **Code reusability** – use the same logic for multiple data types
    
- **Cleaner code** – no need for manual casting
    
- **Improved readability** – types are clear at a glance
    

---

### 🔰 Basic Syntax

```java
class Box<T> {
    private T value;
    public void set(T value) { this.value = value; }
    public T get() { return value; }
}
```

Usage:

```java
Box<String> stringBox = new Box<>();
stringBox.set("Hello");
String s = stringBox.get();
```

---

### 🧩 Generic Methods

```java
public class Util {
    public static <T> void printArray(T[] array) {
        for (T elem : array) {
            System.out.println(elem);
        }
    }
}
```

Usage:

```java
Integer[] nums = {1, 2, 3};
Util.printArray(nums);
```

---

### 🪄 Generic Interfaces

```java
interface Pair<K, V> {
    K getKey();
    V getValue();
}

class OrderedPair<K, V> implements Pair<K, V> {
    private K key;
    private V value;

    public OrderedPair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() { return key; }
    public V getValue() { return value; }
}
```

Usage:

```java
Pair<String, Integer> p = new OrderedPair<>("Age", 30);
```

---

### ⚙️ Bounded Type Parameters

```java
// T must be a subclass of Number
public class Stats<T extends Number> {
    T[] nums;

    public Stats(T[] nums) {
        this.nums = nums;
    }

    public double average() {
        double sum = 0.0;
        for (T num : nums)
            sum += num.doubleValue();
        return sum / nums.length;
    }
}
```

---

### 🧪 Wildcards in Generics

```java
// Unbounded wildcard
public void printList(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}
```

```java
// Upper bounded wildcard: List of Number or subclass
public double sum(List<? extends Number> list) {
    double total = 0;
    for (Number num : list) {
        total += num.doubleValue();
    }
    return total;
}
```

```java
// Lower bounded wildcard: List of Integer or superclass
public void addIntegers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}
```

---

### ⚠️ Limitations of Generics

- **Cannot create** generic arrays: `T[] arr = new T[10]; ❌`
    
- **Cannot use primitives** directly: use wrapper classes (`int` → `Integer`)
    
- **Cannot use instanceof** with generic types: `if (obj instanceof T) ❌`
    

---

### 🧱 Type Erasure

At runtime, Java **removes** all generic type information – this is called **type erasure**.

```java
Box<Integer> intBox = new Box<>();
Box<String> strBox = new Box<>();
System.out.println(intBox.getClass() == strBox.getClass()); // true ✅
```

---

### 📦 Generic Collections (from java.util)

```java
List<String> names = new ArrayList<>();
Map<Integer, String> idToName = new HashMap<>();
Set<Double> scores = new HashSet<>();
```

These use generics under the hood!

---

### 🪸 Real Example – Generic Repository

```java
interface Repository<T> {
    void save(T entity);
    T findById(int id);
}

class User {}
class UserRepository implements Repository<User> {
    public void save(User entity) { /*...*/ }
    public User findById(int id) { return new User(); }
}
```

---

### 📌 Tips

- Use **`<T extends Something>`** for constraints
    
- Prefer **wildcards** (`? extends`, `? super`) when passing data
    
- Use **generics** for custom data structures (linked list, stack, etc.)
    
- Don't overuse generics – can hurt readability