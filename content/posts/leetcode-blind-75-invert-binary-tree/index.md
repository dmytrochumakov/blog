+++
title = "LeetCode - Blind 75 - Invert Binary Tree"
date = 2024-12-30T09:54:49+03:00
tags = ["LeetCode", "Blind 75", "Invert Binary Tree", "Swift"]
draft = false
+++

### The Problem  
Given the `root` of a binary tree, invert the tree, and return its root.

#### Examples  
![alt image](images/invert1-tree.jpg#center)  
```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

![alt image](images/invert2-tree.jpg#center)  
```
Input: root = [2,1,3]
Output: [2,3,1]
```

```
Input: root = []
Output: []
```

#### Constraints  
* The number of nodes in the tree is in the range [0, 100].  
* -100 <= Node.val <= 100  

---

### Depth First Search Solution  
```swift
func invertTree(_ root: TreeNode?) -> TreeNode? {
    if root == nil {
        return nil
    }

    let tmp = root?.left
    root?.left = root?.right
    root?.right = tmp

    invertTree(root?.left)
    invertTree(root?.right)

    return root
}
```

#### Explanation  
When working with trees, we often consider two general ways to solve these problems: depth-first search (DFS) and breadth-first search (BFS).  
In this case, we use the DFS algorithm, where we swap the `left` subtree with the `right` subtree and recursively call `invertTree` on the `left` and `right` nodes.  

---

#### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

---

### Breadth First Search Solution  
```swift
func invertTree(_ root: TreeNode?) -> TreeNode? {
    if root == nil {
        return nil
    }

    var q: [TreeNode?] = [root]

    while !q.isEmpty {
        let node = q.removeFirst()

        let tmp = node?.left
        node?.left = node?.right
        node?.right = tmp

        if node?.left != nil {
            q.append(node?.left)
        }

        if node?.right != nil {
            q.append(node?.right)
        }
    }

    return root
}
```  

#### Explanation  
To solve this problem using BFS, we initialize a queue and traverse the tree level by level, swapping the `left` and `right` subtrees until the queue is empty.  
We also update the queue by adding the `left` or `right` nodes if they exist.  

#### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

#### Thank you for reading! 😊
