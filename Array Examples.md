
### ArrayList
```java
List<String> crew = new ArrayList<>();
crew.add("Picard");
crew.add("Data");
crew.add(1, "Riker");        // middle insert – shifts 1 slot
System.out.println(crew.get(0)); // Picard
crew.remove("Data");
```



---

### 🧵 `String[]` (plain array)


```java
String[] vowels = {"a","e","i","o","u"};
vowels[2] = "î";             // direct write
System.out.println(vowels.length); // 5
```


```java
String[] vowels = {"a","e","i","o","u"};

// direct ops (fields, no methods)
int len = vowels.length;
String third = vowels[2];
vowels[4] = "ü";

// Arrays toolbox
String[] bigger   = Arrays.copyOf(vowels, 10);          // resize
String[] slice    = Arrays.copyOfRange(vowels, 1, 4);   // half-open
Arrays.sort(bigger, 0, len);                            // sort prefix
int idx = Arrays.binarySearch(bigger, "i");             // assumes sorted
Arrays.fill(bigger, "x");                               // blast everything
boolean same = Arrays.equals(vowels, bigger);
int mis = Arrays.mismatch(vowels, slice);               // first diff index or -1
Stream<String> stream = Arrays.stream(vowels);          // join stream ops
List<String> listView = Arrays.asList(vowels);          // fixed-size list backed by array
```


---

### 🔑 HashMap<K,V>


```java
Map<String,Integer> wordFreq = new HashMap<>();
wordFreq.put("hello", 1);
wordFreq.merge("hello", 1, Integer::sum); // ++ if present
int h = wordFreq.getOrDefault("hi", 0);
wordFreq.forEach((k,v) -> System.out.println(k + " -> " + v));
```


```java
Map<String,Integer> score = new HashMap<>();

// CRUD
score.put("Alice", 1);               // write / overwrite
score.putIfAbsent("Bob", 0);         // only if missing
int bob = score.getOrDefault("Bob", 0);
score.remove("Alice");               // drop
score.replace("Bob", 1, 2);          // CAS-style conditional replace

// Atomic updates (thread-safe *within* a single map)
score.compute("Carol", (k,v) -> v == null ? 1 : v + 1);
score.merge("Bob", 3, Integer::sum); // upsert with combiner

// Iteration goodies
for (Map.Entry<String,Integer> e : score.entrySet()) {
    System.out.printf("%s ⇒ %d%n", e.getKey(), e.getValue());
}
score.forEach((k,v) -> System.out.println(k + ":" + v));

// Bulk views
Set<String>   names   = score.keySet();
Collection<Integer> vals = score.values();
Set<Map.Entry<String,Integer>> entries = score.entrySet();
```
---

### 🌳 TreeMap (bonus)


```java
Map<Integer,String> sorted = new TreeMap<>();
sorted.put(3,"c"); sorted.put(1,"a"); sorted.put(2,"b");
System.out.println(sorted); // {1=a, 2=b, 3=c}
```


---

### 🗂️ HashSet (bonus)


```java
Set<String> tags = new HashSet<>(List.of("java","streams","collections"));
boolean fresh = tags.add("java"); // false – already there
```


Set — membership & set algebra

```java
Set<String> tags = new HashSet<>(Set.of("java","streams","immutability"));
tags.add("jvm");           // +1  
tags.addAll(List.of("parsing","java")); // bulk (duplicates auto-ignored)

boolean hasStreams = tags.contains("streams");  // test
tags.remove("parsing");     // −1
int n = tags.size();        // count
tags.isEmpty();             // quick emptiness check
tags.clear();               // nuke all

// ✨ “set math”
Set<String> more = Set.of("jvm","gc","bytecode");
Set<String> union       = new HashSet<>(tags); union.addAll(more);        // ∪
Set<String> intersection= new HashSet<>(tags); intersection.retainAll(more); // ∩
Set<String> diff        = new HashSet<>(tags); diff.removeAll(more);      // tags \ more
```

---
