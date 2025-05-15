- Aggregations:
	1. [https://www.youtube.com/watch?v=ogyyZeKjmTE]()
	2. [https://www.youtube.com/watch?v=_TAX3WFUCe0]()
- Composition:
	1. https://www.youtube.com/watch?v=svocaCbs-Aw
	2. https://www.youtube.com/watch?v=VfTiLE3RZds
- Polymorpism(*runtime*):
	1. https://www.youtube.com/watch?v=jhDUxynEQRI
	2. https://www.youtube.com/watch?v=Ft88V_rDO4I
- Encapsulation:
	1. https://www.youtube.com/watch?v=eboNNUADeIc
	2. https://www.youtube.com/watch?v=YbqneqDIZh8
- Wrapper classes:
	1. https://www.youtube.com/watch?v=Fyc86kVIePE
	2. https://www.youtube.com/watch?v=4MiEznM8y8Q
- Generics:
	1. https://www.youtube.com/watch?v=K1iu1kXkVoA
	2. https://www.youtube.com/watch?v=H9vc4gTtGGA



## ~={green}Basics

## ~={pink}Java – practical examples of constructors:=~

- *[[Constructors examples#1. Copy constructor]]*
- *[[Constructors examples#2. Default constructor]]*
- *[[Constructors examples#3. Constructor chaining]]*
- *[[Constructors examples#4. Constructor overloading]]*
- *[[Constructors examples#5. Super constructor]]*
- *[[Constructors examples#6. Private constructor]]*
- *[[Constructors examples#7. Builder pattern constructor]]*
- *[[Constructors examples#8. Immutable object constructor]]*

## ~={pink} Generics

[[Generics]] 
   Generiká ti umožnia pracovať s rôznymi dátovými typmi bez potreby opakovaného písania toho istého kódu. Typ sa určí až pri používaní triedy alebo metódy, vďaka čomu:

- **nemusíš ručne pretypovávať hodnoty**
    
- **vyhneš sa runtime chybám typu ClassCastException**
    
- **získaš čitateľnejší a bezpečnejší kód**
    

### ✅ Príklad:

```java
Box<String> stringBox = new Box<>();
stringBox.set("Ahoj");
String s = stringBox.get(); // netreba žiadne pretypovanie
```



## ~={pink} Arrays,Maps



## ~={pink} Streams

### Most-used operations (cheat-sheet)
[https://www.youtube.com/watch?v=FWoYpM-E3EQ]()
[https://www.youtube.com/watch?v=2StXP1XaU04]()

* [[Java Streams Examples#^Filter|filter(Predicate<T>)]]
* [[Java Streams Examples#^Map|map(Function<T,R>)]]
* [[Java Streams Examples#^FlatMap|flatMap(Function<T, Stream<R>>)]]
* [[Java Streams Examples#^Collect|collect(Collectors.toList())]]
* [[Java Streams Examples#^Reduce|reduce(identity, BinaryOperator)]]
* [[Java Streams Examples#^Sorted|sorted(Comparator)]]
* [[Java Streams Examples#^Distinct|distinct()]]
* [[Java Streams Examples#^Limit-Skip|limit() / skip()]]
* [[Java Streams Examples#^Match|anyMatch() / allMatch() / noneMatch()]]

