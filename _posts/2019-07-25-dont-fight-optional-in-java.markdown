---
layout: post
title: "Don't fight Optional in Java"
date:  2019-07-25 11:38:11 +0300
categories: [java, functional]
---

Many programmers don't know much about Optional in Java and they even don't want to understand it and use it in their purposes.
```java
public User authenticate(HttpServletRequest req) {
    String username = req.getParameter("username");
    String password = securityService.hashPassword(
            req.getParameter("password")
    );

    User user = userDao.getByUsername(username);
    if (user != null && user.getPassword().equals(password)) {
        return user;
    } else {
        return null;
    }
}
```
Look at my code I wrote when I didn't know that userDao can return Optional<User> instead of just User instance.
You should not avoid using Optional, you can interact with it like it is almost a User instance.
```java
public Optional<Account> authenticate(String username, String password) {
    Optional<Account> account = accountRepo.findByUsername(username);
    return account.filter(u -> encoder.matches(password, u.getPassword()));
}
```
`Optional<User>` is not some odd object that makes you call `.get()` method and handle exceptions or check if it is null or something like this.
It is your OBJECT ITSELF, but it may not present. You can invoke all its methods!!! like this:
```java
Optional<Account> a = accountRepository.getFirst();
a.map(Account::email) // Optional<String>
        .map(messageService::sendNotificationEmail) // sends email if user was found
        // Optional<EmailMessage>
        .orElseThrow(AccountNotFoundException::new); // notifies us that there were no user found
// OR
map.put("fullName", a.map(Account::getFullName).orElse("User not found"));
```
