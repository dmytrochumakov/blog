+++
title = 'DSA - Binary Search Tree - Inorder'
date = 2024-09-28T07:08:52+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "BST", "Traverse", "Inorder", "Swift"]
draft = false
+++

### What is the BST inorder algorithm?
The inorder algorithm returns values in ascending order (sorted from smallest to the largest value).

### Code example
```swift
final class BSTNode<Value: Comparable> {

    var val: Value?
    var left: BSTNode?
    var right: BSTNode?

    init(val: Value? = nil) {
        self.val = val
    }

    func inorder(_ visited: inout [Value]) -> [Value] {
        if self.left != nil {
            self.left!.inorder(&visited)
        }

        if self.val != nil {
            visited.append(self.val!)
        }

        if self.right != nil {
            self.right!.inorder(&visited)
        }

        return visited
    }
}
```

#### Implementation
The inorder algorithm:
1. Recursively traverse the `left` subtree.
2. Visit the current node.
3. Recursively traverse the `right` subtree.

### Time/Space complexity
The time complexity of the inorder algorithm is O(N), where N is the total number of nodes.
The space complexity:
* O(1) if recursion stack space is not considered.
* Otherwise, O(H), where H is the height of the tree.
* In the worst case, H can be the same as N (when the tree is skewed).
* In the best case, H can be the same as logN (when the tree is complete).

#### Thank you for reading! 😊
