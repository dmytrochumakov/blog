+++
title = "LeetCode - 150 - Binary Tree Right Side View"
date = 2025-08-05T07:23:43+03:00
tags = ["LeetCode", "150", "Binary Tree Right Side View", "Swift"]
draft = false
+++

### The problem

Given the `root` of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

#### Examples

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
Explanation:
```

![alt image](images/tmpd5jn43fs-1.png#center)

```
Input: root = [1,2,3,4,null,null,null,5]
Output: [1,3,4,5]
Explanation:
```

![alt image](images/tmpkpe40xeh-1.png#center)

```
Input: root = [1,null,3]
Output: [1,3]
```

```
Input: root = []
Output: []
```

#### Constraints

* The number of nodes in the tree is in the range \[0, 100].
* -100 <= Node.val <= 100

#### Explanation

Let's imagine that we have a person on the right side and if we look at the first example, we can see everything on the right side.
![alt image](images/199.png#center)

* We can see the `root` node with value `1`
* We can see node with value `3`
* Lastly, we can see node with the value `4`

But we can't see nodes that are behind nodes that we can see, because they are blocking our view.

Your first thought of solving this problem might be to start from the `root` and start going to the right and down and get every node on the right side. But it will not always work.
![alt image](images/199-1.png#center)
Imagine that on our left side, node with value `5` had a left child.
![alt image](images/199-2.png#center)
You can see that node with value `7` is not being blocked, therefore we need to include it into our result.

### Depth First Search Solution

```swift
func rightSideView(_ root: TreeNode?) -> [Int] {
    var res: [Int] = []

    func dfs(_ node: TreeNode?, _ depth: Int) {
        guard let node = node else {
            return
        }

        if depth == res.count {
            res.append(node.val)
        }

        dfs(node.right, depth + 1)
        dfs(node.left, depth + 1)
    }

    dfs(root, 0)

    return res
}
```

#### Explanation

One way to solve this problem is to use DFS algorithm.
We are going to have an additional `depth` property that will help us add the first node that we see.
We prioritize `right` side by visiting `right` node, and ensure that we capture the rightmost values first.

#### Time/Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

### Breadth First Search Solution

```swift
func rightSideView(_ root: TreeNode?) -> [Int] {
    guard let root = root else {
        return []
    }
    var res: [Int] = []
    var q: [TreeNode?] = [root]

    while !q.isEmpty {
        var rightSide: TreeNode?
        let qLen = q.count
        for i in 0 ..< qLen {
            let node = q.removeFirst()
            if let node = node {
                rightSide = node
                q.append(node.left)
                q.append(node.right)
            }
        }
        if let rightSide = rightSide {
            res.append(rightSide.val)
        }
    }

    return res
}
```

#### Explanation

Another way to solve this problem is to use BFS algorithm, in other words it's called `level order traversal`.
We can frame this problem by finding the rightmost value at each level in our tree.
![alt image](images/199-3.png#center)

We are going to start from our `root` - the first layer where we get value of `1`

* Next, we are going to move to the second level where we have two nodes, so we are going to get the rightmost value that equals `3`
* Next, we move to our third layer, where we are going to find the rightmost value `4`
* Lastly, we move to the last layer, where you can see that we have only one node, so we get value that equals `7`

We are going to implement BFS algorithm by using a queue.
![alt image](images/199-4.png#center)
We will be adding nodes in order `(left node first)` then the right node, because we always want to get the rightmost value when we add to our result.

* Initially, we are going to add our `root` node to our `queue`
* We are also going to have a `res` array
* Next, we are going to take a look at our rightmost value in the queue and add `1` to our `res`
* Next, we are no longer looking at the `root` node, now we are looking for its children, so we will take `left` and `right` child and add them to our queue. It's important that we add nodes in order - `left` first and `right` second, so this way we can get the rightmost value when we add to our `res`
* Next, we have the second level setup, so we are going to take the rightmost value `3` and add it to our `res`
* Next, we are going to look at the `leftmost` value's children nodes and add them to our `queue`, and after that we are going to `pop` it
* The same goes for the rightmost value - we are going to add its children and `pop` it

We will continue doing these steps until we reach the end of the tree.

#### Time/Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
