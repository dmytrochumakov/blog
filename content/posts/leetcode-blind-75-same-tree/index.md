+++
title = "LeetCode - Blind 75 - Same Tree"
date = 2025-01-02T12:30:42+03:00
tags = ["LeetCode", "Blind 75", "Same Tree", "Swift"]
draft = false
+++

#### The Problem  
Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

> Two binary trees are considered the same if they are structurally identical, and the nodes have the same values.

#### Examples  

![alt image](images/ex1.jpg#center)
```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

![alt image](images/ex2.jpg#center)
```
Input: p = [1,2], q = [1,null,2]
Output: false
```

![alt image](images/ex3.jpg#center)
```
Input: p = [1,2,1], q = [1,1,2]
Output: false
```

#### Constraints  
* The number of nodes in both trees is in the range [0, 100].  
* -10⁴ <= Node.val <= 10⁴  

---

### Depth First Search Solution  

```swift
func isSameTree(_ p: TreeNode?, _ q: TreeNode?) -> Bool {
    if p == nil && q == nil {
        return true
    }

    if p == nil || q == nil {
        return false
    }

    if p?.val != q?.val {
        return false
    }

    return isSameTree(p?.left, q?.left) && isSameTree(p?.right, q?.right)
}
```

#### Explanation  
One way to solve this problem is using depth-first search. We need to be very cautious with base cases, as they can affect the result. In our case, the trees can be the same if `p` and `q` are both `nil` and their values are equal. In all other cases, the trees are not the same.

> If you have never implemented the DFS algorithm before, you can refer to this [Wiki example](https://en.wikipedia.org/wiki/Depth-first_search#:~:text=high%20degree.-,Example,-%5Bedit%5D).

##### Time/Space Complexity  
* **Time complexity**: O(n)  
* **Space complexity**: O(h), where `h` is the height of the tree  

### Breadth First Search Solution  

```swift
func isSameTree(_ p: TreeNode?, _ q: TreeNode?) -> Bool {
    var q1: [TreeNode?] = [p]
    var q2: [TreeNode?] = [q]

    while !q1.isEmpty && !q2.isEmpty {
        let nodeP = q1.removeFirst()
        let nodeQ = q2.removeFirst()

        if nodeP == nil && nodeQ == nil {
            continue
        }

        if nodeP == nil || nodeQ == nil || nodeP?.val != nodeQ?.val {
            return false
        }

        q1.append(nodeP?.left)
        q1.append(nodeP?.right)
        q2.append(nodeQ?.left)
        q2.append(nodeQ?.right)
    }

    return q1.isEmpty && q2.isEmpty
}
```

#### Explanation  
Another way to solve this problem is by using the breadth-first search algorithm. In this case, we use two queues and a few additional checks to determine the equality of the trees. Trees can be the same if `p` and `q` are both `nil` and their values are equal. In all other cases, the trees are not the same.

> If you have never implemented the BFS algorithm before, you can refer to this [Wiki example](https://en.wikipedia.org/wiki/Breadth-first_search#:~:text=%5B8%5D-,Pseudocode,-%5Bedit%5D).

##### Time/Space Complexity  
* **Time complexity**: O(n)  
* **Space complexity**: O(n)  

#### Thank you for reading! 😊
