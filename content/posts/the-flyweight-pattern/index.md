+++
title = 'The Flyweight Pattern'
date = 2024-03-17T08:29:30+03:00
tags = ["flyweight pattern", "design patterns", "swift", "oop"]
draft = false
+++

### What is a Flyweight Pattern?

> The Flyweight Pattern refers to an object that minimizes memory usage by sharing some of its data with other similar objects.

![alt image](images/0.jpg#center)
[Source](https://upload.wikimedia.org/wikipedia/commons/4/4e/W3sDesign_Flyweight_Design_Pattern_UML.jpg)

### What problems does it solve?

The Flyweight Pattern helps solve following problems:

- **Large Memory Footprint**: When dealing with a large number of objects, especially if these objects share a significant amount of common state, traditional object creation can lead to excessive memory consumption. The Flyweight Pattern reduces memory usage by sharing this common state among multiple objects.
- **Performance Overhead**: Creating and managing a large number of objects can also introduce performance overhead due to memory allocation, deallocation, and initialization. By reusing shared objects and minimizing the creation of new objects, the Flyweight Pattern can improve performance.
- **Object Creation Cost**: Creating new objects can be costly in terms of time and resources, especially if the objects require complex initialization. By reusing existing objects, the Flyweight Pattern reduces the need for creating new objects, thereby reducing object creation costs.

### Real-world code example
``` swift
// Flyweight protocol defining the interface for shapes
protocol Shape {
    func draw(at point: CGPoint)
}

// Concrete flyweight class representing a circle
class Circle: Shape {
    private let radius: CGFloat
    private let fillColor: UIColor

    init(radius: CGFloat, fillColor: UIColor) {
        self.radius = radius
        self.fillColor = fillColor
    }

    func draw(at point: CGPoint) {
        print("Drawing Circle at (\(point.x), \(point.y)) with radius \(radius) and fill color \(fillColor)")
        // Actual drawing logic would go here
    }
}

// Flyweight factory class responsible for creating and managing flyweight objects
class ShapeFactory {
    private var flyweights = [String: Shape]()

    func getCircle(radius: CGFloat, fillColor: UIColor) -> Shape {
        let key = "Circle-\(radius)-\(fillColor)"
        if let existingShape = flyweights[key] {
            return existingShape
        } else {
            let newShape = Circle(radius: radius, fillColor: fillColor)
            flyweights[key] = newShape
            return newShape
        }
    }
}

// Client code
let shapeFactory = ShapeFactory()

// Request for circles with different properties
let circle1 = shapeFactory.getCircle(radius: 10, fillColor: .red)
let circle2 = shapeFactory.getCircle(radius: 10, fillColor: .red) // Reusing the same circle object
let circle3 = shapeFactory.getCircle(radius: 20, fillColor: .blue)

// Drawing circles
circle1.draw(at: CGPoint(x: 100, y: 100))
circle2.draw(at: CGPoint(x: 200, y: 200))
circle3.draw(at: CGPoint(x: 300, y: 300))
```

#### Thank you for reading! 😊
