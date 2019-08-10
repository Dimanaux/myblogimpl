---
layout: post
title: "Why Kotlin > Java?"
date:  2019-08-10 09:08:26 +0300
categories: [java, kotlin]
---
## 1. Syntax of Kotlin is more pretty and enjoyable than one in Java.

Java inherits prehistoric syntax from C and C++ languages. In Java
we use all constructions we used in C such as `while () {}`, `for (;;) {}`,
`;`, `return`, `void`, `new Object()`, `do {} while ();`, `int func() {}` nullable references!. `return` is unacceptable in high level programming languages since it comes from assembly. Kotlin threw this garbage (not at all) away and desided to look beautiful.
Compare Java's:

```java
public class Program {
    public static void main(String[] args) {
        User u = new User("u1", "Jake", 12);
        System.out.println(s);
    }
}
```

VS Kotlin's

```kotlin
fun main() {
    val u = User(id = "u1", name = "Jake", age = 12)
    // OR val u = User("u1", "Jake", 12)
    println(u)
}
```

## 2. Kotlin is more object-oriented.

Kotlin has no primitives such as `int`, `char` and arrays (`Object[]`).
Instead we have `Int` and `Array<Int>`.
These classes have behaviour!
Also Kotlin does not let us write static methods.

Java:
```java
public class Program {
    public static void process(Object o) {/*...*/}
}
```

Kotlin:
```kotlin
// ??
```

## 3. Kotlin is more functional.

Kotlin has functions!

```kotlin
fun square(x: Int) = x * x

fun main() = println(
    listOf(3, 4, 5)
        .map(::square)
        .reduce(Int::times)
)
```

## 4. We can use any symbols in names.

Unfortunately, we have to quote it with ``` ` ```, it is not recommended and Android does not support it.

```kotlin
fun `even?`(x: Int) = x % 2 == 0
```

## 5. We can extend existing types!

```kotlin
fun Int.isEven() = this % 2 == 0

fun main() = println(
    listOf(1, 2, 3, 4, 5)
        .filter { it.isEven() }
        // OR
        // .filter(Int::isEven)
)
```

## 6. Kotlin suppports tail recursion optimization.

```kotlin
tailrec fun factorial(of: Int, accumulator: Int = 1): Int =
    if (of == 0) accumulator
    else factorial(of - 1, accumulator * of)
```
