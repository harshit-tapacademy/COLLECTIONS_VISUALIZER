# 🔥 COMPARABLE INTERFACE — Complete Teaching Flow & Script

---

## 📋 PRE-CLASS CHECKLIST

- ✅ Students already know: `ArrayList`, `LinkedList`, `HashSet`, `LinkedHashSet`, `TreeSet`, `PriorityQueue`, `ArrayDeque`, `Stack`, `Queue`
- ✅ Students already know: `Collections.sort()` works on `List<Integer>`, `List<String>`
- ✅ Today's goal: **WHY** does `sort()` work on Integer but **CRASHES** on Employee? → Reveal the **contract** behind sorting

---

## ⏱️ CLASS TIMELINE (~75–90 minutes)

| Time      | Section                               | What Happens                                      |
| --------- | ------------------------------------- | ------------------------------------------------- |
| 0–5 min   | Hook — The Crash                      | Show `sort()` failing on custom objects           |
| 5–15 min  | Phase 1 — Integer's Secret            | How `Integer` already implements `Comparable`     |
| 15–25 min | Phase 2 — Dry Run: `[55, 90]`         | Step-by-step `compareTo` trace — no swap          |
| 25–35 min | Phase 3 — Dry Run: `[100, 50]`        | Step-by-step `compareTo` trace — swap happens     |
| 35–50 min | Phase 4 — The Employee Problem        | Build `Employee implements Comparable<Employee>`  |
| 50–65 min | Phase 5 — Dry Run: `[tim, sundar]`    | Trace `compareTo` with Employee objects by `id`   |
| 65–75 min | Phase 6 — Real-World & Interview      | Where this is used in production + top company Qs |
| 75–90 min | Phase 7 — Bonus: Tips, Traps & Tricks | Common mistakes, `Comparator` teaser, rapid-fire  |

---

---

# 🎬 PHASE 0 — THE HOOK (0–5 min)

## 🗣️ Script

> _"Okay everyone, quick question — you've all sorted an `ArrayList<Integer>` using `Collections.sort()`, right? Easy. One line. Done."_

```java
ArrayList<Integer> al = new ArrayList<>();
al.add(100);
al.add(50);
al.add(75);
Collections.sort(al);
System.out.println(al); // [50, 75, 100] ✅ Works perfectly!
```

> _"Beautiful. Now let me try the EXACT same thing with Employee objects…"_

```java
class Employee {
    int id;
    String name;
    int salary;

    Employee(int id, String name, int salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }
}

ArrayList<Employee> al = new ArrayList<>();
al.add(new Employee(2, "Tim", 30000));
al.add(new Employee(1, "Sundar", 90000));
Collections.sort(al); // ❌ COMPILATION ERROR!
```

> _"💥 BOOM! Compilation error. But why? Same `sort()`, same `ArrayList`… what changed?"_

### 🔴 The Error Message:

```
error: no suitable method found for sort(ArrayList<Employee>)
reason: inferred type does not conform to upper bound
    inferred: Employee
    upper bound: Comparable<? super Employee>
```

> _"Read that error carefully — it says **`Comparable`**. Java is telling you: 'I don't know HOW to compare two Employee objects. Are they compared by id? By name? By salary? You haven't told me!'"_

> _"And THAT, my friends, is what we'll solve today. Let's first understand how Integer manages to sort itself so effortlessly."_

### 💡 KEY INSIGHT TO PLANT:

> `Collections.sort()` doesn't know how to sort. It **delegates** the comparison to the **objects themselves**. If the objects don't know how to compare themselves → crash.

---

---

# 📘 PHASE 1 — THE SECRET INSIDE INTEGER (5–15 min)

## 🗣️ Script

> _"Let's go detective mode. Let's open the actual source code of the `Integer` class."_

### Reveal the Integer class declaration:

```java
public final class Integer extends Number implements Comparable<Integer> {
    // ...
}
```

> _"🔍 See that? `implements Comparable<Integer>`! The Integer class ALREADY implements the Comparable interface. Let's see what this interface looks like..."_

### Show the Comparable Interface:

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

> _"This interface has exactly ONE method — `compareTo()`. That's it. One method. But this ONE method is the backbone of ALL sorting in Java."_

### 🧠 Explain the Interface Structure (Use Slide 2 — Interface Diagram)

```
┌─────────────────────────────────┐
│         Comparable<T>           │
│        <<interface>>            │
│                                 │
│   + compareTo(T o) : int        │
└───────────────┬─────────────────┘
                │ implements
                │ (dashed arrow ↑)
                │
┌───────────────┴─────────────────┐
│           Integer               │
│                                 │
│   + compareTo(Integer o) {      │
│       // actual logic here      │
│   }                             │
└─────────────────────────────────┘
```

> _"The Comparable interface is a **contract**. It says: 'Any class that implements me MUST provide a `compareTo()` method. And that method MUST return an integer.'"_

### 🎯 The Contract — What `compareTo()` MUST Return:

|    Return Value    | Meaning                                     | Sort Action                    |
| :----------------: | :------------------------------------------ | :----------------------------- |
| **Positive (+ve)** | `this` object is **GREATER** than the other | **SWAP** (in ascending sort)   |
| **Negative (-ve)** | `this` object is **SMALLER** than the other | **NO SWAP** (already in order) |
|    **Zero (0)**    | Both objects are **EQUAL**                  | **NO SWAP**                    |

