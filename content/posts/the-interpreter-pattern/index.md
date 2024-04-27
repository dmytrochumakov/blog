+++
title = 'The Interpreter Pattern'
date = 2024-03-18T08:29:30+03:00
tags = ["interpreter pattern", "design patterns", "swift", "oop"]
draft = false
+++

### What is an Interpreter Pattern?

> The Interpreter Pattern helps implement a simple language and defines a class based representation for its grammar along with an interpreter to interpret its sentences.

![alt image](images/0.jpg#center)
[Source](https://www.cs.unc.edu/~stotts/GOF/hires/pat5cfso.htm)

### What problems does it solve?
The Interpreter Pattern helps solve following problems:
- **Language Interpretation**: When you have a language or syntax that needs to be interpreted, such as mathematical expressions, regular expressions, or domain-specific languages (DSLs), the Interpreter Pattern helps in implementing the logic to interpret and execute these expressions.
- **Extensibility**: The Interpreter Pattern allows for easy addition of new grammar rules or language constructs without modifying the core interpreter logic. This promotes extensibility, enabling the interpreter to support new features or languages with minimal changes.
- **Separation of Concerns**: It separates the grammar definition from the interpretation logic. This separation of concerns makes the codebase modular and easier to maintain. Changes to the grammar or language rules do not affect the interpretation logic, and vice versa.

### Real-world code example
``` swift 
// Define the protocol for the expression
protocol Expression {
    func interpret() -> Int
}

// Concrete expression for a number
class NumberExpression: Expression {
    private var value: Int
    
    init(_ value: Int) {
        self.value = value
    }
    
    func interpret() -> Int {
        return value
    }
}

// Concrete expression for addition
class AdditionExpression: Expression {
    private var left: Expression
    private var right: Expression
    
    init(_ left: Expression, _ right: Expression) {
        self.left = left
        self.right = right
    }
    
    func interpret() -> Int {
        return left.interpret() + right.interpret()
    }
}

// Concrete expression for subtraction
class SubtractionExpression: Expression {
    private var left: Expression
    private var right: Expression
    
    init(_ left: Expression, _ right: Expression) {
        self.left = left
        self.right = right
    }
    
    func interpret() -> Int {
        return left.interpret() - right.interpret()
    }
}

// Concrete expression for multiplication
class MultiplicationExpression: Expression {
    private var left: Expression
    private var right: Expression
    
    init(_ left: Expression, _ right: Expression) {
        self.left = left
        self.right = right
    }
    
    func interpret() -> Int {
        return left.interpret() * right.interpret()
    }
}

// Concrete expression for division
class DivisionExpression: Expression {
    private var left: Expression
    private var right: Expression
    
    init(_ left: Expression, _ right: Expression) {
        self.left = left
        self.right = right
    }
    
    func interpret() -> Int {
        let divisor = right.interpret()
        if divisor != 0 {
            return left.interpret() / divisor
        } else {
            // Handle division by zero error
            fatalError("Division by zero")
        }
    }
}

// Usage
let expression = AdditionExpression(
    MultiplicationExpression(NumberExpression(2), NumberExpression(3)),
    DivisionExpression(NumberExpression(10), NumberExpression(5))
)

// Interpret the expression
let result = expression.interpret()
print("Result: \(result)") 
```

#### Thank you for reading! 😊
