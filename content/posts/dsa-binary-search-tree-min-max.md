+++
title = 'DSA - Binary Search Tree Min/Max'
date = 2024-09-22T07:26:23+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "BST", "Min", "Max", "Swift"]
draft = false
+++

### Introduction
Finding the `min` and `max` elements is one of the simplest algorithms regarding `BST` (Binary Search Tree). The `findMin` method loops through the `left` child nodes and returns the value from the last node. The `findMax` method does the same but traverses the `right` child nodes.

#### Code Example - Min
```swift
func findMin() -> Int? {
    var min: Int?
    var curr: BSTNode? = self

    while curr != nil {
        min = curr?.val
        curr = curr?.left
    }

    return min
}
```

#### Code Example - Max
```swift
func findMax() -> Int? {
    var max: Int?
    var curr: BSTNode? = self

    while curr != nil {
        max = curr?.val
        curr = curr?.right
    }

    return max
}
```

#### Complete Code Example
```swift
final class BSTNode {

    var val: Int?
    var left: BSTNode?
    var right: BSTNode?

    init(val: Int? = nil) {
        self.val = val
    }

    func insert(_ val: Value) {
        if self.val == nil {
            self.val = val
            return
        }

        if self.val == val {
            return
        }

        if val < self.val! {
            if self.left != nil {
                self.left!.insert(val)
                return
            } else {
                self.left = BSTNode(val: val)
                return
            }
        }

        if self.right != nil {
            self.right!.insert(val)
            return
        }

        self.right = BSTNode(val: val)
    }

    func findMin() -> Int? {
        var min: Int?
        var curr: BSTNode? = self

        while curr != nil {
            min = curr?.val
            curr = curr?.left
        }

        return min
    }

    func findMax() -> Int? {
        var max: Int?
        var curr: BSTNode? = self

        while curr != nil {
            max = curr?.val
            curr = curr?.right
        }

        return max
    }

}
```

### Time/Space Complexity
For both `findMin` and `findMax`:  
- Time Complexity is O(h), where `h` is the height of the tree.  
- Space Complexity is O(1).


#### Thank you for reading! 😊