> _"Remember this table. Tattoo it in your brain. This is what every interview will test you on."_

### 🧠 Quick Analogy:

> _"Think of it like a boxing match. Two fighters step into the ring. `compareTo()` is the referee. The referee looks at both and says:_
>
> - _+ve → 'Left fighter wins, swap positions'_
> - _-ve → 'Right fighter wins, stay as you are'_
> - _0 → 'It's a draw, nobody moves'"_

---

### 📌 FACT CHECK — Which Wrapper Classes Implement Comparable?

| Class        | Implements Comparable? | Comparison Basis                 |
| ------------ | :--------------------: | -------------------------------- |
| `Integer`    |           ✅           | Numeric value                    |
| `Double`     |           ✅           | Numeric value                    |
| `Float`      |           ✅           | Numeric value                    |
| `Long`       |           ✅           | Numeric value                    |
| `Short`      |           ✅           | Numeric value                    |
| `Byte`       |           ✅           | Numeric value                    |
| `Character`  |           ✅           | Unicode value                    |
| `String`     |           ✅           | Lexicographic (dictionary) order |
| `Boolean`    |   ✅ (since Java 5)    | `false` < `true`                 |
| `LocalDate`  |           ✅           | Chronological order              |
| `BigDecimal` |           ✅           | Numeric value                    |

> _"That's why you can sort `ArrayList<String>`, `ArrayList<Double>`, etc. — they ALL implement Comparable. It was given to them by Java developers at Sun/Oracle."_

---

---

# 🔢 PHASE 2 — DRY RUN: Integer `[55, 90]` — Already Sorted (15–25 min)

## 🗣️ Script (Use Slide 4 — `[55, 90]`)

> _"Let's trace exactly what happens inside `sort()` when we have `[55, 90]`."_

### Setup:

```java
ArrayList<Integer> al = new ArrayList<>();
al.add(55);
al.add(90);
Collections.sort(al);
```

### Memory Visualization:

```
al → [ 55 | 90 ]
       idx 0  idx 1
```

> _"When `sort()` runs, it needs to decide: should 55 and 90 swap? It doesn't decide itself. It calls `compareTo()` on the objects."_

### Step-by-step trace:

> _"`sort()` picks the element at index 0 (which is `55`) and calls ITS `compareTo()` method, passing the element at index 1 (which is `90`) as the argument."_

```java
// sort() internally calls:
55.compareTo(90)
// Remember: 55 is "this", 90 is the "parameter"
```

### Inside Integer's `compareTo()`:

```java
// Inside the Integer class:
public int compareTo(Integer val2) {  // val2 = 90
    Integer val1 = this;               // val1 = 55 (the object on which compareTo was called)

    if (val1 > val2) {                 // Is 55 > 90? → NO ❌
        return positive;               // Skip
    }
    else if (val1 < val2) {            // Is 55 < 90? → YES ✅
        return negative;               // 🎯 Returns NEGATIVE
    }
    else {
        return 0;                      // Skip
    }
}
```

### Result Analysis:

```
compareTo() returned: NEGATIVE (-ve)

-ve means: "this" (55) is SMALLER than "other" (90)
-ve means: Already in correct ascending order
-ve means: ❌ NO SWAP

Final array: [ 55 | 90 ] → UNCHANGED ✅
```

> _"55 is less than 90, so `compareTo()` returns negative. Negative means 'I'm smaller, I should come first in ascending order.' So no swap needed. The array stays [55, 90]."_

### 🧑‍🏫 IMPROVISATION — Ask the Class:

> _"Quick — if the list was `[55, 55]`, what would `compareTo()` return?"_  
> **Answer: 0 (zero) — they're equal, no swap.**

> _"And if I wanted **descending** order? What would I need to change?"_
> **Plant this seed — you'll answer it in Comparator later.**

---

---

# 🔢 PHASE 3 — DRY RUN: Integer `[100, 50]` — Needs Swap (25–35 min)

## 🗣️ Script (Use Slide 5 — `[100, 50]`)

> _"Now let's see the opposite case. What if the bigger number comes first?"_

### Setup:

```java
ArrayList<Integer> al = new ArrayList<>();
al.add(100);
al.add(50);
Collections.sort(al);
```

### Memory Visualization:

```
al → [ 100 | 50 ]
      idx 0  idx 1
```

> _"`sort()` again picks element at index 0 (100) and calls `compareTo()`, passing element at index 1 (50)."_

```java
// sort() internally calls:
100.compareTo(50)
// 100 is "this", 50 is the "parameter"
```

### Inside Integer's `compareTo()`:

```java
public int compareTo(Integer val2) {  // val2 = 50
    Integer val1 = this;               // val1 = 100

    if (val1 > val2) {                 // Is 100 > 50? → YES ✅
        return positive;               // 🎯 Returns POSITIVE
    }
    else if (val1 < val2) {
        return negative;               // Skip
    }
    else {
        return 0;                      // Skip
    }
}
```

### Result Analysis:

