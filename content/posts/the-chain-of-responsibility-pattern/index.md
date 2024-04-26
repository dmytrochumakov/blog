+++
title = 'The Chain Of Responsibility Pattern'
date = 2024-03-15T08:29:30+03:00
tags = ["patternt"]
draft = false
+++

### What is a Chain Of Responsibility Pattern?

> The Chain Of Responsibility Pattern helps create a chain of objects to examine requests. Each object in turn examines a request and either handles it or passes onto the next object in the chain.

![alt image](images/0.jpg#center)
[Source](https://reactiveprogramming.io/blog/en/design-patterns/chain-of-responsability)

### What problems does it solve?

The Chain Of Responsibility Pattern (CoR) helps solve following problems:

- **Dynamic Request Handling**: It enables dynamic assignment of responsibilities at runtime. Handlers can be added, removed, or reordered without affecting the client’s code. This flexibility allows for easier maintenance and extension of the system.
- **Decoupling Sender and Receiver**: In traditional systems, a sender often needs to know the exact receiver of a request, leading to tight coupling between them. The CoR pattern decouples senders from receivers by allowing multiple objects to handle a request without the sender knowing the specific handler.

### Real-world code example
``` swift 
// Protocol defining the handler interface
protocol PurchaseHandler {
    var next: PurchaseHandler? { get set }
    func handleRequest(amount: Double)
}

// Concrete handlers
class SmallPurchaseHandler: PurchaseHandler {
    var next: PurchaseHandler?
    let maxAmount: Double = 100.0
    
    func handleRequest(amount: Double) {
        if amount <= maxAmount {
            print("SmallPurchaseHandler: Purchase approved for $\(amount)")
        } else if let nextHandler = next {
            print("SmallPurchaseHandler: Passing request to next handler")
            nextHandler.handleRequest(amount: amount)
        } else {
            print("SmallPurchaseHandler: No handler available, purchase rejected")
        }
    }
}

class MediumPurchaseHandler: PurchaseHandler {
    var next: PurchaseHandler?
    let maxAmount: Double = 500.0
    
    func handleRequest(amount: Double) {
        if amount <= maxAmount {
            print("MediumPurchaseHandler: Purchase approved for $\(amount)")
        } else if let nextHandler = next {
            print("MediumPurchaseHandler: Passing request to next handler")
            nextHandler.handleRequest(amount: amount)
        } else {
            print("MediumPurchaseHandler: No handler available, purchase rejected")
        }
    }
}

class LargePurchaseHandler: PurchaseHandler {
    var next: PurchaseHandler?
    
    func handleRequest(amount: Double) {
        print("LargePurchaseHandler: Purchase approved for $\(amount)")
    }
}

// Usage
func main() {
    let smallHandler = SmallPurchaseHandler()
    let mediumHandler = MediumPurchaseHandler()
    let largeHandler = LargePurchaseHandler()
    
    // Connecting handlers into a chain
    smallHandler.next = mediumHandler
    mediumHandler.next = largeHandler
    
    smallHandler.handleRequest(amount: 50.0)
    smallHandler.handleRequest(amount: 200.0)
    smallHandler.handleRequest(amount: 1000.0) 
}
``` 

#### Thank you for reading! 😊
