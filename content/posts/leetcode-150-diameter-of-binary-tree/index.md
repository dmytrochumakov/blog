+++
title = "LeetCode - 150 - Diameter of Binary Tree"
date = 2025-07-26T07:15:32+03:00
tags = ["LeetCode", "150", "Diameter of Binary Tree", "Swift"]
draft = false
+++

### The problem

Given the `root` of a binary tree, return the length of the diameter of the tree.

> The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

> The length of a path between two nodes is represented by the number of edges between them.

#### Examples

![alt image](images/diamtree.jpg#center)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

```
Input: root = [1,2]
Output: 1
```

#### Constraints

* The number of nodes in the tree is in the range \[1, 10^4].
* -100 <= Node.val <= 100

#### Explanation

From the problem statement, we learn that we are given the `root` of a binary tree and we need to find the diameter of the binary tree. The diameter is defined as the longest path from a particular node.

In the given example, we can see that the longest path equals `3`, because on the left side we have our longest path equal to `2`, derived from the number of edges on that side. On the right side, we only have `1` edge, so the diameter through the `root` would be `2 + 1 = 3`.
![alt image](images/543.png#center)

The problem statement also mentioned an edge case when the longest path may not pass through the `root`.
Let's look at the counterexample:
![alt image](images/543-1.png#center)
On the left side, we have nothing. On the right side, we have multiple nodes.

We can try to find the longest path on the left and the longest path on the right and add them together using the DFS algorithm, but the problem with this approach is that we will be returning the height, not the diameter.

### Depth First Search Solution

```swift
var res = 0
func diameterOfBinaryTree(_ root: TreeNode?) -> Int {

    func dfs(_ node: TreeNode?) -> Int {
        guard let node = node else {
            return 0
        }
        let left = dfs(node.left)
        let right = dfs(node.right)
        res = max(res, left + right)
        return 1 + max(left, right)
    }

    dfs(root)

    return res
}
```

#### Explanation

![alt image](images/543-2.png#center)
We are going to solve this problem by recursively running DFS:

* On the left side, we have no nodes, therefore our height would be `0`
* On the right side, we continuously go down, and at the bottom, we start calculating our height.

Above we learned that the height is different from the diameter, so to find the diameter, we are going to take the `max` of the left and the right subtrees and add `one` to it.

We will have a local variable `res` that we will use to store our calculated diameter.

When we reach the bottom, we can see that the height at the node with value `6` is equal to `one`, and the height for the node with value `7` is equal to `one`.

* Next, we are going to calculate the diameter by taking `max(leftSubtree, rightSubtree) + 1` - `max(1, 1) + 1 = 2`
* Since in our example the `left subtree` and the `right subtree` are symmetrical, the `right subtree` is going to have the same height and diameter as the `left subtree` - `max(1, 1) + 1 = 2`.
* To calculate our `res`, we are going to take the sum of `leftSubtree` and the `rightSubtree`, and if the result is more than the current `res`, we are going to store it - `max(res, leftSubtree + rightSubtree)`.

We will continue calculating our diameter until our DFS finishes its work.

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(h)
* Where `n` is the number of nodes in the tree, and `h` is the height of the tree.

#### Thank you for reading! 😊
