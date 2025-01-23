+++
title = "LeetCode - Blind 75 - Construct Binary Tree from Preorder and Inorder Traversal"
date = 2025-01-23T15:17:04+03:00
tags = ["LeetCode", "Blind 75", "Construct Binary Tree from Preorder and Inorder Traversal", "Swift"]
draft = false
+++

### The Problem  
Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.  

#### Examples  
![alt image](images/tree.jpg#center)  
``` 
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```  

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```  

#### Constraints  
- 1 <= preorder.length <= 3000  
- inorder.length == preorder.length  
- -3000 <= preorder[i], inorder[i] <= 3000  
- `preorder` and `inorder` consist of unique values.  
- Each value of `inorder` also appears in `preorder`.  
- `preorder` is guaranteed to be the preorder traversal of the tree.  
- `inorder` is guaranteed to be the inorder traversal of the tree.  

---

### Depth First Search Solution  
```swift
func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
    if preorder.isEmpty || inorder.isEmpty {
        return nil
    }

    let rootVal = preorder[0]
    let root = TreeNode(rootVal)
    guard let mid = inorder.firstIndex(of: rootVal) else {
        return nil
    }

    if mid > 0 {
        root.left = buildTree(Array(preorder[1...mid]), Array(inorder[0..<mid]))
    }

    if mid < inorder.count - 1 {
        root.right = buildTree(Array(preorder[mid + 1..<preorder.count]), Array(inorder[mid + 1..<inorder.count]))
    }

    return root
}
```  

#### Explanation  
We can solve this problem by understanding how preorder and inorder traversal work.  
- **Preorder Traversal:**  
  For preorder traversal, we start from the `root`, then move to the `left` subtree, and after that to the `right` subtree. For example, with the input `preorder = [3,9,20,15,7]`, the `root` is always the first element in the array (in this case, `3`). Knowing this, we can recursively reconstruct the `left` and `right` subtrees.  

- **Inorder Traversal:**  
  Inorder traversal starts with the `left` subtree first, then moves to the `root`, and after that to the `right` subtree. For example, with the input `inorder = [9,3,15,20,7]`, every value to the left of the `root` belongs to the `left` subtree, and every value to the right of the `root` belongs to the `right` subtree.  

Using this information, we can find the `root` and then partition the arrays into subarrays to reconstruct the tree.  

This is not a very efficient solution and takes O(n^2) time and space, but we can optimize it.  

##### Time/Space Complexity  
- **Time complexity:** O(n^2)
- **Space complexity:** O(n^2)

---

### DFS Optimal Solution  
```swift
func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
    var indices: [Int: Int] = [:]
    for (i, v) in inorder.enumerated() {
        indices.updateValue(i, forKey: v)
    }
    var preIdx = 0

    func dfs(_ l: Int, _ r: Int) -> TreeNode? {
        if l > r {
            return nil
        }

        let rootVal = preorder[preIdx]
        preIdx += 1
        let root = TreeNode(rootVal)
        let mid = indices[rootVal]!

        root.left = dfs(l, mid - 1)
        root.right = dfs(mid + 1, r)

        return root
    }

    return dfs(0, inorder.count - 1)
}
```  

#### Explanation  
We can solve this problem in a more optimal way by using the DFS algorithm and a hashmap. The logic behind the solution stays the same: we find the `root value` from the `preorder` array, then find the `mid` index in the `inorder` array, and build the tree using recursion.  

##### Time/Space Complexity  
- **Time complexity:** O(n) 
- **Space complexity:** O(n)

---  

#### Thank you for reading! 😊
