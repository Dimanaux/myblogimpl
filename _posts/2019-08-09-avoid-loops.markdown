---
layout: post
title: "Avoid loops in high level programs"
date:  2019-08-09 12:21:12 +0300
categories: procedural declarative programming
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

Also lists is often used to iterate over array or list:
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

In a pinch use `forEach` method or something like that.