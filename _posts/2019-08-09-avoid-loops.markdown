---
layout: post
title: "Avoid loops in high level programs"
date:  2019-08-09 12:21:12 +0300
categories: [procedural programming, declarative programming]
---
Don't use loops!

Why are we against loops in our code?
Loop is a basic construction in procedural programming but there is no place for it in our object-oriented or functional programs. They come from assembly:

```assembly
; eax -> n
; n!  -> eax
FACTORIAL:
    PUSH %ecx
    MOV  %eax, %ecx
    MOV  $1,   %eax
    LOOP:
        TEST %eax, %eax
        JZ   RETURN
        MUL  %ecx
        DEC  %ebx
        JMP  LOOP
    RETURN:
    POP %ecx
    RET
```

But in high level PL we want to declare:

```
0! = 1
n! = (n - 1)! * n
```

OR

```java
public class Factorial extends Natural {
    public Factorial(Natural number) {
        if (number.isZero()) {
            this.value = 1;
        } else {
            this.value =
                new Factorial(number.minus(1))
                    .times(number)
                    .value;
        }
    }

    public static void main(String[] args) {
        Natural five = Natural.of(5);
        Natural factorialOf5 = new Factorial(five);
        factorialOf5.print(); // prints 120
    }
}
```

Also loops are often used to iterate over array or list:

```java
for (int i = 0; i < array.length; i++) {
    process(array[i]);
}
```

It is wrong! Since if we program lists this way, each walk contains code duplication (`for (int i = 0; i < array.length; i++`).
What if we decide to implement lists in other way?
What if we want indexing to start with 1?
What if we want to get rid of random access structures?

Such walks should be hidden from us like this:

```ruby
result = list.map { |x| process(x) }
```

OR

```haskell
processed = map process list
```

Function `map` create abstraction between the way list is being accessed and what we want to do with it.
We don't want to see procedural assembly like loops in our code.

In a pinch use `forEach` method or something like that.

But what if we have to repeat something?

There is a pure mathematical way to create iteration called recursion!

```kotlin
fun multiply(x: Int, y: Int): Int = when {
    x == 0 -> 0
    x == 1 -> y
    else   -> y + multiply(x -  1, y)
}
```
Recursion is much better than loop! Because
1. It is abstract and declarative.
2. It cannot iterate endlessly and it fails if program is wrong.
3. It makes our code cleaner (because we can use variables only within function scope).

But! Remember Bob Martin's advice (from "Clean Code") how to write code, steps:
1. Make it work
2. Refactor
3. Optimize

Optimize recursion when function is 100% ready!
We can use tail recursion to make it super fast (as fast as loops in procedural programmnig).

```kotlin
fun multiply(x: Int, y: Int): Int {
    tailrec fun iter(acc: Int, num: Int): Int =
        if (num == 0) acc
        else iter(acc + x, num - 1)
    return iter(0, y)
}
```
