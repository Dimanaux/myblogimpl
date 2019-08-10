---
layout: post
title: "We don't know OOP"
date:  2019-07-30 12:12:12 +0300
categories: [oop, procedural programming]
---
What is Object-oriented programming?
It is a paradigm where we use objects.
Objects are like small machines with its inner state (position of all of its gears) and functions (buttons and livers).
We can't intervene in it and move the gears, because we can easily break it.

Also rotating a gear in our machine doesn't make any sense. We won't know what will happen.
But we can interact with the machine using its buttons. All the buttons together form an interface.
Also with interface we abstract its internal organization and we don't need to be puzzled with question "Which gear do we need to rotate to make the machine print our document?"

Java is positioned as an object-oriented programming language.
Now look at typical Java web application.
```
application
├─── controller
├─── model
│    └ User.java
├─── repository
└─── service
     └ UserService.java
```
Entity.java (access modifiers are omitted).
```java
class User {
    String name;
    int age;

    User() {}
    String getName() {
        return name;
    }
    int getAge() {
        return age;
    }
    void setName(String name) {
        this.name = name;
    }
    void setAge(int age) {
        this.age = age;
    }
}
```
Huh! Looks like a structure in procedural programming language C.

UserService.java
```java
class UserService {
    UserRepository userRepository;

    boolean isAdult(User user) {
        return user.getAge() >= 18;
    }
    String firstName(User user) {
        return user.getName().split(" ")[0];
    }
    void updateName(User user, String name) {
        user.setName(name);
        userRepository.persist(user);
    }
    void updateAge(User user, int age) {
        if (age < 0 || age > 256) {
            throw new AgeException("Invalid age: " + age);
        }
        user.setAge(age);
        userRepository.persist(user);
    }
}
```

What are gears, buttons for User? For UserService? Nothing. User creates no abstraction.

We separeted User's functionality. Now we have only data structures and some functions for it.

We still can break User like this: `user.setAge(-1); userRepository.persist(user);`

It is not an object-oriented program.

```c
struct User {
   char* name;
   int age;
};

int isAdult(struct User* user) {
    return *(user->age) >= 18;
}
char* firstName(struct User* user) {
    char* lastName = strchr(user->name, ' ');
    int firstNameLength = lastName - user->name;
    char* firstName = (char*)malloc((firstNameLength + 1) * sizeof(char));
    strncpy(firstName, user->name, firstNameLength);
    return firstName;
}
void updateName(struct User* user, char* name) {
    free(user->name);
    user->name = name;
    userRepository->persist(user);
}
int updateAge(struct User* user, int age) {
    if (age < 0 || age > 256) {
        return -1;
    }
    user->age = age;
    userRepository->persist(user);
    return 0;
}
```
Is it an object-oriented program written in C? No, it is procedural program written in Java. In the majority of our projects we create UserService and put tons of long methods there. UserService has no state, it is just a namespace for our procedures. User has no behaviour, it is data structure (or record, or associative array). They are not objects.
