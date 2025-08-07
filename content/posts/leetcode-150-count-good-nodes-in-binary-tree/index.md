+++
title = "LeetCode - 150 - Count Good Nodes in Binary Tree"
date = 2025-08-07T07:55:32+03:00
tags = ["LeetCode", "150", "Count Good Nodes in Binary Tree", "Swift"]
draft = false
+++

### The problem

Given a binary tree `root`, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

#### Examples

![alt image](images/test_sample_1.png#center)

```
Input: root = [3,1,4,3,null,1,5]  
Output: 4  
Explanation: Nodes in blue are good.  
Root Node (3) is always a good node.  
Node 4 -> (3,4) is the maximum value in the path starting from the root.  
Node 5 -> (3,4,5) is the maximum value in the path  
Node 3 -> (3,1,3) is the maximum value in the path.  
```

![alt image](images/test_sample_2.png#center)

```
Input: root = [3,3,null,4,2]  
Output: 3  
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.  
```

```
Input: root = [1]  
Output: 1  
Explanation: Root is considered as good.  
```

#### Constraints

* The number of nodes in the binary tree is in the range \[1, 10^5].
* Each node's value is between \[-10^4, 10^4].

#### Explanation

Before we jump into code, let's figure out what a good node means.
The node is defined as good if, from the path from the `root` node all the way down to the current node, there are no nodes in this path that have a greater value than the node that we are currently at.
For example, if we look at the path from the `root` and the node with value `4`:
![alt image](images/1448.png#center)

* node with value `4` counts as a good node, because its value is greater than the value from the `root` node `4 > 3`

Now, let's look from the `root` node at the node on the left side with value `1`
![alt image](images/1448-1.png#center)

* node with value `1` does not count as a good node because its value is less than the `root` value `1 < 3`

### Depth First Search (Preorder) Solution

```swift
func goodNodes(_ root: TreeNode?) -> Int {
    func dfs(_ node: TreeNode?, _ maxVal: Int) -> Int {
        guard let node = node else {
            return 0
        }
        var maxVal = maxVal

        var res = node.val >= maxVal ? 1 : 0
        maxVal = max(maxVal, node.val)
        res += dfs(node.left, maxVal)
        res += dfs(node.right, maxVal)
        return res
    }

    return dfs(root, root?.val ?? 0)
}
```

#### Explanation

To solve this problem we will be using DFS preorder algorithm. We are going to start from the `root` node, after that we are going to visit the `left subtree`, next we are going to visit the `right subtree`.
To count how many good nodes we have in the `left subtree`, we are going to pass the `max` value that we have seen so far, and check if the current node value is greater than our `max` value, and if it is, then it's a `good` node.
![alt image](images/1448-2.png#center)

* When we are at the node with value `1`, we will pass `max = 3`, and check if it's a `good` node
* Next, we are going to update our `max` if necessary; in our case, our `max` will be the same, because `1 < 3`
* Next, we are going to pass our `max` to the next node with value `3`, and check if it's a `good` node
* Next, we are going to apply the same algorithm to the `right subtree`, and in the end, we have `four` good nodes.

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

### Breadth First Search Solution

```swift
func goodNodes(_ root: TreeNode?) -> Int {
    var res = 0
    var q: [(TreeNode?, Int)] = []
    q.append((root, Int.min))

    while !q.isEmpty {
        let pair = q.removeFirst()
        let node = pair.0
        let maxVal = pair.1
        if node!.val >= maxVal {
            res += 1
        }
        if node?.left != nil {
            q.append((node?.left, max(maxVal, node!.val)))
        }
        if node?.right != nil {
            q.append((node?.right, max(maxVal, node!.val)))
        }
    }

    return res
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
