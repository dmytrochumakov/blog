+++
title = 'The State Pattern'
date = 2024-03-10T08:29:30+03:00
tags = ["state pattern", "design patterns", "swift", "oop"]
draft = false
+++

### What is a State Pattern?
> The State Pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

![alt image](images/0.jpg#center)
[Source](https://en.wikipedia.org/wiki/State_pattern#/media/File:State_Design_Pattern_UML_Class_Diagram.svg)

### What problems does it solve?

- **Complex conditional logic**: When an object’s behavior depends on its internal state, it often leads to complex conditional statements. The State pattern simplifies this by encapsulating each state and its behavior in separate classes, making the code more readable and maintainable.
- **State-specific behavior**: Objects often need to change their behavior based on their state. The State pattern allows objects to delegate behavior to state objects, which can vary independently. This promotes better encapsulation and separation of concerns.
- **Adding new states**: When new states need to be added, the State pattern makes it easier to extend the functionality without modifying existing code. New states can be added by creating new state classes and integrating them into the existing context, without changing the context class itself.

### Real-world code example
``` swift 
// Define the VendingMachine protocol
protocol VendingMachineState {
    func insertCoin()
    func dispenseItem()
}

// Define concrete states
class NoCoinState: VendingMachineState {
    private let vendingMachine: VendingMachine
    
    init(vendingMachine: VendingMachine) {
        self.vendingMachine = vendingMachine
    }
    
    func insertCoin() {
        print("Coin inserted")
        // Transition to the HasCoinState
        vendingMachine.changeState(newState: vendingMachine.hasCoinState)
    }
    
    func dispenseItem() {
        print("Please insert a coin first")
    }
}

class HasCoinState: VendingMachineState {
    private let vendingMachine: VendingMachine
    
    init(vendingMachine: VendingMachine) {
        self.vendingMachine = vendingMachine
    }
    
    func insertCoin() {
        print("Coin already inserted")
    }
    
    func dispenseItem() {
        if vendingMachine.inventoryCount > 0 {
            print("Item dispensed")
            vendingMachine.decreaseInventory()
            // Transition to the NoCoinState
            vendingMachine.changeState(newState: vendingMachine.noCoinState)
        } else {
            print("Out of stock")
        }
    }
}

// Define the VendingMachine class
class VendingMachine {
    var inventoryCount: Int = 5
    var currentState: VendingMachineState!
    var noCoinState: VendingMachineState!
    var hasCoinState: VendingMachineState!
    
    init() {
        noCoinState = NoCoinState(vendingMachine: self)
        hasCoinState = HasCoinState(vendingMachine: self)
        currentState = noCoinState
    }
       
    func changeState(newState: VendingMachineState) {
        currentState = newState
    }
       
    func insertCoin() {
        currentState.insertCoin()
    }
    
    func dispenseItem() {
        currentState.dispenseItem()
    }
    
    func decreaseInventory() {
        inventoryCount -= 1
    }
}

// Usage
let vendingMachine = VendingMachine()
vendingMachine.dispenseItem() 
vendingMachine.insertCoin() 
vendingMachine.insertCoin() 
vendingMachine.dispenseItem() 
vendingMachine.dispenseItem() 
```

Thank you for reading!
