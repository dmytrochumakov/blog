+++
title = "LeetCode - Blind 75 - Kth Smallest Element in a BST"
date = 2025-01-20T08:49:50+03:00
tags = ["LeetCode", "Blind 75", "Kth Smallest Element in a BST", "Swift"]
draft = false
+++

### The Problem
Given the `root` of a binary search tree and an integer `k`, return the `kth` smallest value (1-indexed) among all the values of the nodes in the tree.

#### Examples
![alt image](images/kthtree1.jpg#center)
``` 
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

![alt image](images/kthtree2.jpg#center)
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

#### Constraints
* The number of nodes in the tree is `n`.
* 1 <= k <= n <= 10⁴
* 0 <= Node.val <= 10⁴

**Follow-up:** If the BST is modified often (i.e., we can perform insert and delete operations) and you need to find the `kth` smallest element frequently, how would you optimize?

---

### Depth-First Search Solution
```swift
func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
    var arr: [Int] = []

    func dfs(_ node: TreeNode?) {
        if node == nil {
            return
        }
        dfs(node!.left)
        arr.append(node!.val)
        dfs(node!.right)
    }
    
    dfs(root)

    return arr[k - 1]
}
```

#### Explanation
When it comes to searching in trees, it usually means we can use the Depth-First Search (DFS) algorithm. In this problem, we can use **inorder traversal**.

Inorder traversal visits the left subtree first, then the parent node, and finally all nodes in the right subtree. Knowing this, we can use it to our advantage to solve this problem efficiently.

In our case, for the `root = [3,1,4,null,2], k = 1`, the `arr` will look like `[1, 2, 3, 4]`, and the result will be `1`.

---

#### Time/Space Complexity
* **Time complexity:** O(n)  
* **Space complexity:** O(n)

---

### Iterative DFS Solution
```swift
func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
    var stack: [TreeNode] = []
    var curr = root
    var visitedCount = 0

    while !stack.isEmpty || curr != nil {
        while curr != nil {
            stack.append(curr!)
            curr = curr!.left
        }

        curr = stack.removeLast()
        visitedCount += 1

        if visitedCount == k {
            return curr!.val
        }

        curr = curr!.right
    }

    return -1 // This line should ideally never be reached due to constraints.
}
```

#### Explanation
We can solve this problem using another approach: **iterative DFS**. This involves maintaining a stack to handle left subtree nodes first, then checking the parent node, and finally moving to the right subtree. 

The space complexity is the same as the recursive inorder DFS solution due to the additional stack data structure.

---

#### Time/Space Complexity
* **Time complexity:** O(n)  
* **Space complexity:** O(n)  

#### Thank you for reading! 😊
