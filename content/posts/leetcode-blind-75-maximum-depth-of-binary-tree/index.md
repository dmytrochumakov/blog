+++
title = "LeetCode - Blind 75 - Maximum Depth of Binary Tree"
date = 2025-01-01T10:09:24+03:00
tags = ["LeetCode", "Blind 75", "Maximum Depth of Binary Tree", "Swift"]
draft = false
+++

### The Problem 
Given the `root` of a binary tree, return its maximum depth.

> A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

#### Examples

![alt image](images/tmp-tree.jpg#center)

```swift
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

```swift
Input: root = [1,null,2]
Output: 2
```

#### Constraints
* The number of nodes in the tree is in the range `[0, 10⁴]`.
* `-100 <= Node.val <= 100`

---

### Brute Force Solution 
```swift
func maxDepth(_ root: TreeNode?) -> Int {
    if root == nil {
        return 0
    }
    return 1 + max(maxDepth(root?.left), maxDepth(root?.right))
}
```

#### Explanation
We can solve this problem using recursion by calling the `maxDepth` function on the `left` and `right` child nodes. This approach helps us find the longest path down the tree.

#### Time/Space Complexity
* **Time Complexity**: `O(n)`, where `n` is the number of nodes in the tree.
* **Space Complexity**: `O(h)`, where `h` is the height of the tree.

---

### BFS Solution 
```swift
func maxDepth(_ root: TreeNode?) -> Int {
    if root == nil {
        return 0
    }

    var q: [TreeNode?] = [root]
    var level = 0

    while !q.isEmpty {
        for _ in 0 ..< q.count {
            let node = q.removeFirst()
            if node?.left != nil {
                q.append(node?.left)
            }
            if node?.right != nil {
                q.append(node?.right)
            }
        }
        level += 1
    }

    return level
}
```

#### Explanation
We can solve this problem using the BFS algorithm by counting the levels of the tree. If you've never implemented BFS before, you can check the [pseudocode section](https://en.wikipedia.org/wiki/Breadth-first_search#:~:text=%5B8%5D-,Pseudocode,-%5Bedit%5D) on Wikipedia to learn more about it.

#### Time/Space Complexity
* **Time Complexity**: `O(n)`
* **Space Complexity**: `O(n)`

#### Thank you for reading! 😊
