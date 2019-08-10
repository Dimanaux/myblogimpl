---
layout: post
title: "Methods' bodies should be five lines long (or even less)"
date:  2019-07-28 20:43:32 +0300
categories: [code, design]
---
Think of method's size.
How can large methods be convenient? They can't.
If we write 5 lines of code in method we can clearly understand what it does.
It is easy to debug.
I have met 300-lined methods, invoking other huge methods and so on.
It is very hard to understand what is happening there.
They check something modyfiyng objects' states at the same time.
They create a big mess.
Also classes should be very small and contain a few methods.

And the best method is one-lined method. It looks like:
```java
class UserWithVacation {
    private final User user;

    UserWithVacation(User user) {
        this.user = user;
    }

    public Hours vacationBalance() {
        return Hours.of(user.vacatoinDaysLeft().size())
                    .times(user.workHoursPerDay());
    }
}
```
Here is a sign of bad method that does 2 things or more: we insert empty line in it.
What is 2 things? Sounds abstract. It does.
Method (function or procedure) should do only one thing - wrap less abstract procedures invocations.

For instance, what is `person.addFriend(Person other)`? It is `if (other.subscribedTo(this)) { this.subscribeTo(other); }` (checks mutual subscription and subscribes one person to the other.

What is `person.subscribedTo(Person other)`? It is `person.subscriptions.contains(other)`. What is `subscribeTo`? It is `person.subscriptions.add(other)` then `person.save()`.

Abstract methods call a bit less abstract methods and they call even less abstract methods and so on.
Very abstract methods (your application itself, business logic) should not contain low-level methods calls. It makes our software inflexible.