```
compareTo() returned: POSITIVE (+ve)

+ve means: "this" (100) is GREATER than "other" (50)
+ve means: 100 should NOT come before 50 in ascending order
+ve means: ✅ SWAP!

Before swap: [ 100 | 50 ]
After swap:  [ 50 | 100 ] ✅ SORTED!
```

> _"100 is greater than 50, so `compareTo()` returns positive. Positive means 'I'm bigger, swap me!' And that's exactly what `sort()` does."_

### 📊 Summary Table — Both Cases:

|  ArrayList  | `compareTo()` Call  | `this` | `param` | Condition | Returns | Action  |   Result    |
| :---------: | :-----------------: | :----: | :-----: | :-------: | :-----: | :-----: | :---------: |
| `[55, 90]`  | `55.compareTo(90)`  |   55   |   90    |  55 < 90  | **-ve** | No Swap | `[55, 90]`  |
| `[100, 50]` | `100.compareTo(50)` |  100   |   50    | 100 > 50  | **+ve** |  Swap   | `[50, 100]` |

> _"Now you fully understand how `sort()` works with Integer. But here's the big question — Integer already has `compareTo()` written for it by Java. What about YOUR custom classes like Employee? NOBODY wrote `compareTo()` for Employee. That's YOUR job."_

---

### 🧠 ACTUAL Integer compareTo() Source Code (Bonus Fact)

> _"Fun fact — here's what the ACTUAL source code looks like inside `java.lang.Integer`:"_

```java
public int compareTo(Integer anotherInteger) {
    return compare(this.value, anotherInteger.value);
}

public static int compare(int x, int y) {
    return (x < y) ? -1 : ((x == y) ? 0 : 1);
}
```

> _"It doesn't return random positive/negative numbers. It returns exactly `-1`, `0`, or `1`. This is a best practice — but your custom `compareTo()` can return ANY positive or negative number. The sign is what matters, not the magnitude."_

---

---

# 👨‍💼 PHASE 4 — THE EMPLOYEE PROBLEM (35–50 min)

## 🗣️ Script

> _"Alright. Now the real deal. Let's build the Employee class and make it sortable."_

### Step 1: The Problem

```java
class Employee {
    int id;
    String name;
    int salary;

    Employee(int id, String name, int salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }
}

ArrayList<Employee> al = new ArrayList<>();
al.add(new Employee(2, "Tim", 30000));
al.add(new Employee(1, "Sundar", 90000));

Collections.sort(al); // ❌ COMPILE ERROR!
```

> _"Java says: 'I don't know how to compare two Employees. Should I compare by id? name? salary? You have to TELL me.'"_

### Step 2: The Solution — Implement Comparable (Use Slide 2 - Interface Diagram)

```
┌─────────────────────────────────┐
│      Comparable<Employee>       │
│         <<interface>>           │
│                                 │
│   + compareTo(Employee) : int  │
└───────────────┬─────────────────┘
                │ implements
                │ (dashed arrow ↑)
                │
┌───────────────┴─────────────────┐
│           Employee              │
│                                 │
│   - int id                     │
│   - String name                │
│   - int salary                 │
│                                 │
│   + compareTo(Employee o) {    │
│       // YOUR logic here       │
│   }                            │
└─────────────────────────────────┘
```

> _"We implement `Comparable<Employee>` and provide OUR comparison logic. Let's say we want to sort by `id`."_

### Step 3: The Implementation

```java
class Employee implements Comparable<Employee> {
    int id;
    String name;
    int salary;

    Employee(int id, String name, int salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    @Override
    public int compareTo(Employee other) {
        // "this" is the current object
        // "other" is the parameter object

        Employee tim = this;          // The object calling compareTo
        Employee sundar = other;      // The object passed as parameter

        Integer id1 = tim.getId();    // id of "this"
        Integer id2 = sundar.getId(); // id of "other"

        if (id1 > id2) {
            return 1;                 // positive → swap
        }
        else if (id1 < id2) {
            return -1;               // negative → no swap
        }
        else {
            return 0;                // equal → no swap
        }
    }

    public int getId() { return id; }
    public String getName() { return name; }
    public int getSalary() { return salary; }
}
```

> _"Now Employee has a `compareTo()` method. It says: 'Compare me with another Employee by our `id` fields.' That's the **natural ordering** of Employee."_

### 🧠 IMPORTANT CONCEPT — Natural Ordering

> _"When you implement `Comparable` on a class, you're defining that class's **NATURAL ORDERING** — the default way objects of that class should be sorted. For `Integer`, the natural ordering is by numeric value. For `String`, it's alphabetical. For our `Employee`, we chose it to be by `id`."_

> _"A class can have only ONE natural ordering (one `compareTo()`). If you want to sort by salary sometimes and by name other times, that's where `Comparator` comes in — but that's a topic for another day."_

---

### 💡 PRO TIP — Shorter Way to Write compareTo()

> _"In industry, nobody writes those if-else blocks. Here's the clean way:"_

```java
// Way 1: Using Integer.compare() — BEST PRACTICE
@Override
public int compareTo(Employee other) {
    return Integer.compare(this.id, other.id);
}

// Way 2: Simple subtraction (⚠️ risky with large numbers — integer overflow)
@Override
public int compareTo(Employee other) {
    return this.id - other.id;   // ascending by id
}

// Way 3: For descending order
@Override
public int compareTo(Employee other) {
    return other.id - this.id;   // descending by id
}

// Way 4: Comparing by String field (name)
@Override
public int compareTo(Employee other) {
    return this.name.compareTo(other.name);  // alphabetical by name
}
```

