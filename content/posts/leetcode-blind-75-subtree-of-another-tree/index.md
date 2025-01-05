+++
title = "LeetCode - Blind 75 - Subtree of Another Tree"
date = 2025-01-05T12:34:59+03:00
tags = ["LeetCode", "Blind 75", "Subtree of Another Tree", "Swift"]
draft = false
+++

### The Problem
Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values as `subRoot`, and `false` otherwise.

> A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

#### Examples
![Example 1](images/subtree1-tree.jpg#center)
```swift
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

![Example 2](images/subtree2-tree.jpg#center)
```swift
Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
Output: false
```

#### Constraints
- The number of nodes in the `root` tree is in the range [1, 2000].
- The number of nodes in the `subRoot` tree is in the range [1, 1000].
- -10⁴ <= `root.val` <= 10⁴
- -10⁴ <= `subRoot.val` <= 10⁴

---

### Depth-First Search Solution
```swift
func isSubtree(_ root: TreeNode?, _ subRoot: TreeNode?) -> Bool {
    if subRoot == nil {
        return true
    }

    if root == nil {
        return false
    }

    if isEqual(root, subRoot) {
        return true
    }

    return isSubtree(root?.left, subRoot) || isSubtree(root?.right, subRoot)
}

func isEqual(_ root: TreeNode?, _ subRoot: TreeNode?) -> Bool {
    if root == nil && subRoot == nil {
        return true
    }

    if root != nil && subRoot != nil && root?.val == subRoot?.val {
        return isEqual(root?.left, subRoot?.left) && isEqual(root?.right, subRoot?.right)
    }

    return false
}
```

#### Explanation
One way to solve this problem is by using the Depth-First Search (DFS) algorithm. Before that, we need to implement the `isEqual` helper function, which checks if the `root` and `subRoot` nodes are the same. We also handle a few base cases. As a result, we can determine if the subtree exists.

#### Time/Space Complexity
- **Time complexity**: O(m * n)
- **Space complexity**: O(m + n)  
  Where `m` is the number of nodes in `subRoot` and `n` is the number of nodes in `root`.

---

### Breadth-First Search Solution
```swift
func isSubtree(_ root: TreeNode?, _ subRoot: TreeNode?) -> Bool {
    var q: [TreeNode?] = [root]

    while !q.isEmpty {
        let node = q.removeFirst()

        if isEqual(node, subRoot) {
            return true
        }

        if node?.left != nil {
            q.append(node?.left)
        }

        if node?.right != nil {
            q.append(node?.right)
        }
    }

    return false
}

func isEqual(_ root: TreeNode?, _ subRoot: TreeNode?) -> Bool {
    var q1: [TreeNode?] = [root]
    var q2: [TreeNode?] = [subRoot]

    while !q1.isEmpty && !q2.isEmpty {
        let nodeP = q1.removeFirst()
        let nodeQ = q2.removeFirst()

        if nodeP == nil && nodeQ == nil {
            continue
        }

        if nodeP == nil || nodeQ == nil || nodeP?.val != nodeQ?.val {
            return false
        }

        q1.append(nodeP?.left)
        q1.append(nodeP?.right)
        q2.append(nodeQ?.left)
        q2.append(nodeQ?.right)
    }

    return q1.isEmpty && q2.isEmpty
}
```

#### Explanation
Another way to solve this problem is by using the Breadth-First Search (BFS) algorithm. It differs slightly from DFS because it is based on the `queue` data structure, uses iteration instead of recursion, and processes the tree level by level.

#### Time/Space Complexity
- **Time complexity**: O(m * n)
- **Space complexity**: O(m + n)  
  Where `m` is the number of nodes in `subRoot` and `n` is the number of nodes in `root`.  

#### Thank you for reading! 😊
