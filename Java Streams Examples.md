### Map ^Map

Transform each element into something else.

```java
List<String> usernames = users.stream()
        .map(User::username)
        .toList();
```

---

### FlatMap ^FlatMap

Flatten nested structures (think 2-D → 1-D).

```java
List<Book> allBooks = libraries.stream()
        .flatMap(lib -> lib.books().stream())
        .toList();
```

---

### Collect ^Collect

Gather results into a container of your choice.

```java
List<OrderDto> dtos = orders.stream()
        .map(OrderDto::from)
        .collect(Collectors.toList());
```

---

### Reduce ^Reduce

Collapse the stream to a single value.

```java
BigDecimal total = cart.stream()
        .map(Item::price)
        .reduce(BigDecimal.ZERO, BigDecimal::add);
```

---

### Sorted ^Sorted

Re-order the stream.

```java
List<Customer> oldestFirst = customers.stream()
        .sorted(Comparator.comparing(Customer::birthDate))
        .toList();
```

---

### Distinct ^Distinct

Remove duplicates while preserving order.

```java
List<String> uniqueTags = posts.stream()
        .flatMap(Post::tags)
        .distinct()
        .toList();
```

---

### Limit / Skip ^Limit-Skip

Simple paging.

```java
List<Article> page2 = articles.stream()
        .skip(10)   // first page size
        .limit(10)  // page size
        .toList();
```

---

### Match ^Match

Boolean checks over the stream.

```java
boolean hasOverdue = invoices.stream()
        .anyMatch(Invoice::isOverdue);

boolean allPaid = invoices.stream()
        .allMatch(Invoice::isPaid);
```

---

## Gotchas & pro tips

- **Avoid side-effects** inside `map`—future you will thank you.
    
- Parallel streams on small lists = _slower_ (thread setup cost).
    
- Once consumed, a stream is toast—create a new one if needed.
    
- Prefer **method references** (`Person::getName`) for readability.
    