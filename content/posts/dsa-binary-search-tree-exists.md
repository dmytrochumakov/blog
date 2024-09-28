+++
title = 'DSA - Binary Search Tree - Exists'
date = 2024-09-28T06:56:08+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "BST", "Exists", "Swift"]
draft = false
+++

### Code Example
```swift
final class BSTNode<Value: Comparable> {

    var val: Value?
    var left: BSTNode?
    var right: BSTNode?

    init(val: Value? = nil) {
        self.val = val
    }

    func exists(_ val: Value) -> Bool {
        // 1
        guard let selfVal = self.val else { return false }
        // 2
        if self.val == val {
            return true
        }
        // 3
        if val < self.val! {
            if self.left == nil {
                return false
            }
            return self.left!.exists(val)
        }
        // 4
        if self.right == nil {
            return false
        }
        // 5
        return self.right!.exists(val)
    }
}
```

#### Implementation
The `exists` algorithm:
1. Check if `self.val` exists. If it does not exist, return `false`. If it does, move to the next step.
2. Compare `self.val` with the input value. If the values are equal, return `true`. If they are not equal, move to the next step.
3. If the input `val` is less than `self.val`, and the `left` node exists, return a recursive call of the `exists` method on the `left` node; otherwise, return `false`. If the input `val` is greater than `self.val`, move to the next step.
4. Check if the `right` node exists. If it does not exist, return `false`. If it does exist, move to the next step.
5. Return a recursive call of the `exists` method on the `right` node.

### Time/Space Complexity
- **Time complexity:** The time complexity of the `exists` algorithm is O(N), where N is the total number of nodes.
- **Space complexity:**
  - O(1) if recursion stack space is not considered.
  - Otherwise, O(H), where H is the height of the tree.
  - In the worst case, H can be the same as N (when the tree is skewed).
  - In the best case, H can be the same as log N (when the tree is complete).

#### Thank you for reading! 😊
