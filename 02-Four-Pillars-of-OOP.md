# 04. Four Pillars of OOP

Object-Oriented Programming (OOP) is based on four fundamental principles:

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

These four concepts are extremely important for Java development and are commonly asked in technical interviews.

---

# 1. Encapsulation

## What is Encapsulation?

Encapsulation means **bundling data and the methods that operate on that data into a single unit (class), while controlling access to the data**.

In Java, encapsulation is mainly achieved using:

- `private` fields
- `public` methods
- Getters and setters

## Example

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

Here:

```java
private double balance;
```

prevents outside classes from directly modifying `balance`.

We cannot do:

```java
BankAccount account = new BankAccount();

account.balance = -10000; // Not allowed
```

Instead, we use a method:

```java
account.deposit(500);
```

The class controls how its internal data is modified.

## Why Encapsulation?

Encapsulation helps with:

- Data protection
- Maintaining valid object states
- Controlling how data is modified
- Reducing coupling
- Improving maintainability

### Interview Definition

> Encapsulation is the bundling of data and behavior together while restricting direct access to an object's internal state.

---

# 2. Inheritance

## What is Inheritance?

Inheritance allows one class to **acquire properties and behaviors of another class**.

Java uses the `extends` keyword for class inheritance.

## Example

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}
```

Now `Dog` can use the `eat()` method inherited from `Animal`.

```java
Dog dog = new Dog();

dog.eat();
dog.bark();
```

Output:

```text
Eating
Barking
```

The relationship is:

```text
        Animal
           ↑
           |
          Dog
```

This represents an **IS-A relationship**:

> Dog IS-A Animal.

## Why Inheritance?

Inheritance provides:

- Code reuse
- Hierarchical relationships
- Method overriding
- Runtime polymorphism

## Important Java Interview Point

Java does **not support multiple inheritance using classes**.

This is not allowed:

```java
class C extends A, B { } // Not allowed
```

However, Java supports multiple inheritance of type through interfaces:

```java
interface A {
}

interface B {
}

class C implements A, B {
}
```

---

# 3. Polymorphism

## What is Polymorphism?

The word polymorphism comes from:

- **Poly** → Many
- **Morphism** → Forms

Therefore, polymorphism means:

> **One interface or reference can represent different forms of objects.**

Java has two major types of polymorphism:

1. Compile-time polymorphism
2. Runtime polymorphism

---

## 3.1 Compile-Time Polymorphism

Compile-time polymorphism is achieved through **method overloading**.

### Example

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

Both methods have the same name:

```text
add()
```

but different parameter lists.

```java
Calculator calculator = new Calculator();

calculator.add(10, 20);

calculator.add(10, 20, 30);
```

The compiler determines which method should be called.

Therefore:

> **Method Overloading → Compile-Time Polymorphism**

---

## 3.2 Runtime Polymorphism

Runtime polymorphism is achieved through **method overriding**.

### Example

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Meow");
    }
}
```

Now:

```java
Animal a1 = new Dog();
Animal a2 = new Cat();

a1.sound();
a2.sound();
```

Output:

```text
Bark
Meow
```

Notice:

```java
Animal a1 = new Dog();
```

The reference type is:

```text
Animal
```

but the actual object is:

```text
Dog
```

At runtime, Java determines which overridden method should execute.

Therefore:

> **Method Overriding → Runtime Polymorphism**

This is a very important concept for Java interviews.

---

# 4. Abstraction

## What is Abstraction?

Abstraction means:

> **Hiding unnecessary implementation details and exposing only the essential functionality.**

Consider a car.

When you drive a car, you interact with:

```text
Steering
Accelerator
Brake
Gear
```

You don't need to know all the internal details of the engine.

That is the idea of abstraction.

---

# Abstraction Using Abstract Classes

Java provides the `abstract` keyword.

### Example

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Eating");
    }
}
```

The child class provides the implementation of the abstract method:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

The abstract class specifies **what** the child must implement.

The child class specifies **how** it is implemented.

---

# Abstraction Using Interfaces

Interfaces are another major mechanism for abstraction in Java.

```java
interface Payment {

    void pay();
}
```

Different classes can provide different implementations.

```java
class CreditCardPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Pay using credit card");
    }
}
```

```java
class UPIPayment implements Payment {

    @Override
    public void pay() {
        System.out.println("Pay using UPI");
    }
}
```

The client can use:

```java
Payment payment = new UPIPayment();

payment.pay();
```

The client only needs to know that:

```java
payment.pay();
```

will perform the payment.

The internal implementation is hidden.

---

# Encapsulation vs Abstraction

This is a very common interview question.

| Encapsulation | Abstraction |
|---|---|
| Protects internal data | Hides implementation details |
| Controls access | Shows essential functionality |
| Focuses on how data is accessed | Focuses on what functionality is exposed |
| Achieved using access modifiers, private fields, methods | Achieved using abstract classes and interfaces |

### Example of Encapsulation

```java
private int balance;
```

The data is protected from direct access.

### Example of Abstraction

```java
interface Payment {

    void pay();
}
```

The implementation details of `pay()` are hidden.

---

# Four Pillars Summary

| Pillar | Main Idea | Java Mechanism |
|---|---|---|
| **Encapsulation** | Protect data and control access | `private`, getters, setters |
| **Inheritance** | Reuse and extend behavior | `extends` |
| **Polymorphism** | One interface, many forms | Overloading, Overriding |
| **Abstraction** | Hide implementation details | `abstract`, `interface` |

---

# Real-World Example

Consider a `Car`.

## Encapsulation

Internal engine state is protected.

```text
Engine details → Hidden/Protected
```

## Inheritance

```text
        Vehicle
           ↓
          Car
```

`Car` inherits common behavior from `Vehicle`.

## Polymorphism

```java
Vehicle v1 = new Car();
Vehicle v2 = new Bike();
```

The same `Vehicle` reference can refer to different objects.

## Abstraction

The user interacts with:

```text
start()
accelerate()
brake()
```

without needing to understand the internal implementation.

---

# Interview One-Liner

If the interviewer asks:

**"What are the four pillars of OOP?"**

A strong answer is:

> The four pillars of OOP are **Encapsulation, Inheritance, Polymorphism, and Abstraction**. Encapsulation protects an object's internal state, inheritance enables code reuse through parent-child relationships, polymorphism allows the same interface to behave differently for different objects, and abstraction hides implementation details while exposing essential functionality.

---

# Quick Revision

```text
OOP
│
├── Encapsulation
│   └── Protect data
│
├── Inheritance
│   └── Reuse/extend behavior
│
├── Polymorphism
│   ├── Overloading → Compile Time
│   └── Overriding → Runtime
│
└── Abstraction
    └── Hide implementation details
```

## Easy Way to Remember

```text
Encapsulation → Protect
Inheritance   → Reuse
Polymorphism  → Many Forms
Abstraction   → Hide Complexity
```
