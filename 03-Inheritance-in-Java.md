# 03. Inheritance in Java ⭐

Inheritance is one of the **four pillars of OOP** and a very common Java interview topic.

Inheritance allows a **child class (subclass)** to acquire properties and behavior from a **parent class (superclass)**.

It represents an **IS-A relationship**.

---

## Table of Contents

1. [What is Inheritance?](#1-what-is-inheritance)
2. [`extends`](#2-extends)
3. [What is Inherited?](#3-what-is-inherited)
4. [Method Overriding](#4-method-overriding)
5. [Rules of Method Overriding](#5-rules-of-method-overriding)
6. [`super` Keyword](#6-super-keyword)
7. [`super` for Parent Variables](#7-super-for-parent-variables)
8. [`super` for Parent Methods](#8-super-for-parent-methods)
9. [`super()` for Parent Constructors](#9-super-for-parent-constructors)
10. [Constructor Chaining](#10-constructor-chaining)
11. [Constructor Execution Order](#11-constructor-execution-order)
12. [Types of Inheritance](#12-types-of-inheritance)
13. [Why Java Does Not Support Multiple Inheritance Through Classes](#13-why-java-does-not-support-multiple-inheritance-through-classes)
14. [Multiple Inheritance Through Interfaces](#14-multiple-inheritance-through-interfaces)
15. [Upcasting and Downcasting](#15-upcasting-and-downcasting)
16. [Reference Type vs Object Type](#16-reference-type-vs-object-type)
17. [Inheritance vs Composition](#17-inheritance-vs-composition)
18. [FAANG Interview Questions](#18-faang-interview-questions)
19. [Quick Revision](#19-quick-revision)

---

# 1. What is Inheritance?

Inheritance is a mechanism where one class acquires the properties and behavior of another class.

The class being inherited from is called:

- Parent class
- Superclass
- Base class

The class that inherits is called:

- Child class
- Subclass
- Derived class

### Example

```java
class Animal {

    String name;

    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Dog is barking");
    }
}
```

Relationship:

```text
        Animal
           ↑
           |
          Dog
```

Now:

```java
Dog d = new Dog();

d.eat();   // inherited method
d.bark();  // Dog's own method
```

### IS-A Relationship

Inheritance represents an **IS-A relationship**.

```text
Dog IS-A Animal
Cat IS-A Animal
Manager IS-A Employee
Car IS-A Vehicle
```

---

# 2. `extends`

The `extends` keyword is used to establish inheritance between classes.

```java
class Parent {

    int x = 10;

    void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int y = 20;
}
```

Now:

```java
Child c = new Child();

System.out.println(c.x);  // 10
System.out.println(c.y);  // 20

c.display();              // Parent
```

The child can use members inherited from the parent.

### Important

A Java class can extend only **one class**.

Valid:

```java
class C extends A {
}
```

Invalid:

```java
class C extends A, B {
}
```

Java does not support multiple inheritance through classes.

---

# 3. What is Inherited?

A subclass does not simply get unrestricted access to everything in the parent.

Consider:

```java
class Parent {

    int a = 10;

    private int b = 20;

    protected int c = 30;

    public int d = 40;

    void method1() {
    }

    private void method2() {
    }
}
```

A subclass can directly access:

```text
a       → Yes
b       → No, private
c       → Yes, subject to access rules
d       → Yes
method1 → Yes
method2 → No, private
```

### Private Members

Private members cannot be directly accessed by a subclass.

```java
class Parent {

    private int x = 10;
}

class Child extends Parent {

    void test() {
        // System.out.println(x);  // Compilation error
    }
}
```

A private field can be accessed through a public/protected method:

```java
class Parent {

    private int x = 10;

    public int getX() {
        return x;
    }
}

class Child extends Parent {

    void test() {
        System.out.println(getX());
    }
}
```

### Constructors Are Not Inherited

Constructors belong to the class that declares them.

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}
```

`Child` does not inherit the `Parent()` constructor.

However, when creating a `Child` object, the parent constructor must execute first.

This leads to **constructor chaining**.

---

# 4. Method Overriding

Method overriding occurs when a child class provides its own implementation of a method already defined in the parent class.

```java
class Animal {

    void sound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

Now:

```java
Dog d = new Dog();

d.sound();
```

Output:

```text
Dog barks
```

The child has overridden the parent's implementation.

---

## Why Method Overriding?

Suppose different animals have different sounds.

```java
class Animal {

    void sound() {
        System.out.println("Some sound");
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

This is **runtime polymorphism**.

The reference type is `Animal`, but Java chooses the overridden instance method based on the actual object at runtime.

---

# 5. Rules of Method Overriding

## Rule 1: Same Method Signature

Parent:

```java
void display()
```

Child:

```java
@Override
void display()
```

The method name and parameter list must match.

---

## Rule 2: Return Type

The return type must be the same or **covariant**.

Example:

```java
class Animal {
}

class Dog extends Animal {
}

class Parent {

    Animal getAnimal() {
        return new Animal();
    }
}

class Child extends Parent {

    @Override
    Dog getAnimal() {
        return new Dog();
    }
}
```

This is valid because `Dog` is a subtype of `Animal`.

---

## Rule 3: Access Cannot Be Reduced

Suppose:

```java
class Parent {

    public void display() {
    }
}
```

This is invalid:

```java
class Child extends Parent {

    @Override
    protected void display() {
    }
}
```

Why?

Because `protected` is more restrictive than `public`.

An overriding method cannot reduce the visibility of the parent method.

Valid:

```text
public    → public
protected → protected/public
default   → default/protected/public
```

---

## Rule 4: `final` Methods Cannot Be Overridden

```java
class Parent {

    final void display() {
        System.out.println("Parent");
    }
}
```

This is invalid:

```java
class Child extends Parent {

    @Override
    void display() {
        // Compilation error
    }
}
```

A `final` method cannot be overridden.

---

## Rule 5: Static Methods Are Not Overridden

Static methods are **hidden**, not overridden.

```java
class Parent {

    static void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void display() {
        System.out.println("Child");
    }
}
```

Now:

```java
Parent p = new Child();

p.display();
```

Output:

```text
Parent
```

Static method calls are resolved based on the reference type.

---

## Rule 6: Private Methods Are Not Overridden

A private method is not visible to subclasses.

```java
class Parent {

    private void display() {
        System.out.println("Parent");
    }
}
```

A method with the same name in the child is a new method, not an override.

---

# 6. `super` Keyword

`super` refers to the **immediate parent class**.

There are three major uses:

```text
1. Access parent variable
2. Call parent method
3. Call parent constructor
```

---

# 7. `super` for Parent Variables

Suppose both parent and child have a variable with the same name:

```java
class Parent {

    int x = 10;
}

class Child extends Parent {

    int x = 20;

    void display() {
        System.out.println(x);
        System.out.println(super.x);
    }
}
```

Now:

```java
Child c = new Child();

c.display();
```

Output:

```text
20
10
```

Because:

```java
x
```

refers to the child's variable.

While:

```java
super.x
```

refers to the parent's variable.

---

# 8. `super` for Parent Methods

Suppose the child overrides a method:

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog bark");

        super.sound();
    }
}
```

Now:

```java
Dog d = new Dog();

d.sound();
```

Output:

```text
Dog bark
Animal sound
```

`super.sound()` explicitly calls the parent implementation.

### Why is this useful?

Sometimes you don't want to completely replace the parent's behavior. You want to extend it.

```java
@Override
void display() {

    super.display();

    System.out.println("Additional child behavior");
}
```

---

# 9. `super()` for Parent Constructors

`super()` calls the constructor of the immediate parent.

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {
        super();

        System.out.println("Child constructor");
    }
}
```

Now:

```java
Child c = new Child();
```

Output:

```text
Parent constructor
Child constructor
```

The parent constructor executes before the child constructor body.

---

# 10. Constructor Chaining

Constructor chaining means one constructor calls another constructor.

There are two important forms:

### Same class

Using:

```java
this()
```

### Parent class

Using:

```java
super()
```

---

## Constructor Chaining Using `this()`

```java
class Student {

    Student() {
        this(10);
        System.out.println("Default constructor");
    }

    Student(int age) {
        System.out.println("Age: " + age);
    }
}
```

Now:

```java
Student s = new Student();
```

Execution:

```text
Student()
   ↓
this(10)
   ↓
Student(int)
```

Output:

```text
Age: 10
Default constructor
```

---

## Constructor Chaining Using `super()`

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        super();
        System.out.println("Child");
    }
}
```

Execution:

```text
Child()
   ↓
super()
   ↓
Parent()
```

Output:

```text
Parent
Child
```

---

## Important Constructor Rule

`this()` or `super()` must be the **first statement** in a constructor.

Valid:

```java
Child() {
    super();

    System.out.println("Child");
}
```

Invalid:

```java
Child() {

    System.out.println("Hello");

    super(); // Compilation error
}
```

You also cannot use both in the same constructor:

```java
Child() {
    this();
    super(); // Compilation error
}
```

---

# 11. Constructor Execution Order

Consider:

```java
class A {

    A() {
        System.out.println("A");
    }
}

class B extends A {

    B() {
        System.out.println("B");
    }
}

class C extends B {

    C() {
        System.out.println("C");
    }
}
```

Now:

```java
C obj = new C();
```

The constructors execute:

```text
A
B
C
```

Conceptually:

```text
new C()
   ↓
C constructor
   ↓
B constructor
   ↓
A constructor
```

But actual constructor execution occurs from the topmost superclass down:

```text
A → B → C
```

### Key Interview Rule

> When creating a child object, superclass constructors execute before subclass constructors.

---

## What If `super()` Is Not Written?

Java automatically inserts:

```java
super();
```

if possible.

For example:

```java
class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}
```

is conceptually:

```java
class Child extends Parent {

    Child() {
        super();

        System.out.println("Child");
    }
}
```

---

## Parent Without a No-Argument Constructor

Consider:

```java
class Parent {

    Parent(int x) {
        System.out.println(x);
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}
```

This produces a compilation error.

Why?

Java tries to insert:

```java
super();
```

but `Parent` does not have a no-argument constructor.

It only has:

```java
Parent(int x)
```

### Solution

Explicitly call the parent constructor:

```java
class Child extends Parent {

    Child() {
        super(10);

        System.out.println("Child");
    }
}
```

---

# 12. Types of Inheritance

The commonly discussed types are:

```text
1. Single
2. Multilevel
3. Hierarchical
4. Multiple
5. Hybrid
```

---

## 12.1 Single Inheritance

One parent → one child.

```text
A
↓
B
```

Example:

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

Java supports this.

---

## 12.2 Multilevel Inheritance

Inheritance across multiple levels.

```text
A
↓
B
↓
C
```

Example:

```java
class Animal {
}

class Mammal extends Animal {
}

class Dog extends Mammal {
}
```

Therefore:

```text
Dog IS-A Mammal
Dog IS-A Animal
```

Java supports multilevel inheritance.

---

## 12.3 Hierarchical Inheritance

One parent has multiple children.

```text
        Animal
        /    \
       /      \
     Dog      Cat
```

Example:

```java
class Animal {

    void eat() {
    }
}

class Dog extends Animal {

    void bark() {
    }
}

class Cat extends Animal {

    void meow() {
    }
}
```

Java supports hierarchical inheritance.

---

# 13. Why Java Does Not Support Multiple Inheritance Through Classes

Multiple inheritance means one class has multiple parent classes.

Conceptually:

```text
      A       B
       \     /
        \   /
          C
```

Java does not allow:

```java
class C extends A, B {
}
```

### Why?

The primary problem is ambiguity, commonly illustrated by the **Diamond Problem**.

```text
        A
       / \
      B   C
       \ /
        D
```

Suppose `A` has:

```java
void display()
```

Both `B` and `C` override it:

```text
B → display()
C → display()
```

Now `D` inherits from both.

If we call:

```java
D d = new D();

d.display();
```

Which implementation should execute?

```text
B.display()
```

or:

```text
C.display()
```

Java avoids this ambiguity by not allowing multiple inheritance through classes.

---

# 14. Multiple Inheritance Through Interfaces

Java does allow a class to implement multiple interfaces.

```java
interface A {

    void display();
}

interface B {

    void show();
}

class C implements A, B {

    @Override
    public void display() {
        System.out.println("Display");
    }

    @Override
    public void show() {
        System.out.println("Show");
    }
}
```

Relationship:

```text
       A       B
        \     /
         \   /
           C
```

Java allows:

```java
class C implements A, B
```

This is often described as **multiple inheritance of type through interfaces**.

---

## Interface Default Method Conflict

Java interfaces can contain `default` methods.

```java
interface A {

    default void display() {
        System.out.println("A");
    }
}

interface B {

    default void display() {
        System.out.println("B");
    }
}
```

If:

```java
class C implements A, B {
}
```

Java cannot determine which `display()` to use.

The class must resolve the conflict:

```java
class C implements A, B {

    @Override
    public void display() {
        A.super.display();
    }
}
```

or:

```java
@Override
public void display() {
    B.super.display();
}
```

This is a useful advanced interview question.

---

# 15. Upcasting and Downcasting

## Upcasting

Upcasting means treating a child object as a parent type.

```java
Dog d = new Dog();

Animal a = d;
```

or:

```java
Animal a = new Dog();
```

This is valid because:

```text
Dog IS-A Animal
```

Upcasting is generally safe and is commonly used for polymorphism.

---

## Downcasting

Downcasting means converting a parent reference back to a child reference.

```java
Animal a = new Dog();

Dog d = (Dog) a;
```

Now:

```java
d.bark();
```

works.

But:

```java
Animal a = new Cat();

Dog d = (Dog) a;
```

causes:

```text
ClassCastException
```

because the actual object is a `Cat`.

Use `instanceof` when appropriate:

```java
if (a instanceof Dog) {
    Dog d = (Dog) a;
    d.bark();
}
```

---

# 16. Reference Type vs Object Type

This is one of the most important inheritance concepts for interviews.

Consider:

```java
Animal a = new Dog();
```

There are two types:

```text
Reference type → Animal
Object type    → Dog
```

### Reference type determines what the compiler allows

```java
a.bark();
```

will not compile if `bark()` exists only in `Dog`.

Why?

The reference is:

```java
Animal a
```

and `Animal` doesn't define `bark()`.

### Runtime object determines overridden method execution

If:

```java
a.sound();
```

and `Dog` overrides `sound()`, then:

```text
Dog.sound()
```

will execute.

Therefore:

```text
Reference type
    ↓
Determines accessible members at compile time

Runtime object
    ↓
Determines overridden instance method at runtime
```

---

## Important Example

```java
class Animal {

    int x = 10;

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    int x = 20;

    @Override
    void sound() {
        System.out.println("Dog");
    }
}
```

Now:

```java
Animal obj = new Dog();

System.out.println(obj.x);
obj.sound();
```

Output:

```text
10
Dog
```

Why?

### Variable

```java
obj.x
```

uses the reference type:

```text
Animal
```

so:

```text
Animal.x = 10
```

### Method

```java
obj.sound()
```

is an overridden instance method, so runtime dispatch chooses:

```text
Dog.sound()
```

This distinction is a **high-value interview concept**.

---

# 17. Inheritance vs Composition

FAANG interviews often discuss whether inheritance is actually the best design choice.

Inheritance represents:

```text
IS-A
```

Composition represents:

```text
HAS-A
```

Example:

```text
Dog IS-A Animal
```

but:

```text
Car HAS-A Engine
```

### Composition Example

```java
class Engine {

    void start() {
        System.out.println("Engine started");
    }
}

class Car {

    private Engine engine = new Engine();

    void startCar() {
        engine.start();
    }
}
```

Here:

```text
Car HAS-A Engine
```

This is composition.

### Important Design Principle

> **Favor composition over inheritance** when inheritance does not represent a strong, stable IS-A relationship.

Inheritance creates stronger coupling between parent and child.

Composition often gives greater flexibility.

---

# 18. FAANG Interview Questions

After studying inheritance, you should be able to answer these.

## Basic

1. What is inheritance?
2. Why do we use inheritance?
3. What does `extends` do?
4. What is an IS-A relationship?
5. Are constructors inherited?
6. Can a subclass access private members directly?

## Method Overriding

7. What is method overriding?
8. Difference between overriding and overloading?
9. Can static methods be overridden?
10. Can final methods be overridden?
11. Can private methods be overridden?
12. What are the rules for overriding?
13. What is covariant return type?

## `super`

14. What is `super`?
15. Difference between `this` and `super`?
16. How do you call a parent's method?
17. How do you call a parent's constructor?
18. Can `super()` appear anywhere in a constructor?

## Constructors

19. What is constructor chaining?
20. What happens if you don't explicitly write `super()`?
21. What happens if the parent doesn't have a no-argument constructor?
22. What is constructor execution order in multilevel inheritance?

## Polymorphism

23. What is runtime polymorphism?
24. Explain:

```java
Animal a = new Dog();
```

25. Why does:

```java
a.sound();
```

call `Dog.sound()`?

26. Why does:

```java
a.bark();
```

fail to compile?

27. What is the difference between reference type and object type?

## Types of Inheritance

28. What types of inheritance does Java support?
29. Why doesn't Java support multiple inheritance through classes?
30. What is the Diamond Problem?
31. How do interfaces allow multiple inheritance?
32. What happens when two interfaces have the same default method?

---

# 19. Quick Revision

## Inheritance

```text
Inheritance
    ↓
Child acquires behavior/state from Parent
    ↓
IS-A relationship
```

---

## `extends`

```java
class Dog extends Animal {
}
```

One class can extend only one class.

---

## Method Overriding

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Overriding enables **runtime polymorphism**.

---

## `super`

```java
super.x;          // Parent variable
super.display();  // Parent method
super();          // Parent constructor
```

`super` refers to the immediate parent.

---

## Constructor Chaining

```text
Child constructor
       ↓
   super()
       ↓
Parent constructor
```

`super()` must be the first statement if explicitly written.

---

## Types of Inheritance

```text
Single          → Supported
Multilevel      → Supported
Hierarchical    → Supported
Multiple        → Not through classes
Multiple        → Possible through interfaces
Hybrid          → Not directly through classes
```

---

# ⭐ Most Important Interview Takeaways

If you have limited time, make sure you know these **10 points extremely well**:

1. **Inheritance represents an IS-A relationship.**
2. `extends` is used for class inheritance.
3. **Constructors are not inherited.**
4. Parent constructors execute before child constructors.
5. `super()` calls the parent constructor.
6. `super.method()` calls the parent's implementation.
7. **Method overriding enables runtime polymorphism.**
8. `static`, `private`, and `final` methods have special overriding rules.
9. Java does **not** support multiple inheritance through classes because of ambiguity such as the Diamond Problem.
10. Java supports multiple inheritance of type through **interfaces**.

### Core Mental Model

```text
                    INHERITANCE
                         |
                     extends
                         |
                Parent ← Child
                         |
             +-----------+-----------+
             |                       |
      Method Overriding           super
             |                       |
      Runtime Polymorphism     Parent Access
             |                       |
       Animal a = new Dog()    +----+----+
                               |    |    |
                           variable method constructor
```

### Constructor Mental Model

```text
new Child()
     ↓
super()
     ↓
Parent Constructor
     ↓
Child Constructor
```

### Polymorphism Mental Model

```java
Animal a = new Dog();

a.sound();
```

```text
Reference Type → Animal
Object Type    → Dog

Accessible methods → Animal reference
Overridden method  → Dog implementation
```

This is the core knowledge you should be able to explain confidently in a FAANG Java OOP interview.