> _"⚠️ WARNING about subtraction: `this.id - other.id` can cause integer overflow if values are near `Integer.MAX_VALUE` or `Integer.MIN_VALUE`. Always prefer `Integer.compare()` in production."_

---

---

# 🏃 PHASE 5 — DRY RUN: Employee `[tim, sundar]` by ID (50–65 min)

## 🗣️ Script (Use Slide 1 & Slide 3)

> _"Let's trace through this step by step, just like we did with Integer."_

### Setup:

```java
ArrayList<Employee> al = new ArrayList<>();
al.add(new Employee(2, "Tim", 30000));     // index 0
al.add(new Employee(1, "Sundar", 90000));  // index 1
Collections.sort(al);
```

### Memory Visualization (Use Slide 3):

```
al → [ 1000 | 2000 ]        ← ArrayList stores REFERENCES (memory addresses)
      idx 0    idx 1

1000 ──→ ┌──────────────┐    2000 ──→ ┌──────────────┐
         │  id:     2   │             │  id:     1   │
 tim ──→ │  name: "Tim" │  sundar ──→│  name:"Sundar│
         │  salary:30000│             │  salary:90000│
         └──────────────┘             └──────────────┘
```

> _"Very important — the `ArrayList` does NOT store the actual Employee objects inside it. It stores REFERENCES (memory addresses like 1000, 2000) that POINT to the Employee objects on the heap. When we swap, we swap the references, not the objects."_

### Step-by-step `sort()` trace:

> _"`sort()` picks the object at index 0 (tim, id=2) and calls `compareTo()`, passing the object at index 1 (sundar, id=1)."_

```java
// sort() internally calls:
tim.compareTo(sundar)
// tim is "this", sundar is the "parameter"
```

### Inside Employee's `compareTo()` (Use Slide 1):

```java
public int compareTo(Employee sundar) {  // sundar is the parameter
    Employee tim = this;                  // tim is "this" (the caller)

    Integer id1 = tim.getId();           // id1 = 2
    Integer id2 = sundar.getId();        // id2 = 1

    if (id1 > id2) {                     // Is 2 > 1? → YES ✅
        return positive;                  // 🎯 Returns POSITIVE (+ve)
    }
    else if (id1 < id2) {
        return negative;                  // Skip
    }
    else {
        return 0;                         // Skip
    }
}
```

### Result Analysis:

```
compareTo() returned: POSITIVE (+ve)

+ve means: "this" (tim, id=2) is GREATER than "other" (sundar, id=1)
+ve means: tim should NOT come before sundar in ascending order
+ve means: ✅ SWAP the references!

Before swap: al → [ 1000(tim) | 2000(sundar) ]
After swap:  al → [ 2000(sundar) | 1000(tim) ] ✅

Sorted by id: sundar(1), tim(2) → ASCENDING ORDER!
```

> _"The ArrayList swapped the REFERENCES. sundar (id=1) now comes first, tim (id=2) comes second. Sorted in ascending order by id!"_

### 🧑‍🏫 IMPROVISATION — Ask the Class:

> _"What if we wanted to sort by SALARY instead of ID? What would we change?"_

```java
// Just change the compareTo() logic:
@Override
public int compareTo(Employee other) {
    return Integer.compare(this.salary, other.salary);
}

// Result: Tim(30000), Sundar(90000) → ascending by salary
```

> _"What if we wanted to sort by NAME?"_

```java
@Override
public int compareTo(Employee other) {
    return this.name.compareTo(other.name);
}

// Result: Sundar, Tim → alphabetical order
// (because 'S' comes before 'T' in Unicode)
```

> _"But wait — can we sort by ID sometimes and salary others? NOT with Comparable alone. You'd need Comparator. Teaser for next class! 😉"_

---

---

# 🌍 PHASE 6 — REAL-WORLD IMPORTANCE & INTERVIEW PREPARATION (65–75 min)

## 🗣️ Script

> _"Now let's talk about WHY this matters beyond the classroom. Comparable isn't just a textbook concept — it's running inside Netflix, Amazon, Google, Uber right now."_

---

## 🏢 Where Comparable Is Used in Real-World Software

### 1. 🛒 E-Commerce (Amazon, Flipkart, Myntra)

```java
class Product implements Comparable<Product> {
    String name;
    double price;
    double rating;

    @Override
    public int compareTo(Product other) {
        return Double.compare(this.price, other.price); // Default: sort by price
    }
}

// When user clicks "Sort by Price: Low to High" → Collections.sort(products)
// The default natural ordering is the MOST commonly used sort
```

> _"When you open Amazon and sort by 'Price: Low to High', behind the scenes, a `compareTo()` on the Product class is being called millions of times per second."_

### 2. 🏦 Banking & Financial Systems (Goldman Sachs, Morgan Stanley, JP Morgan)

```java
class Transaction implements Comparable<Transaction> {
    LocalDateTime timestamp;
    double amount;
    String type;

    @Override
    public int compareTo(Transaction other) {
        return this.timestamp.compareTo(other.timestamp); // Chronological order
    }
}

// Bank statement: transactions sorted by date
// Fraud detection: sort transactions to find patterns
```

