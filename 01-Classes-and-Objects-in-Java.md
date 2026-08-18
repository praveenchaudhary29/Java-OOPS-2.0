# 01 — Classes and Objects in Java

Classes and objects are the foundation of OOP in Java. For interviews, you should understand not only the definition but also how objects are created, stored in memory, and accessed.

---

## 1. What is a Class?

A **class is a blueprint/template** that defines:

- **Data** → fields/variables
- **Behavior** → methods
- **How objects should be created** → constructors

```java
class Car {
    String brand;
    int speed;

    void drive() {
        System.out.println(brand + " is driving at " + speed + " km/h");
    }
}
```

Here:

```text
Car
 ├── brand     → data
 ├── speed     → data
 └── drive()   → behavior
```

A class itself is **not a specific car**. It describes what a `Car` object will have and what it can do.

---

## 2. What is an Object?

An **object is an instance of a class**.

```java
Car car1 = new Car();
```

Here:

- `Car` → class/type
- `car1` → reference variable
- `new Car()` → creates an object
- Object → actual instance created in memory

You can create multiple objects from one class:

```java
Car car1 = new Car();
Car car2 = new Car();
Car car3 = new Car();
```

All three are `Car` objects, but they can have different data.

```java
car1.brand = "BMW";
car1.speed = 200;

car2.brand = "Audi";
car2.speed = 180;
```

Conceptually:

```text
Car class
     |
     | creates
     ↓
 ┌───────────┐
 │  car1     │
 │ BMW       │
 │ 200       │
 └───────────┘

 ┌───────────┐
 │  car2     │
 │ Audi      │
 │ 180       │
 └───────────┘
```

---

## 3. Class vs Object

| Class | Object |
|---|---|
| Blueprint/template | Actual instance |
| Logical definition | Real entity in memory |
| Defines fields and methods | Contains actual field values |
| One class can create many objects | Each object is an instance of a class |

Example:

```java
class Student {
    String name;
    int age;
}
```

`Student` is the **class**.

```java
Student s1 = new Student();
Student s2 = new Student();
```

`s1` and `s2` are **references to objects**.

---

## 4. Creating an Object

The most common way is using `new`.

```java
Student s1 = new Student();
```

There are several things happening here.

### Step 1

```java
Student
```

Specifies the type of reference.

### Step 2

```java
s1
```

Creates a reference variable.

### Step 3

```java
new Student()
```

Creates a new object.

So:

```java
Student s1 = new Student();
```

can conceptually be understood as:

```text
Stack                    Heap

s1 ───────────────────→ Student object
                         name = null
                         age  = 0
```

The reference `s1` points to the object.

---

## 5. Reference Variable vs Object

This is **very important for Java interviews**.

Consider:

```java
Student s1 = new Student();
```

The `Student` object and `s1` are **not the same thing**.

Think of:

```text
s1 ─────────→ Object
```

`s1` is a **reference** that refers to the object.

For example:

```java
Student s1 = new Student();
Student s2 = s1;
```

Now:

```text
s1 ──────┐
         ↓
      Student Object
         ↑
s2 ──────┘
```

Both references point to the **same object**.

Therefore:

```java
s1.name = "Rahul";

System.out.println(s2.name);
```

Output:

```text
Rahul
```

Because `s1` and `s2` refer to the same object.

---

## 6. Fields of an Object

A class can contain variables called **fields** or **instance variables**.

```java
class Employee {
    String name;
    int salary;
}
```

When objects are created:

```java
Employee e1 = new Employee();
Employee e2 = new Employee();
```

Each object gets its **own copy of instance variables**.

```text
e1 → name = "Amit", salary = 50000

e2 → name = "Raj",  salary = 70000
```

Changing one doesn't change the other:

```java
e1.salary = 60000;
```

`e2.salary` remains `70000`.

---

## 7. Methods

A class can define behavior using methods.

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }
}
```

Create an object:

```java
Calculator c = new Calculator();

int result = c.add(10, 20);

System.out.println(result);
```

Output:

```text
30
```

The object allows you to invoke the class's instance methods.

---

## 8. Constructors

A constructor is used to **initialize an object when it is created**.

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Now:

```java
Student s1 = new Student("Amit", 22);
```

The constructor initializes:

```text
name = Amit
age  = 22
```

### Why use constructors?

Instead of:

```java
Student s = new Student();

