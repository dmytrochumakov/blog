+++
title = 'DSA - Binary Search Tree - Delete'
date = 2024-09-24T06:53:52+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "BST", "Delete", "Swift"]
draft = false
+++

### Introduction 
The [delete](https://en.wikipedia.org/wiki/Binary_search_tree#Deletion) algorithm is perhaps one of the hardest parts of managing a binary search tree (BST).

### Code Example

```swift
final class BSTNode {

    var val: Int?
    var left: BSTNode?
    var right: BSTNode?

    init(val: Int? = nil) {
        self.val = val
    }

    func delete(_ val: Int) -> BSTNode? {
        // 1
        guard let selfVal = self.val else {
            return nil
        }
        // 2
        if val < selfVal {
            if self.left == nil {
                return self
            }
            self.left = self.left!.delete(val)
        }
        // 3
        if val > selfVal {
            if self.right == nil {
                return self
            }
            self.right = self.right!.delete(val)
        }
        // 4
        if self.right == nil {
            return self.left
        }
        // 5
        if self.left == nil {
            return self.right
        }

        // 6
        var minLargerNode = self.right
        while minLargerNode?.left != nil {
            minLargerNode = minLargerNode?.left
        }

        guard let minLargerNode = minLargerNode else {
            return self
        }
        self.val = minLargerNode.val
        self.right = self.right?.delete(minLargerNode.val!)

        return self
    }

}
```

#### Implementation

Let’s look at the details:

1. The first step is to check for the base scenario: whether the BST has a `self.val`. If it does not, return `nil`. If it has a value, move to the second step.
2. The second step is to check if the delete value is less than `self.val`. If it is, check if the left node exists. If it does not exist, return the `self` node. If it does exist, call `delete` on the left node and assign the new result to the `left` node.
3. The third step is the opposite of the second step but for the right node.
4. The fourth step is to check if the `right` node is `nil`. If it is, return the `left` node.
5. The fifth step is the opposite of the fourth but for the `left` and `right` nodes.
6. The sixth step is to find the `minLargerNode`, update `self.val` with `minLargerNode.val`, and delete `minLargerNode.val` from the `right` node, assigning the new result to it.

### Time Complexity 
The time complexity of the BST deletion algorithm is O(h), where `h` is the height of the tree. 

In the case where the BST becomes skewed, the height of the BST `h` becomes O(n), making the time complexity O(n).

In the case where the BST is balanced, the height of the BST `h` becomes O(log n), making the time complexity O(log n).

#### Thank you for reading! 😊
