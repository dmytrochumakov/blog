+++
title = "LeetCode - Blind 75 - Serialize and Deserialize Binary Tree"
date = 2025-01-28T12:30:52+03:00
tags = ["LeetCode", "Blind 75", "Serialize and Deserialize Binary Tree", "Swift"]
draft = false
+++

### The Problem
> Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as [how LeetCode serializes a binary tree](https://support.leetcode.com/hc/en-us/articles/32442719377939-How-to-create-test-cases-on-LeetCode#h_01J5EGREAW3NAEJ14XC07GRW1A). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

#### Examples
![alt image](images/serdeser.jpg#center)

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

```
Input: root = []
Output: []
```

#### Constraints
* The number of nodes in the tree is in the range [0, 10⁴].
* -1000 <= Node.val <= 1000

---

### Depth-First Search Solution
```swift
func serialize(_ root: TreeNode?) -> String {
    var res: [String] = []

    func dfs(_ root: TreeNode?) {
        guard let root = root else {
            res.append("nil")
            return
        }

        res.append(String(root.val))
        dfs(root.left)
        dfs(root.right)
    }

    dfs(root)
    return res.joined(separator: ",")
}

func deserialize(_ data: String) -> TreeNode? {
    var values = data.components(separatedBy: ",")
    var i = 0

    func dfs() -> TreeNode? {
        if values[i] == "nil" {
            i += 1
            return nil
        }
        let node = TreeNode(Int(values[i])!)
        i += 1
        node.left = dfs()
        node.right = dfs()
        return node
    }

    return dfs()
}
```

#### Explanation
The first step to solving this problem is `serialize` a node to a string: to do that, we need to find every value in the `root` node and decide how to represent `null` values. We will use `"nil"` for null, as this is the default behavior in Swift. To find the values, we use a depth-first search (DFS) algorithm, adding found values to an array. After DFS completes, we `join` the values with a `comma` separator.

The second step is to `deserialize` the data. To do this, we `split` the input string using the same `comma` separator we used when serializing the node. We use a DFS algorithm to reconstruct the tree, with one base case to verify if the value is `"nil"`.

#### Time/Space Complexity
* **Time complexity**: O(n)
* **Space complexity**: O(n)

---

### Breadth-First Search Solution
```swift
func serialize(_ root: TreeNode?) -> String {
    if root == nil {
        return "nil"
    }

    var res: [String] = []
    var queue: [TreeNode?] = [root]

    while !queue.isEmpty {
        let node = queue.removeFirst()
        if node == nil {
            res.append("nil")
        } else {
            res.append(String(node!.val))
            queue.append(node!.left)
            queue.append(node!.right)
        }
    }

    return res.joined(separator: ",")
}

func deserialize(_ data: String) -> TreeNode? {
    var values = data.components(separatedBy: ",")

    if values[0] == "nil" {
        return nil
    }

    let root = TreeNode(Int(values[0])!)
    var queue: [TreeNode?] = [root]
    var idx = 1

    while !queue.isEmpty {
        let node = queue.removeFirst()
        if values[idx] != "nil" {
            node?.left = TreeNode(Int(values[idx])!)
            queue.append(node?.left)
        }
        idx += 1
        if values[idx] != "nil" {
            node?.right = TreeNode(Int(values[idx])!)
            queue.append(node?.right)
        }
        idx += 1
    }

    return root
}
```

#### Explanation
Another way to solve this problem is by using a breadth-first search (BFS) algorithm. The string representation for `null` and the separator remain the same; we only change the underlying mechanism to traverse nodes when we `serialize` and find values to create new nodes when we `deserialize` the string.

#### Time/Space Complexity
* **Time complexity**: O(n)
* **Space complexity**: O(n) 

#### Thank you for reading! 😊
