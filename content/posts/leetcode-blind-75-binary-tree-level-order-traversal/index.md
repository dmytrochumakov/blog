+++
title = "LeetCode - Blind 75 - Binary Tree Level Order Traversal"
date = 2025-01-08T13:23:04+03:00
tags = ["LeetCode", "Blind 75", "Binary Tree Level Order Traversal", "Swift"]
draft = false
+++

### The Problem 
Given the `root` of a binary tree, return the level order traversal of its nodes' values (i.e., from left to right, level by level).

#### Examples

![alt image](images/tree1.jpg#center)

``` 
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

```
Input: root = [1]
Output: [[1]]
```

```
Input: root = []
Output: []
```

#### Constraints
* The number of nodes in the tree is in the range [0, 2000].
* -1000 <= Node.val <= 1000

---

### BFS Solution 

```swift
func levelOrder(_ root: TreeNode?) -> [[Int]] {
    if root == nil {
        return []
    }

    var queue: [TreeNode?] = []
    queue.append(root)

    var res: [[Int]] = []

    while !queue.isEmpty {
        var level: [Int] = []

        for _ in 0 ..< queue.count {
            let node = queue.removeFirst()

            if let node = node {
                level.append(node.val)
                queue.append(node.left)
                queue.append(node.right)
            }
        }

        if !level.isEmpty {
            res.append(level)
        }
    }

    return res
}
```

#### Explanation
When you see that a binary tree problem involves level order traversal, it means the solution usually implies the breadth-first search (BFS) algorithm. BFS is a common solution to this problem because, according to Wikipedia, breadth-first search is also called “level-order search.” The core idea behind BFS is to iterate level by level from top to bottom, and from left to right.

![alt image](images/tree1-1.png#center)

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

---

### DFS Solution  

```swift
func levelOrder(_ root: TreeNode?) -> [[Int]] {
    var res: [[Int]] = []

    func dfs(_ node: TreeNode?, _ depth: Int) {
        guard let node = node else {
            return
        }

        if res.count == depth {
            res.append([])
        }

        res[depth].append(node.val)

        dfs(node.left, depth + 1)
        dfs(node.right, depth + 1)
    }

    dfs(root, 0)

    return res
}
``` 

#### Explanation
You can also solve this problem by using the depth-first search (DFS) algorithm. In this case, you will need to visit the `left` and `right` subtrees separately and count the `depth` accordingly.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)  

#### Thank you for reading! 😊
