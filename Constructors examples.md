## 1. Copy constructor:

Používa sa na **vytvorenie novej inštancie objektu zo existujúceho objektu**. Je to bežný spôsob, ako **naklonovať (kopírovať) objekt** — teda vytvoriť nový objekt s rovnakými hodnotami, ale inou referenciou.

### Význam:

`Car copy = new Car(original);` – vytvorí **nezávislú kópiu** objektu `original`.

### V praxi:

- ochrana pred neúmyselnými zmenami pôvodného objektu,
    
- implementácia Prototype patternu,
    
- náhrada za `Cloneable`.
    

```java
class Car {
    private String brand;
    private int year;

    // Regular constructor
    public Car(String brand, int year) {
        this.brand = brand;
        this.year = year;
    }

    // Copy constructor
    public Car(Car other) {
        this.brand = other.brand;
        this.year  = other.year;
    }
}
```

---

## 2. Default constructor:

Konštruktor **bez parametrov**; kompilátor ho doplní, ak žiadny iný neexistuje. Inicializuje polia na default hodnoty.

### Význam:

```java
Book book = new Book(); // volá sa default constructor
```

### V praxi:

- potrebný pre frameworky (Hibernate, Jackson),
    
- DTO/POJO triedy vytvárané reflexiou,
    
- rýchle prototypovanie.
    

```java
class Book {
    private String title = "Untitled";
    private int pages;
}
```

---

## 3. Constructor chaining:

**Volanie jedného konštruktora z iného** cez `this(...)`, aby sa zamedzilo duplicite.

### Význam:

```java
User u = new User("Anna"); // volá sa User(String) → User(String,int)
```

### V praxi:

- centralizuje shared logiku,
    
- udržiava DRY princíp.
    

```java
class User {
    private String name;
    private int age;

    public User(String name) {
        this(name, 0); // chained
    }

    public User(String name, int age) {
        this.name = name;
        this.age  = age;
    }
}
```

---

## 4. Constructor overloading:

**Viacero konštruktorov** s rôznymi parametrami pre flexibilnú inicializáciu.

### Význam:

```java
Point p1 = new Point();
Point p2 = new Point(5);
Point p3 = new Point(5, 7);
```

### V praxi:

- API‑friendly triedy s rôznymi vstupmi,
    
- postupné rozširovanie bez breaking changes.
    

```java
class Point {
    private int x, y;

    public Point() {
        this(0, 0);
    }
    public Point(int x) {
        this(x, 0);
    }
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

---

## 5. Super constructor:

Volá **konštruktor nadradenej triedy** cez `super(...)`; musí byť prvý riadok v podtriede.

### Význam:

```java
Dog rex = new Dog("Rex"); // Dog → Animal
```

### V praxi:

- správna inicializácia dedičnej hierarchie,
    
- odovzdanie povinných parametrov rodičovi.
    

```java
class Animal {
    private String name;
    public Animal(String name) {
        this.name = name;
    }
}

class Dog extends Animal {
    public Dog(String name) {
        super(name); // calls Animal(String)
    }
}
```

---

## 6. Private constructor:

Zabraňuje vytváraniu inštancií zvonka – ideálne pre **singleton** alebo util triedy.

### Význam:

```java
Logger.getInstance().log("Hello");
```

### V praxi:

- Singleton pattern,
    
- utility triedy (`java.lang.Math`),
    
- kontrola počtu inštancií.
    

```java
public class Logger {
    private static final Logger INSTANCE = new Logger();
    private Logger() {}
    public static Logger getInstance() { return INSTANCE; }
}
```

---

## 7. Builder pattern constructor:

Konštruktor je **súkromný** a objekty sa tvoria cez vnútorný `Builder` pre čitateľnosť pri množstve voliteľných polí.

### Význam:

```java
User u = User.builder().name("Eva").age(30).build();
```

### V praxi:

- tvorba immutable objektov,
    
- DSL‑štýl konfigurácie,
    
- nahrádza desiatky preťažených konštruktorov.
    

```java
class User {
    private final String name;
    private final int age;

    private User(Builder b) {
        this.name = b.name;
        this.age  = b.age;
    }

    public static Builder builder() { return new Builder(); }

    public static class Builder {
        private String name;
        private int age;
        public Builder name(String n) { this.name = n; return this; }
        public Builder age(int a)     { this.age  = a; return this; }
        public User build()           { return new User(this); }
    }
}
```

---

## 8. Immutable object constructor:

Všetky polia sú `final`, trieda je **nemenná** po vytvorení.

### Význam:

```java
Person p = new Person("Nina", 28);
```

### V praxi:

- DTO vracané zo service vrstvy,
    
- hodnotové objekty v DDD,
    
- thread‑safe zdieľanie.
    

```java
public final class Person {
    private final String name;
    private final int    age;

    public Person(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    public String getName() { return name; }
    public int    getAge()  { return age;  }
}
```