### 3. 📊 Leaderboard Systems (Gaming, Sports, Competitive Programming)

```java
class Player implements Comparable<Player> {
    String username;
    int score;
    long timeTaken; // milliseconds

    @Override
    public int compareTo(Player other) {
        // Higher score first (descending)
        // If tie → less time first (ascending)
        if (this.score != other.score) {
            return Integer.compare(other.score, this.score); // descending
        }
        return Long.compare(this.timeTaken, other.timeTaken); // ascending
    }
}
```

> _"Every competitive coding platform — LeetCode, Codeforces, HackerRank — uses this exact pattern for their leaderboards."_

### 4. 🚗 Ride-Hailing (Uber, Ola, Lyft)

```java
class Driver implements Comparable<Driver> {
    String name;
    double distanceFromUser; // in km
    double rating;

    @Override
    public int compareTo(Driver other) {
        return Double.compare(this.distanceFromUser, other.distanceFromUser);
    }
}

// Show nearest drivers first
```

### 5. 📋 Task/Priority Management (Jira, Asana, Trello)

```java
class Task implements Comparable<Task> {
    String title;
    Priority priority; // HIGH, MEDIUM, LOW (enum implementing Comparable)
    LocalDate deadline;

    @Override
    public int compareTo(Task other) {
        return this.priority.compareTo(other.priority); // Enums are naturally ordered
    }
}
```

### 6. 🏥 Hospital Emergency Queue

```java
class Patient implements Comparable<Patient> {
    String name;
    int severityLevel; // 1 = critical, 5 = minor
    LocalDateTime arrivalTime;

    @Override
    public int compareTo(Patient other) {
        // Lower severity = more critical = comes first
        if (this.severityLevel != other.severityLevel) {
            return Integer.compare(this.severityLevel, other.severityLevel);
        }
        // Same severity → who arrived first
        return this.arrivalTime.compareTo(other.arrivalTime);
    }
}
// Used with PriorityQueue<Patient> in hospital management systems!
```

> _"Remember PriorityQueue from last class? NOW you know why it requires Comparable — it uses `compareTo()` to maintain the heap structure!"_

---

## 🔗 Connection to What You've Already Learned

|   Collection    |     Uses Comparable?     | How?                                            |
| :-------------: | :----------------------: | :---------------------------------------------- |
|   `ArrayList`   | Via `Collections.sort()` | Calls `compareTo()` during sorting              |
|  `LinkedList`   | Via `Collections.sort()` | Same as above                                   |
|    `TreeSet`    |     ✅ **Directly**      | Uses `compareTo()` for BST insertion + ordering |
| `PriorityQueue` |     ✅ **Directly**      | Uses `compareTo()` for heap ordering            |
|    `HashSet`    |          ❌ No           | Uses `hashCode()` + `equals()`, not ordering    |
| `LinkedHashSet` |          ❌ No           | Uses insertion order, not comparison            |
|  `ArrayDeque`   |          ❌ No           | Not a sorted collection                         |

> _"TreeSet and PriorityQueue will THROW `ClassCastException` at RUNTIME if you try to add objects that don't implement Comparable (and no Comparator is provided). This is a very common interview question!"_

```java
TreeSet<Employee> ts = new TreeSet<>();
ts.add(new Employee(1, "Tim", 30000));
// ❌ ClassCastException: Employee cannot be cast to Comparable
// Because TreeSet needs to compare to decide BST left/right placement
```

---

## 🎯 Top Tech Companies That Ask Comparable in Interviews

### 🔴 TIER 1 — FAANG / MAANG + Top Product Companies

| Company             | What They Ask                                                                                                                                                | Difficulty |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------: |
| **Google**          | Design a custom sorting order for search results using Comparable. "Sort employees by department, then by salary within department." Multi-level comparison. |  ⭐⭐⭐⭐  |
| **Amazon**          | "Sort a list of products by multiple criteria." Frequently tests Contract understanding — what happens if compareTo is inconsistent with equals?             |   ⭐⭐⭐   |
| **Microsoft**       | "Implement a comparable class for a ticket system." Focus on natural ordering + TreeMap/TreeSet usage.                                                       |   ⭐⭐⭐   |
| **Meta (Facebook)** | Less direct — but tests through system design problems involving sorted data structures.                                                                     |   ⭐⭐⭐   |
| **Apple**           | "Sort notifications by priority and timestamp." Tests multi-field comparison.                                                                                |   ⭐⭐⭐   |
| **Netflix**         | "Design content ranking/recommendation sort." Natural ordering of shows by rating, release date.                                                             |  ⭐⭐⭐⭐  |

### 🟡 TIER 2 — Finance / Enterprise

| Company                        | What They Ask                                                                                                                                            |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Goldman Sachs**              | "Sort trade orders by timestamp with tie-breaking by price." Very rigorous on the Comparable contract. Also asks about Comparable vs Comparator heavily. |
| **JP Morgan / Morgan Stanley** | "Implement Comparable for a financial instrument class." Focus on consistency with `equals()`.                                                           |
| **Bloomberg**                  | Sorting of real-time market data feeds. Tests understanding of natural ordering.                                                                         |

