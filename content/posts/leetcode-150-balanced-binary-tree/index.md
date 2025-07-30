+++
title = "LeetCode - 150 - Balanced Binary Tree"
date = 2025-07-30T07:35:43+03:00
tags = ["LeetCode", "150", "Balanced Binary Tree", "Swift"]
draft = false
+++

### The problem

Given a binary tree, determine if it is height-balanced.

> A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

#### Examples

![alt image](images/balance_1.jpg#center)

```
Input: root = [3,9,20,null,null,15,7]  
Output: true  
```

![alt image](images/balance_2.jpg#center)

```
Input: root = [1,2,2,3,3,null,null,4,4]  
Output: false  
```

```
Input: root = []  
Output: true  
```

#### Constraints

* The number of nodes in the tree is in the range \[0, 5000].
* -10^4 <= Node.val <= 10^4

#### Explanation

From the description of the problem, we learn that we need to find if the tree is height-balanced, and that we can do it by determining if the difference between the height of every single node in the left and right subtrees is no more than `1`.

Let's look at the given first example:
![alt image](images/110.png#center)

If we start calculating our height from the `root`, you can see that in the left subtree we have height `1` and in the right subtree we have height `2`. When we calculate the difference `abs(1 - 2)`, we get `1`, so this means that our tree is balanced.

Now, let's look at the counter example
![alt image](images/110-1.png#center)

If we take a look at our `root`:

* in our left subtree we have height `3`
* in our right subtree we have height `3`
* the difference is `0`

You can see that from the `root`, the left subtree and the right subtree are balanced.

But if we look at the next node and its left subtree, you can see that it has a height of `0`, and its right subtree has height `2`, and the difference equals `2`. Therefore, our subtrees are not balanced, and we need to calculate the height for every single node.
![alt image](images/110-2.png#center)

### Brute Force Solution

```swift
func isBalanced(_ root: TreeNode?) -> Bool {
    guard let root = root else {
        return true
    }

    let left = height(root.left)
    let right = height(root.right)

    if abs(left - right) > 1 {
        return false
    }

    return isBalanced(root.left) && isBalanced(root.right)
}

func height(_ root: TreeNode?) -> Int {
    guard let root = root else {
        return 0
    }
    return 1 + max(height(root.left), height(root.right))
}
```

#### Explanation

The brute force way to solve this problem is to recursively check every single node starting from the `root` and check if the left subtree and right subtree are balanced.

The problem with the brute force approach is that we need to go through every single node in subtrees twice. That will take `O(n^2)` time because we need to check if our subtree is balanced at each node, and since we have `n` nodes and we need to check `n` times, we are going to have `n^2` time complexity.
![alt image](images/110-3.png#center)

#### Time/ Space complexity

* Time complexity: O(n^2)
* Space complexity: O(n)

### Depth First Search Solution

```swift
func isBalanced(_ root: TreeNode?) -> Bool {
    guard let root = root else {
        return true
    }

    func dfs(_ root: TreeNode?) -> (Bool, Int) {
        guard let root = root else {
            return (true, 0)
        }

        let left = dfs(root.left)
        let right = dfs(root.right)
        let diff = abs(left.1 - right.1)
        let balanced = left.0 && right.0 && diff <= 1

        return (balanced, 1 + max(left.1, right.1))
    }

    return dfs(root).0
}
```

#### Explanation

We can solve this problem in a more optimal way by using the DFS algorithm.
Instead of asking if our tree is balanced from the `root`, we are going to start from the bottom and move up. This way, we ensure that we visit each node only once.

We start from the bottom leaf nodes, check if they are balanced, and then we pop back up. We will also need to get the height of our subtrees—it will help us go up to the parent node, find the difference between subtrees without revisiting them, and find if subtrees are balanced from the parent position.

Lastly, we are going to go back to the `root` node, and before we do that, we need to calculate the entire height of the subtree. We can do that by using the formula `1 + max(leftSubtreeHeight, rightSubtreeHeight)`, and we can find if the subtrees are balanced from the `root` node.
![alt image](images/110-4.png#center)

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
