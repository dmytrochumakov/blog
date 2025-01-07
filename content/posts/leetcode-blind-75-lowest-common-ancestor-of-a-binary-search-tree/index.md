+++
title = "LeetCode - Blind 75 - Lowest Common Ancestor of a Binary Search Tree"
date = 2025-01-07T12:19:23+03:00
tags = ["LeetCode", "Blind 75", "Lowest Common Ancestor of a Binary Search Tree", "Swift"]
draft = false
+++

### The Problem
Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

> According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).”

#### Examples

![alt image](images/binarysearchtree_improved.png#center)

``` 
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

![alt image](images/binarysearchtree_improved.png#center)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

```
Input: root = [2,1], p = 2, q = 1
Output: 2
```

#### Constraints
* The number of nodes in the tree is in the range [2, 10⁵].
* -10⁹ <= Node.val <= 10⁹
* All Node.val are unique.
* p != q
* p and q will exist in the BST.

---

### Recursive Solution
```swift
func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
    if root == nil || p == nil || q == nil {
        return nil
    }

    if (max(p!.val, q!.val)) < root!.val {
        return lowestCommonAncestor(root?.left, p, q)
    } else if (min(p!.val, q!.val)) > root!.val {
        return lowestCommonAncestor(root?.right, p, q)
    } else {
        return root
    }
}
```

#### Explanation
If we slightly rephrase the definition of the problem: we need to find the split where both `p` and `q` nodes are in different subtrees.  
To do that, we need to determine in which subtree we can find the `p` or `q` nodes.

For example, for nodes with `p = 2, q = 8`, the LCA will be `6`.

![alt image](images/binarysearchtree_improved_1.png#center)

#### Time/Space Complexity
* Time complexity: O(h)  
* Space complexity: O(h)  
Where `h` is the height of the tree.

---

### Iterative Solution
```swift
func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
    var curr = root

    if p == nil {
        return nil
    }

    if q == nil {
        return nil
    }

    while curr != nil {
        if p!.val < curr!.val && q!.val < curr!.val {
            curr = curr?.left
        } else if p!.val > curr!.val && q!.val > curr!.val {
            curr = curr?.right
        } else {
            return curr
        }
    }

    return root
}
```

#### Explanation
We can solve this problem using iteration. This solution is more memory-efficient because you don’t have recursive stack calls that take additional memory.  
The algorithm looks like this:  
- Compare `p` and `q` values with the `curr` value.  
  - If they are less than `curr`, move to the `left` subtree.  
  - If they are greater than `curr`, move to the `right` subtree.  
  - If one is less and the other is greater (or either equals `curr`), we’ve found the LCA.

#### Time/Space Complexity
* Time complexity: O(h) - where `h` is the height of the tree  
* Space complexity: O(1)

#### Thank you for reading! 😊
