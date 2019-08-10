---
layout: post
title: "Method's statements should be on the same abstraction level"
date:  2019-07-25 11:31:32 +0300
categories: [code, design]
---
If we write method body and we get a huge one that means we build wrong abstraction.
Our procedures used in this body should be on the same abstraction level.
```java
public void sendMessage(Message message, List<User> recipients) {
    for (User u : recipients) {
        u.setMessage(message);
        userRepository.save(u);
        message.addRecipient(u);
        messageRepository.save(message);
    }
    notifyRecipients(recipients);
} // Wrong!
```
Here we can see that you use `notifyRecipients` function in the same method where `for` loop is used, in `for` loop we save our users and message. It is very unreadable since we have to understand something terrible and incomprehensible from this large `for` statement.
Imagine we have this method implementation instead:
```java
public void sendMessage(Message message, List<User> recipients) {
    message.sendAwayTo(recipients);
    notifyRecipients(recipients);
}
```
Now we perceive the meaning of this procedure much more clear, because names of functions tell us what the main idea of our method consists of.