### 🟢 TIER 3 — Indian Product / Service Companies

| Company                     | What They Ask                                                                       |
| --------------------------- | ----------------------------------------------------------------------------------- |
| **Flipkart / Walmart Labs** | "Sort product catalog by multiple attributes." Practical e-commerce scenarios.      |
| **Paytm / PhonePe**         | "Sort transactions by date." Straightforward but tests fundamentals.                |
| **Infosys / TCS / Wipro**   | "What is Comparable? Write compareTo() for a Student class." Theory + basic coding. |
| **Zoho**                    | Custom object sorting is a staple in their hiring challenge rounds.                 |
| **Atlassian**               | "Design JIRA-like ticket priority system." Natural ordering of issues.              |

---

## 🔥 TOP 15 INTERVIEW QUESTIONS ON COMPARABLE

### Level 1 — Conceptual (Every Company)

**Q1: What is the Comparable interface? Where is it defined?**

> **A:** `Comparable` is a functional interface in `java.lang` package. It has a single method `compareTo(T o)` that returns an `int`. It defines the **natural ordering** of objects.

**Q2: What is "natural ordering"?**

> **A:** The default way objects of a class are sorted. For `String` → alphabetical, for `Integer` → numeric, for `LocalDate` → chronological. Defined by the class's `compareTo()` method.

**Q3: What does `compareTo()` return?**

> **A:**
>
> - **Positive integer** → `this` is greater than `other`
> - **Negative integer** → `this` is less than `other`
> - **Zero** → `this` is equal to `other`

**Q4: Comparable vs Comparator — when to use which?**

> | Feature          |               Comparable               |                   Comparator                    |
> | ---------------- | :------------------------------------: | :---------------------------------------------: |
> | Package          |              `java.lang`               |                   `java.util`                   |
> | Method           |            `compareTo(T o)`            |              `compare(T o1, T o2)`              |
> | # of sort orders |         **1** (natural order)          |                  **Multiple**                   |
> | Modifies class?  |     **Yes** (class implements it)      |             **No** (separate class)             |
> | Use when         | You OWN the class or want default sort | You DON'T own the class or need alternate sorts |

---

### Level 2 — Coding (Product Companies)

**Q5: Write a `Student` class that sorts by marks descending, then by name alphabetically if marks are equal.**

```java
class Student implements Comparable<Student> {
    String name;
    int marks;

    @Override
    public int compareTo(Student other) {
        if (this.marks != other.marks) {
            return Integer.compare(other.marks, this.marks); // descending marks
        }
        return this.name.compareTo(other.name); // ascending name
    }
}
```

**Q6: Why should `compareTo()` be consistent with `equals()`?**

> **A:** If `a.compareTo(b) == 0` but `a.equals(b) == false`, then `TreeSet` and `TreeMap` will treat them as equal (no duplicates), but `HashSet` and `HashMap` might not. This causes inconsistent behavior.
>
> **Example:** `BigDecimal` has this issue:
>
> ```java
> BigDecimal a = new BigDecimal("1.0");
> BigDecimal b = new BigDecimal("1.00");
> a.equals(b);      // false! (different scale)
> a.compareTo(b);   // 0 (same mathematical value)
>
> TreeSet<BigDecimal> ts = new TreeSet<>();
> ts.add(a); ts.add(b);
> ts.size(); // 1 ← compareTo says equal
>
> HashSet<BigDecimal> hs = new HashSet<>();
> hs.add(a); hs.add(b);
> hs.size(); // 2 ← equals says NOT equal
> ```
>
> This is a **classic Goldman Sachs / Amazon interview question!**

**Q7: What happens if you don't implement Comparable and try to add to TreeSet?**

> **A:** `ClassCastException` at runtime (NOT compile time — it compiles fine because TreeSet accepts `Object`, but at runtime it tries to cast to `Comparable`).

**Q8: Can we sort `null` elements using Comparable?**

> **A:** `compareTo()` does NOT handle `null`. It will throw `NullPointerException`. The contract states: `e.compareTo(null)` should throw NPE. `Comparator.nullsFirst()` or `Comparator.nullsLast()` handle nulls.

---

### Level 3 — Tricky / Design (FAANG)

**Q9: What are the rules of the `compareTo()` contract?**

> **The 3 Rules (from Javadoc):**
>
> 1. **Antisymmetry:** `sgn(x.compareTo(y)) == -sgn(y.compareTo(x))`
>    - If x > y, then y < x. If x.compareTo(y) throws, y.compareTo(x) must also throw.
> 2. **Transitivity:** If `x.compareTo(y) > 0` and `y.compareTo(z) > 0`, then `x.compareTo(z) > 0`
> 3. **Consistency:** If `x.compareTo(y) == 0`, then `sgn(x.compareTo(z)) == sgn(y.compareTo(z))` for all z

**Q10: What is the danger of using subtraction in `compareTo()`?**

```java
// DANGEROUS — Integer overflow!
public int compareTo(Employee other) {
    return this.id - other.id;
}

// If this.id = Integer.MAX_VALUE and other.id = -1
// Result = Integer.MAX_VALUE - (-1) = Integer.MAX_VALUE + 1 = Integer.MIN_VALUE (NEGATIVE!)
// This gives WRONG comparison result!

// SAFE alternative:
public int compareTo(Employee other) {
    return Integer.compare(this.id, other.id);
}
```