s.name = "Amit";
s.age = 22;
```

you can do:

```java
Student s = new Student("Amit", 22);
```

This makes object creation cleaner and can ensure the object starts in a valid state.

---

## 9. The `this` Keyword

Inside a class, `this` refers to the **current object**.

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

There are two different `name`s here:

```java
this.name
```

means the object's field.

```java
name
```

means the constructor parameter.

So:

```java
this.name = name;
```

means:

> Put the parameter `name` into the current object's `name` field.

---

## 10. Default Values

Instance variables automatically receive default values if you don't initialize them.

```java
class Test {
    int x;
    boolean flag;
    String name;
}
```

If you create:

```java
Test t = new Test();
```

the values are:

```text
x     → 0
flag  → false
name  → null
```

Common defaults:

| Type | Default |
|---|---|
| `int` | `0` |
| `long` | `0L` |
| `double` | `0.0` |
| `boolean` | `false` |
| `char` | `\u0000` |
| Reference types | `null` |

**Important:** local variables do **not** get automatic default values.

This will not compile:

```java
void test() {
    int x;
    System.out.println(x); // error
}
```

---

## 11. Object Creation and Memory

Consider:

```java
class Person {
    String name;
    int age;
}

Person p = new Person();
p.name = "Amit";
p.age = 25;
```

Conceptually:

```text
Stack                         Heap

p ─────────────────────────→ Person Object
                              name → "Amit"
                              age  → 25
```

The important interview idea is:

> **The reference variable and the object are different things.**

Java's memory model is more nuanced than simply saying "all objects are on the heap," but for normal interview-level reasoning, objects created with `new` are treated as heap objects.

---

## 12. Multiple Objects

One class can create many objects subject to memory/resources.

```java
class BankAccount {
    String owner;
    double balance;
}
```

Then:

```java
BankAccount a1 = new BankAccount();
BankAccount a2 = new BankAccount();
BankAccount a3 = new BankAccount();
```

Conceptually:

```text
BankAccount class
       |
       ├──→ a1 → Object 1
       |
       ├──→ a2 → Object 2
       |
       └──→ a3 → Object 3
```

Each object has its own instance state.

---

## 13. `null` Reference

A reference can point to nothing:

```java
Student s = null;
```

There is **no Student object** being referenced.

If you do:

```java
s.name = "Amit";
```

you get:

```text
NullPointerException
```

because `s` doesn't refer to an object.

---

## 14. Two References Can Point to One Object

This is a common interview question.

```java
Student s1 = new Student();
Student s2 = s1;
```

There is only **one object**.

```text
        ┌───────────────┐
s1 ────→│               │
        │ Student       │
s2 ────→│               │
        └───────────────┘
```

Therefore:

```java
s1.name = "Amit";

System.out.println(s2.name);
```

prints:

```text
Amit
```

---

## 15. Objects Can Become Garbage

Consider:

```java
Student s1 = new Student();

s1 = null;
```

The object previously referenced by `s1` may now become **eligible for garbage collection**, assuming no other reachable reference points to it.

```text
Before:

s1 ─────→ Student Object


After:

s1 → null

Student Object
     ↑
     No reference
```

Java's **Garbage Collector (GC)** eventually reclaims memory from unreachable objects.

---

## 16. Static Members

Not everything belongs separately to every object.

```java
class Student {

    String name;
    static String college = "ABC";
}
```

Each object has its own:

```text
name
```

but `college` is associated with the **class**, not separately with each object.

```text
Student
  |
  └── static college = "ABC"

s1 → name = Amit
s2 → name = Raj
s3 → name = Rahul
```

All can access:

```java
Student.college
```

---

## 17. Complete Example

```java
class Employee {

    private String name;
    private int salary;

    Employee(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }

    void display() {
        System.out.println(name + " earns " + salary);
    }
}

public class Main {

    public static void main(String[] args) {

        Employee e1 = new Employee("Amit", 50000);
        Employee e2 = new Employee("Raj", 70000);

        e1.display();
        e2.display();
    }
}
```

Output:

```text
Amit earns 50000
Raj earns 70000
```

---

# ⭐ FAANG Interview Points

You should be able to answer these immediately:

### Q1. What is a class?

A **blueprint/template** defining the properties and behavior of objects.

### Q2. What is an object?

An **instance of a class**.

### Q3. What does `new` do?

It creates an object and returns a reference to that object.

### Q4. What is the difference between an object and a reference?

The **object is the actual instance**, while the **reference variable holds a reference to that object**.

### Q5. Can multiple references point to the same object?

Yes.

```java
A a = new A();
A b = a;
```

### Q6. Does every object have its own instance variables?

Yes. Each object has its own instance state.

### Q7. What does `this` mean?

It refers to the **current object**.

### Q8. What happens when an object becomes unreachable?

It becomes **eligible for garbage collection**.

---

# 🧠 Mental Model

The most important thing to internalize is:

```text
                 CLASS
                   │
            blueprint/template
                   │
          ┌────────┴────────┐
          ↓                 ↓
       OBJECT 1          OBJECT 2
          ↑                 ↑
       reference         reference
```

Remember:

> **Class = blueprint**
>
> **Object = instance**
>
> **Reference = variable that refers to an object**
