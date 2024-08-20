+++
title = 'DSA - Stack'
date = 2024-08-20T07:17:01+03:00
tags = ["Data Structures", "Algorithms", "DSA", "Stack", "Swift"]
draft = false
+++

### What is a Stack?
A [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) is an abstract data type that serves as a collection of elements and implements operations like `push`, `pop`, and `peek` at the end in O(1) time. It uses the LIFO (**last in**, **first out**) order. For example, a stack can be a collection of items where adding or removing is practical at the top.

#### Code Example
```swift
struct Stack<Element> {

    private var array: [Element]

    init(array: [Element] = []) {
        self.array = array
    }

    mutating func push(_ element: Element) {
        array.append(element)
    }

    mutating func pop() -> Element? {
        if array.isEmpty {
            return nil
        }
        return array.popLast()
    }

    func peek() -> Element? {
        if array.isEmpty {
            return nil
        }
        return array.last
    }
}
```

### Practical Applications of Stacks
You can observe stack-like behavior in many places, such as redo-undo features in text editors, Photoshop, and the forward and backward navigation features in web browsers.

#### Thank you for reading! 😊
