+++
title = 'DSA - Binary Search Tree - Traverse - Preorder'
date = 2024-09-26T07:01:30+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "BST", "Traverse", "Preorder", "Swift"]
draft = false
+++

### What is tree traversal?
Tree traversal, also known as "tree search" or "walking the tree," is the process of visiting each node in a tree data structure exactly once.

### What is the BST preorder algorithm?
The preorder algorithm returns a list of values in the order they are visited. It makes a copy that preserves the structure and recursively traverses the BST.

### Code example
```swift
func preorder(_ visited: inout [Value]) -> [Value] {
    if self.val != nil {
        visited.append(self.val!)
    }

    if self.left != nil {
        self.left!.preorder(&visited)
    }

    if self.right != nil {
        self.right!.preorder(&visited)
    }

    return visited
}
```

### Implementation
1. The first step is to check if `val` exists. If it does, append `val` to the `visited` array.
2. The second step is to check if the `left` node exists. If it does, recursively call the `preorder` method on the `left` node.
3. The third step is similar to the second one but for the `right` node.

### Time/Space Complexity
- The time complexity of the preorder algorithm is O(N), where N is the total number of nodes.
- The space complexity:
  * O(1) if no recursion stack space is considered.
  * Otherwise, O(H), where H is the height of the tree.
  * In the worst case, H can be the same as N (when the tree is skewed).
  * In the best case, H can be the same as logN (when the tree is complete).

#### Thank you for reading! 😊
