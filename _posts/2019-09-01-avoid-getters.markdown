---
layout: post
title: "Avoid using getters"
date:  2019-09-01 09:58:32 +0300
categories: oop
---
Avoid using getters in OOP code.

What's wrong with them?
Getters violates the main OOP principle - incapsulation. That leads us to code duplication, complex API, inconsistent state.

Take a look at the example:
```java
public class Sale {
    // ...
    public void notifyAboutSaleEnd() {
        sendMessage(
            new SimpleMessage(
                "Sale is to end in 24 hours!",
                Sale.SALE_NOTIFIER_EMAIL,
                Carts.findWithSaleIn(this).map(Cart::getUser),
                "Sale " + this + " is ending at " + this.endDate + ". You have some in your cart..."
            )
        );
    }

    private void sendMessage(Message message) {
        // here we go
        EmailUtils.sendMessage(
            message.getSubject(), // <- we can muddle here
            message.getSender(),  // and swap parameters.
            message.getRecipients(),
            message.getContent()
        );
    }
}
```
Looks simple? No.
Also if we want to send a message with spicific type (e.g. HTML) in future this API become more complicated?
Also we cannot find class responsible for mailing.

The right way is to incapsulate this behaviour in Message.
API looks simple now: `message.send();`

In this case `Message`s don't have to expose their internals, because they used only within the class.
```java
public class HtmlMessage implements Message {
    private final String subject;
    private final String sender;
    private final List<String> recipients;
    private final String content;

    public HtmlMessage(String subject, User sender,
                       List<User> recipients, String content) {
        this.subject = subject;
        this.sender = sender.email;
        this.recipients = recipients.map(s -> s.email);
        this.content = content;
    }

    @Override public void send() {
        // send message using private fields
        // and content-type=text/html
    }
}
```

Now it's very simple:
```java
Message notification = MessageFactory.emailNotification();
notification.send();
```

Now we can easily change our HtmlMessage implementation without breaking other code.
We can rename its fields, add new, change recipients' type from List to Set and so on. 
