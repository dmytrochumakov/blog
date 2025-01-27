+++
title = "LeetCode - Blind 75 - Binary Tree Maximum Path Sum"
date = 2025-01-27T10:24:47+03:00
tags = ["LeetCode", "Blind 75", "Binary Tree Maximum Path Sum", "Swift"]
draft = false
+++

### The Problem
> A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.  

> The path sum of a path is the sum of the node's values in the path.  

Given the `root` of a binary tree, return the maximum path sum of any non-empty path.  

---

#### Examples  

![alt image](images/exx1.jpg#center)  
```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```  

![alt image](images/exx2.jpg#center)  
```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```  

---

#### Constraints  
* The number of nodes in the tree is in the range [1, 3 * 10⁴].  
* -1000 <= Node.val <= 1000  

---

### Depth-First Search Solution  

```swift
func maxPathSum(_ root: TreeNode?) -> Int {
    var res = Int.min

    func dfs(_ root: TreeNode?) {
        guard let root = root else {
            return
        }

        let left = getMax(root.left)
        let right = getMax(root.right)

        res = max(res, root.val + left + right)

        dfs(root.left)
        dfs(root.right)
    }

    dfs(root)

    return res
}

func getMax(_ root: TreeNode?) -> Int {
    guard let root = root else {
        return 0
    }
    let left = getMax(root.left)
    let right = getMax(root.right)
    let path = root.val + max(left, right)
    return max(0, path)
}
```

---

#### Explanation  

We can solve this problem using the DFS algorithm and an additional helper function needed to determine the path sum.  

To do that, we start from the `root` and move to the `left` subtree to find the max path. For example, with `root = [1,2,null,null,3,4,5]`, we move from root `1` to the left `2`. Since the left subtree only has one value, our max path equals `2`.  

After that, we go to the right subtree, which looks like `[3,4,5]`. Now, we calculate the max path for the `left` and `right` subtrees; it will be `4` and `5`, respectively.  

Finally, we go back up to the root and calculate our result.  

This is not a very efficient solution as it takes **O(n²)** time.  

---

#### Time/Space Complexity  

- **Time Complexity**: O(n²)  
- **Space Complexity**: O(n)  

---

### Optimized DFS Solution  

```swift
func maxPathSum(_ root: TreeNode?) -> Int {
    var res: Int = root!.val

    func dfs(_ root: TreeNode?) -> Int {
        guard let root = root else {
            return 0
        }

        var leftMax = dfs(root.left)
        var rightMax = dfs(root.right)

        leftMax = max(leftMax, 0)
        rightMax = max(rightMax, 0)

        res = max(res, root.val + leftMax + rightMax)

        return root.val + max(leftMax, rightMax)
    }

    dfs(root)

    return res
}
```  

---

#### Explanation  

We can optimize the brute force DFS solution by eliminating the unnecessary recursion of the `getMax` helper function. All we need is to replace the `getMax` function with the built-in `max` function and return `root.val + max(leftMax, rightMax)`. The rest remains the same.  

---

#### Time/Space Complexity  

- **Time Complexity**: O(n)  
- **Space Complexity**: O(n)  

---  

#### Thank you for reading! 😊