> _"This is a favorite Google and Amazon question. They give you subtraction-based compareTo and ask 'what's wrong with this?'"_

**Q11: How does `Collections.sort()` actually use `compareTo()` internally?**

> **A:** `Collections.sort()` uses **TimSort** (a hybrid of merge sort and insertion sort) since Java 7. It calls `compareTo()` to determine the relative order of elements. TimSort has:
>
> - **Time Complexity:** O(n log n) average and worst case
> - **Space Complexity:** O(n)
> - **Stable:** Equal elements maintain their original order
> - **Adaptive:** Faster on partially sorted data

**Q12: Can an `enum` implement Comparable?**

> **A:** Enums already implement `Comparable` by default! They are compared by their **ordinal** (declaration order).
>
> ```java
> enum Priority { LOW, MEDIUM, HIGH }
> Priority.LOW.compareTo(Priority.HIGH); // negative (LOW declared before HIGH)
> ```
>
> ⚠️ You **cannot override** `compareTo()` in an enum — it's declared `final` in `java.lang.Enum`.

**Q13: Design a class where objects are sorted by multiple criteria.**

> _"Sort flights by: price (ascending) → duration (ascending) → departure time (ascending)"_

```java
class Flight implements Comparable<Flight> {
    double price;
    int durationMinutes;
    LocalTime departure;

    @Override
    public int compareTo(Flight other) {
        // 1. Compare by price
        int result = Double.compare(this.price, other.price);
        if (result != 0) return result;

        // 2. If price equal, compare by duration
        result = Integer.compare(this.durationMinutes, other.durationMinutes);
        if (result != 0) return result;

        // 3. If duration also equal, compare by departure time
        return this.departure.compareTo(other.departure);
    }
}
```

**Q14: Java 8+ — Can you write compareTo() using Comparator chain?**

```java
// Modern Java — using Comparator.comparing() inside compareTo()
private static final Comparator<Employee> NATURAL_ORDER =
    Comparator.comparingInt(Employee::getId)
              .thenComparing(Employee::getName)
              .thenComparingInt(Employee::getSalary);

@Override
public int compareTo(Employee other) {
    return NATURAL_ORDER.compare(this, other);
}
```

**Q15: What is a `Comparable` anti-pattern? (Google/Amazon love this)**

> **A:** Making `compareTo()` depend on mutable fields! If the field changes after the object is inserted into a `TreeSet` or `TreeMap`, the data structure breaks because it can't find the object anymore.
>
> ```java
> TreeSet<Employee> set = new TreeSet<>();
> Employee e = new Employee(1, "Tim", 30000);
> set.add(e);
> e.id = 999;        // 💀 MUTATED the comparison key!
> set.contains(e);   // false! TreeSet can't find it anymore
> set.remove(e);     // false! Can't remove it either!
> // The object is now "lost" inside the tree — a memory leak!
> ```

---

---

# 🎁 PHASE 7 — BONUS: Tips, Traps & Quick-Fire (75–90 min)

## 🚨 Common Mistakes Students Make

### Mistake 1: Forgetting the Generic Type

```java
// ❌ WRONG — Raw type
class Employee implements Comparable {
    public int compareTo(Object o) {
        Employee other = (Employee) o;  // Ugly cast, unsafe
        return this.id - other.id;
    }
}

// ✅ CORRECT — Parameterized type
class Employee implements Comparable<Employee> {
    public int compareTo(Employee other) {  // Type-safe, no casting
        return Integer.compare(this.id, other.id);
    }
}
```

### Mistake 2: Returning Only `0` and `1`

```java
// ❌ WRONG — Missing negative case
public int compareTo(Employee other) {
    if (this.id > other.id) return 1;
    return 0;  // This says "I'm equal" even when I'm LESS!
}

// ✅ CORRECT — Handle all 3 cases
public int compareTo(Employee other) {
    return Integer.compare(this.id, other.id);
}
```

### Mistake 3: Inconsistent `compareTo()` and `equals()`

```java
// ❌ Problematic — compareTo uses id, equals uses name
public int compareTo(Employee other) {
    return Integer.compare(this.id, other.id);
}

public boolean equals(Object obj) {
    Employee other = (Employee) obj;
    return this.name.equals(other.name);  // DIFFERENT field!
}

// This breaks TreeSet vs HashSet behavior
```

---

## ⚡ RAPID-FIRE ROUND (Ask the class!)

|  #  | Question                                                          | Answer                                                       |
| :-: | ----------------------------------------------------------------- | ------------------------------------------------------------ |
|  1  | Which package is `Comparable` in?                                 | `java.lang` (auto-imported)                                  |
|  2  | Which package is `Comparator` in?                                 | `java.util` (needs import)                                   |
|  3  | How many methods in `Comparable`?                                 | 1 — `compareTo()`                                            |
|  4  | Is `Comparable` a functional interface?                           | ✅ Yes (exactly one abstract method)                         |
|  5  | Can a class implement both `Comparable` and `Comparator`?         | Yes, but it's unusual and bad practice                       |
|  6  | Does `Arrays.sort()` also use `Comparable`?                       | ✅ Yes — works the same way as `Collections.sort()`          |
|  7  | What algorithm does `Collections.sort()` use?                     | TimSort (since Java 7)                                       |
|  8  | Is `compareTo()` sorting ascending or descending by default?      | It's up to YOUR implementation — convention is ascending     |
|  9  | What exception if you forget to implement Comparable for TreeSet? | `ClassCastException` (runtime)                               |
| 10  | Can you use `Comparable` with `HashMap`?                          | No — HashMap uses `hashCode()`/`equals()`, not `compareTo()` |

