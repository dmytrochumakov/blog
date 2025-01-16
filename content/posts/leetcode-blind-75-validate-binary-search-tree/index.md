+++
title = "LeetCode - Blind 75 - Validate Binary Search Tree"
date = 2025-01-16T13:07:12+03:00
tags = ["LeetCode", "Blind 75", "Validate Binary Search Tree", "Swift"]
draft = false
+++

### The Problem 
Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

A **valid BST** is defined as follows:
* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

> A **subtree** of `treeName` is a tree consisting of a node in `treeName` and all of its descendants.

#### Examples
![alt image](images/tree1.jpg#center)
``` 
Input: root = [2,1,3]
Output: true
```

![alt image](images/tree2.jpg#center)
```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5, but its right child's value is 4.
```

#### Constraints
* The number of nodes in the tree is in the range [1, 10⁴].
* -2³¹ <= Node.val <= 2³¹ - 1

---

### Depth-First Search Solution 
```swift 
func isValidBST(_ root: TreeNode?) -> Bool {
    func valid(_ node: TreeNode?, _ left: Int, _ right: Int) -> Bool {
        if node == nil {
            return true
        }

        if !(node!.val < right && node!.val > left) {
            return false
        }

        return (valid(node!.left, left, node!.val)
                &&
                valid(node!.right, node!.val, right))
    }

    return valid(root, Int.min, Int.max)
}
```

#### Explanation
One of the ways to solve this problem is by using the DFS algorithm. The DFS algorithm allows us to retrieve the `left` and `right` subtree node values and determine if the `root` node is a valid BST.  
To do this, we need to check a few base cases:
- If the node is `nil`, this means it is a valid node.
- If the condition `(node!.val < right && node!.val > left)` is `false`, this means it is an invalid BST.

You can see that in the `valid` method, we have `left` and `right` parameters. We use them as boundaries for our verification condition.  
For example, with `root = [2,1,3]`:
1. We start with `left = Int.min` and `right = Int.max`, so our condition `Int.min < 2 < Int.max` is `true`. 
2. Next, we move to the left subtree with value `1`. Now, our condition looks like this: `Int.min < 1 < 2`, which is also `true`.
3. Finally, we look at the right subtree with value `3`, and the condition `2 < 3 < Int.max` is also `true`.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

---

### Breadth-First Search Solution 
```swift 
func isValidBST(_ root: TreeNode?) -> Bool {
    if root == nil {
        return true
    }

    var queue = [(root, Int.min, Int.max)]

    while !queue.isEmpty {
        let (node, left, right) = queue.removeFirst()

        if !(node!.val < right && node!.val > left) {
            return false
        }

        if node?.left != nil {
            queue.append((node!.left, left, node!.val))
        }

        if node?.right != nil {
            queue.append((node!.right, node!.val, right))
        }
    }

    return true
}
```

#### Explanation
We can solve this problem in another way by using the BFS algorithm. The BFS algorithm allows us to move level by level and determine the `node`, `left`, and `right` values.  
The condition for validation remains the same.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
