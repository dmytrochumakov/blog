+++
title = 'DSA - Binary Search Tree - Postorder'
date = 2024-09-27T06:55:55+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "BST", "Traverse", "Postorder", "Swift"]
draft = false
+++

### What is the postorder algorithm?
The postorder algorithm, similar to the [preorder](https://open.substack.com/pub/dmytrosblog/p/dsa-binary-search-tree-traverse-preorder?r=2fepxg&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true) algorithm, returns a list of values in the order they are visited.

### Code Example
```swift
final class BSTNode<Value: Comparable> {

    var val: Value?
    var left: BSTNode?
    var right: BSTNode?

    init(val: Value? = nil) {
        self.val = val
    }

    func postorder(_ visited: inout [Value]) -> [Value] {
        if self.left != nil {
            self.left!.postorder(&visited)
        }

        if self.right != nil {
            self.right!.postorder(&visited)
        }

        if self.val != nil {
            visited.append(self.val!)
        }

        return visited
    }

}
```

#### Implementation 
The postorder algorithm:
1. Recursively traverse the current node's `left` subtree.
2. Recursively traverse the current node's `right` subtree.
3. Visit the current node.  

### Time/Space Complexity
- The time complexity of the postorder algorithm is O(N), where N is the total number of nodes.
- The space complexity:
  * O(1) if recursion stack space is not considered.
  * Otherwise, O(H), where H is the height of the tree.
  * In the worst case, H can be the same as N (when the tree is skewed).
  * In the best case, H can be the same as logN (when the tree is complete).

#### Thank you for reading! 😊
