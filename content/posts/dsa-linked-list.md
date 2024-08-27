+++
title = 'DSA - Linked List'
date = 2024-08-27T07:13:19+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Linked List", "Swift"]
draft = false
+++

### What is a Linked List?
A [linked list](https://en.wikipedia.org/wiki/Linked_list) is a common data structure that is similar to an array, but its order is based on pointers to the next element in memory instead of using physical placement (indices).

A linked list has two main components:
1. **ListNode** class: This class has a `val` property that represents the value and a `next` property that represents a pointer to the next element in memory.
2. **LinkedList** class: This class stores a collection of `ListNode` elements and provides operations like `add to tail` and `add to head`.
   - The `add to tail` operation takes O(n) time because it needs to iterate through the list to find the last element.
   - The `add to head` operation takes O(1) time because it only needs to set the next pointer of the new node to the current head and update the head with the new node.

### Where can it be used?
A linked list can be used in stacks, queues, and lists.

### Code Example

```swift
final class ListNode {
    let val: Int
    var next: ListNode?

    init(val: Int, next: ListNode? = nil) {
        self.val = val
        self.next = next
    }
}
```

```swift
final class LinkedList {

    private(set) var head: ListNode?

    init(head: ListNode? = nil) {
        self.head = head
    }

    func addToTail(_ newNode: ListNode?) {
        if self.head == nil {
            self.head = newNode
            return
        }

        var node: ListNode? = self.head

        while node?.next != nil {
            node = node!.next
        }

        node?.next = newNode
    }

    func addToHead(_ newNode: ListNode?) {
        newNode?.next = self.head
        self.head = newNode
    }

}
```

#### Thank you for reading! 😊
