+++
title = 'ARC in Swift'
date = 2023-12-17T00:00:00+03:00
tags = ["Swift", "Automatic Reference Count"]
draft = false
+++

### What is ARC?

> Swift uses Automatic Reference Counting (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you don’t need to think about memory management yourself. ARC automatically frees up the memory used by class instances when those instances are no longer needed. - Apple

### ARC In Action
In this example, we assign an instance to the `reference1` property.
- number of references equals 1.

``` swift
class Storage {

    let data: Data

    init(data: Data) {
        self.data = data
        print("\(Self.self) is being initialized")
    }

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}

var reference1: Storage?
var reference2: Storage?
var reference3: Storage?

reference1 = Storage(data: Data())
```

Now we can assign a reference to another two properties, `reference2` and `reference3`.
Whenever you assign a reference, you increase the counter.
- number of references equals 3.

``` swift
reference2 = reference1
reference3 = reference1
```

When you set `reference2` and `reference3` to `nil`, the number of references equals 1.
``` swift
reference2 = nil
reference3 = nil
```

Only when you set `reference1` to `nil` object will be deinitialized.

``` swift
reference1 = nil
```

### Memory leaks

> Memory leaks appear when you have strong references between two instances that point to each other.

In this example, class Employee has optional `department` property, and `class Department` has optional `employee` property.

``` swift
class Department {

    let name: String

    init(name: String) {
        self.name = name
        print("\(Self.self) is being initialized")
    }

    var employee:  Employee?

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}

class Employee {

    let name: String

    init(name: String) {
        self.name = name
        print("\(Self.self) is being initialized")
    }

    var department: Department?

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}

var employee: Employee?
var department: Department?

employee = Employee(name: "John Doe")
department = Department(name: "Research and development")
```

If we try to assign `Department` reference to `employee` property and `Employee` reference to `department` property, it creates a memory leak by strong references that point to each other.

``` swift
employee!.department = department
department!.employee = employee
```

If you try to set `employee` and `department` properties to `nil`, then these two objects can’t be deallocated because of the existing strong reference relationship between both objects.

``` swift
employee = nil
department = nil
```

To avoid this unpleasant situation, we can use `weak`, `unowned` references.

### Weak reference

> If you use a weak keyword before a property, you say that this property should not keep a strong reference. Weak property should always be mutable and optional because ARC set the property to nil after the instance was deallocated.

In this example, `Employee` instance has `department` property with `weak` keyword. It means when we set `employee` property to `nil` ARC sets `department` property to `nil` and deallocates `Department` instance.

``` swift 
class Department  {

    let name: String

    init(name: String) {
        self.name = name
        print("\(Self.self) is being initialized")
    }

    var employee: Employee?

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}

class Employee {

    let name: String

    init(name: String) {
        self.name = name
        print("\(Self.self) is being initialized")
    }

    weak var department: Department?

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}

var employee: Employee?
var department: Department?

employee = Employee(name: "John Doe")
department = Department(name: "Research and development")

employee!.department = department
department!.employee = employee

department = nil
employee = nil
```

### Unowned reference

> Unowned reference can’t be optional, and it should always have value. If you try to access a deallocated property value, you will face a runtime error.

In this example, we have two instances: `User` and `DiscountCard`.
`DiscountCard` has a relationship with the `User` that is marked as `unowned` to avoid a strong reference cycle.

``` swift 
class User {

    let name: String

    var discountCard: DiscountCard?

    init(name: String) {
        self.name = name
    }

    deinit { print("\(Self.self) is being deinitialized") }

}

class DiscountCard {

    let number: UInt64

    unowned let user: User

    init(number: UInt64, user: User) {
        self.number = number
        self.user = user
    }

    deinit { print("\(Self.self) is being deinitialized") }

}

var user: User?
user = User(name: "John Doe")
``` 

When you create `DiscountCard` instance and assign it as reference to `user` property, it no longer holds strong reference.

``` swift
user!.discountCard = DiscountCard(number: 1234_5678_9012_3456, user: user!)
```

After we set `user` property to `nil`, it will deallocate `User` and `DiscountCard` instances.

``` swift
user = nil
```

### Strong Reference Cycles for Closures

> Strong reference cycle for closure can occur if you assign closure to property of instance. In this case, you assign reference to that closure.
The strong reference cycle appears because closures are reference types.

``` swift 
class MemoryStorage {

    let text: String
    let additionalText: String?

    lazy var copy: () -> String = {
        if let additionalText = self.additionalText {
            self.text + "\n" + additionalText
        } else {
            self.text
        }
    }

    init(text: String, additionalText: String? = nil) {
        self.text = text
        self.additionalText = additionalText
    }

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}

var memoryStorage: MemoryStorage? = MemoryStorage(text: "Thank you for registration", additionalText: "John Doe")
print(memoryStorage!.copy())

memoryStorage = nil
```

In this example, closure captures `self.text` property and create strong reference cycle by referencing to `self`.

### Breaking Strong Reference Cycle in Closure

To break strong reference cycle, we need to add capture list with `unowned` keyword to `copy` closure.

``` swift 
class MemoryStorage {

    let text: String
    let additionalText: String?

    lazy var copy: () -> String = { [unowned self] in
        if let additionalText = self.additionalText {
            self.text + "\n" + additionalText
        } else {
            self.text
        }
    }

    init(text: String, additionalText: String? = nil) {
        self.text = text
        self.additionalText = additionalText
    }

    deinit {
        print("\(Self.self) is being deinitialized")
    }
}

var memoryStorage: MemoryStorage? = MemoryStorage(text: "Thank you for registration", additionalText: "John Doe")
print(memoryStorage!.copy())

memoryStorage = nil
```