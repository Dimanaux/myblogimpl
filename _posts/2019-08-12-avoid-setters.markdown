---
layout: post
title: "Avoid using setters"
date:  2019-08-12 20:41:41 +0300
categories: oop
---
Avoid using setters in OOP code.

What's wrong with them?
They allow us to have inconsistent objects in our program.
Imagine we have class:

```java
public class Car {
    private Engine engine;
    private String state;
    private int gasLeft;

    public Car() {}

    public void drive() {
        // uses its state
    }
    public void setEngine(Engine engine) { this.engine = engine; }
    public void setState(String state) { this.state = state; }
    public void setGasLeft(int gasLeft) { this.gasLeft = gasLeft; }
} // WRONG!!!
```

It is a very bad class. It exposes its state outside.
We can easily destroy object of this type like this:

```java
car.setGasLeft(-3);
car.drive(); // what happens?
```

OR

```java
new Thread(car::drive).start();
new Thread(() -> car.setEngine(new Engine())).start();
car.drive();
// fail now
```

In fact, we don't have to do anything to make this object invalid!

```java
Car car = new Car();
car.drive(); // NullPointerException
```

`Car`'s objects violate encapsulation, one of the fundamentals of OOP!

Instead of setters, we must use constructors!

```java
public class Car {
    private int gasLeft;
    private String state;
    private Engine engine;

    public Car(Engine engine, String state, int gasLeft) {
        this.engine = engine;
        this.state = state;
        this.gasLeft = gasLeft;
    }

    public void drive() {
        // uses its state
    }
} // OK
```

Now it is impossible to catch car in incosistent state after instantiation.

```java
Car car = new Car(new Engine(), "HANDBREAKED", 10);
car.drive(); // OK
```

Also we cannot throw old engine away on the go.

```java
new Thread(car::drive).start();
new Thread(() -> {
    /* We can't switch engine while driving */
}).start();
car.drive(); // OK
```
