+++
title = 'The Dependency Inversion Principle'
date = 2024-03-05T08:29:30+03:00
tags = ["dependency inversion principle", "design principles", "swift", "oop"]
draft = false
+++

### What is a Dependency Inversion Principle?

> The Dependency Inversion Principle means that high-level modules should not depend on low-level modules.

![alt image](images/0.jpg#center)
[Source](http://principles-wiki.net/principles:dependency_inversion_principle)

![alt image](images/1.jpg#center)
[Source](http://principles-wiki.net/principles:dependency_inversion_principle)

### What problems does it solve?
The Dependency Inversion Principle (DIP) helps solve:

- Rigidity
- Fragility
- Immobility problems

### Real-world code example
#### Violation of DIP
``` swift 
// High-level module directly depending on low-level modules
class MessageService {
    func sendMessageViaEmail(message: String) {
        let emailSender = EmailSender()
        emailSender.sendMessage(message: message)
    }
    
    func sendMessageViaSMS(message: String) {
        let smsSender = SMSSender()
        smsSender.sendMessage(message: message)
    }
    
    func sendMessageViaPushNotification(message: String) {
        let pushNotificationSender = PushNotificationSender()
        pushNotificationSender.sendMessage(message: message)
    }
}
```

#### Adhering to DIP
``` swift 
// Protocol defining the interface for sending messages
protocol MessageSender {
    func sendMessage(message: String)
}
```
``` swift 
// High-level module depending on abstraction (MessageSender protocol)
class MessageService {
    private let messageSender: MessageSender
    
    init(messageSender: MessageSender) {
        self.messageSender = messageSender
    }
    
    func sendMessage(message: String) {
        messageSender.sendMessage(message: message)
    }
}
```
``` swift 
// Concrete implementations of MessageSender protocol for different channels
class EmailSender: MessageSender {
    func sendMessage(message: String) {
        print("Sending email: \(message)")
    }
}

class SMSSender: MessageSender {
    func sendMessage(message: String) {
        print("Sending SMS: \(message)")
    }
}

class PushNotificationSender: MessageSender {
    func sendMessage(message: String) {
        print("Sending push notification: \(message)")
    }
}
```
``` swift 
// Example usage
let emailSender = EmailSender()
let smsSender = SMSSender()
let pushNotificationSender = PushNotificationSender()

let emailService = MessageService(messageSender: emailSender)
let smsService = MessageService(messageSender: smsSender)
let pushNotificationService = MessageService(messageSender: pushNotificationSender)

emailService.sendMessage(message: "Hello via email")
smsService.sendMessage(message: "Hello via SMS")
pushNotificationService.sendMessage(message: "Hello via push notification")
```

#### Thank you for reading! 😊
