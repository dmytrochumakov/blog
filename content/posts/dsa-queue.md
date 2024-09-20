+++
title = 'DSA - Queue'
date = 2024-09-20T07:15:17+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Queue", "Linked List Queue", "Swift"]
draft = false
+++

### What is a Queue?
A [Queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) is an abstract data type that serves as an ordered collection of elements.

A simple queue typically has several operations:
- `push(item)` - adds an item to the `tail`
- `pop()` - removes and returns an item from the `head`

These operations make a queue a **FIFO** (First In, First Out) data structure.

### Implementation
There are two ways to implement a queue.

The first and simplest (but less efficient) way is by using an array and basic operations:

```swift
struct Queue {

    private(set) var queue: [Int]

    init(queue: [Int] = []) {
        self.queue = queue
    }

    mutating func push(_ item: Int) {
        self.queue.insert(item, at: 0)
    }

    mutating func pop() -> Int? {
        return self.queue.popLast()
    }

    func peek() -> Int? {
        return self.queue.last
    }

}
```

The second, more efficient way is by using a [linked list](https://open.substack.com/pub/dmytrosblog/p/dsa-linked-list?r=2fepxg&utm_campaign=post&utm_medium=web), which allows `push` and `pop` operations in O(1) time.

```swift
final class Node {

    private(set) var val: Int
    var next: Node?

    init(
        val: Int,
        next: Node? = nil
    ) {
        self.val = val
        self.next = next
    }

}

final class LinkedListQueue {

    private(set) var head: Node?
    private(set) var tail: Node?

    func push(_ item: Int) {
        let newNode = Node(val: item)
        if head == nil {
            self.head = newNode
            self.tail = newNode
            return
        }

        self.tail?.next = newNode
        self.tail = newNode
    }

    func pop() -> Int? {
        if head == nil {
            return nil
        }

        let tmp = self.head
        self.head = tmp?.next

        if self.head == nil {
            self.tail = nil
        }

        return tmp?.val
    }

}
```

#### Time/Space Complexity using a Linked List Queue

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Insert    | O(1)    | O(1)       |
| Delete    | O(1)    | O(1)       |

**Space Complexity**: O(n) for both average and worst case.

#### Thank you for reading! 😊