---

## 🎯 HOMEWORK / CHALLENGE FOR STUDENTS

### Easy:

1. Create a `Book` class with `title`, `price`, `pages`. Implement `Comparable<Book>` to sort by price ascending.
2. Create an `ArrayList<Book>`, add 5 books, sort them, and print the sorted list.

### Medium:

3. Create a `Student` class with `rollNo`, `name`, `cgpa`. Sort by CGPA descending. If CGPA is same, sort by name alphabetically.
4. Add these students to a `TreeSet<Student>` and observe the behavior.

### Hard:

5. Create an `Employee` class that implements `Comparable<Employee>`. Sort by department (alphabetically), then by salary (descending within each department). Add the employees to both `TreeSet` and `ArrayList`, sort the ArrayList, and verify both give the same order.
6. What happens if two employees have the same department AND same salary? How does your `compareTo()` handle it? (Hint: add a tiebreaker)

---

## 🗂️ CHEAT SHEET — compareTo() Templates

```java
// ── Ascending by int field ──
return Integer.compare(this.field, other.field);

// ── Descending by int field ──
return Integer.compare(other.field, this.field);

// ── Ascending by String field ──
return this.name.compareTo(other.name);

// ── Descending by String field ──
return other.name.compareTo(this.name);

// ── Case-insensitive String comparison ──
return this.name.compareToIgnoreCase(other.name);

// ── Ascending by double field ──
return Double.compare(this.price, other.price);

// ── Multi-field comparison ──
int result = Integer.compare(this.priority, other.priority);
if (result != 0) return result;
result = this.name.compareTo(other.name);
if (result != 0) return result;
return Long.compare(this.timestamp, other.timestamp);

// ── Java 8+ Modern style ──
// (inside Comparator, but can be used in compareTo delegate)
Comparator.comparingInt(Employee::getId)
          .thenComparing(Employee::getName)
          .thenComparingDouble(Employee::getSalary);
```

---

## 🧩 THE BIG PICTURE — Where Comparable Fits

```
                    ┌──────────────────────────────┐
                    │      java.lang.Comparable     │
                    │      (Natural Ordering)       │
                    │      ONE way to sort          │
                    └──────────────┬───────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                     │
    ┌─────────▼────────┐  ┌───────▼─────────┐  ┌───────▼──────────┐
    │ Collections.sort()│  │    TreeSet       │  │  PriorityQueue   │
    │ Arrays.sort()     │  │    TreeMap       │  │                  │
    │ (Lists & Arrays)  │  │ (Sorted Sets/Maps)│ │ (Heap Structure) │
    └──────────────────┘  └─────────────────┘  └──────────────────┘
              │
              │ Need multiple sort orders?
              ▼
    ┌──────────────────┐
    │ java.util.        │
    │ Comparator        │
    │ (Custom Ordering) │
    │ MANY ways to sort │
    └──────────────────┘
```

---

## 📝 KEY TAKEAWAYS (Write on Board / Final Slide)

1. **`Comparable<T>`** is an interface in `java.lang` with ONE method: `compareTo(T o)`
2. It defines the **natural ordering** of a class — the default way objects are sorted
3. **+ve = swap, -ve = no swap, 0 = equal** (for ascending order)
4. All wrapper classes (`Integer`, `String`, etc.) already implement `Comparable`
5. Custom classes MUST implement `Comparable` (or provide a `Comparator`) to be sorted
6. `TreeSet`, `PriorityQueue`, `TreeMap` **require** `Comparable` or `Comparator`
7. Use `Integer.compare()` instead of subtraction to avoid integer overflow
8. `compareTo()` should be **consistent with `equals()`** (recommended, not mandatory)
9. **Natural ordering** = only ONE `compareTo()`. For multiple sort orders → use `Comparator`
10. **Never mutate** the field used in `compareTo()` after adding to sorted collections

---

## 🔮 TEASER FOR NEXT CLASS — Comparator Interface

> _"Today we learned Comparable — where the object decides how IT should be compared. But what if:_
>
> - _You don't own the source code of the class?_
> - _You want to sort by salary TODAY and by name TOMORROW?_
> - _You want descending order sometimes and ascending other times?_
>
> _That's where **Comparator** comes in — a separate object that defines comparison logic FROM THE OUTSIDE. See you tomorrow!"_

---

> 📅 **Document Created:** April 21, 2026  
> 🎓 **Course:** Java Collections Framework  
> 👨‍🏫 **Topic:** Comparable Interface — Complete Teaching Flow  
> 📌 **Prerequisites:** Collections (List, Set, Queue, Deque) — except Maps  
> ⏭️ **Next Topic:** Comparator Interface
