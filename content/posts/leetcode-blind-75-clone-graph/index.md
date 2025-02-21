+++
title = "LeetCode - Blind 75 - Clone Graph"
date = 2025-02-21T12:49:46+03:00
tags = ["LeetCode", "Blind 75", "Clone Graph", "Swift"]
draft = false
+++

### The Problem
Given a reference to a node in a [connected](https://en.wikipedia.org/wiki/Connectivity_(graph_theory)#Connected_graph) undirected graph, return a [deep copy](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors:

```swift
class Node {
    public int val;
    public List<Node> neighbors;
}
```

#### Test Case Format
For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node has `val == 1`, the second node has `val == 2`, and so on. The graph is represented in the test case using an adjacency list.

> An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the copy of the given node as a reference to the cloned graph.

### Examples

#### Example 1
![alt image](images/133_clone_graph_question.png#center)
```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are the 2nd node (val = 2) and the 4th node (val = 4).
2nd node (val = 2)'s neighbors are the 1st node (val = 1) and the 3rd node (val = 3).
3rd node (val = 3)'s neighbors are the 2nd node (val = 2) and the 4th node (val = 4).
4th node (val = 4)'s neighbors are the 1st node (val = 1) and the 3rd node (val = 3).
```

#### Example 2
![alt image](images/graph.png#center)
```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1, and it does not have any neighbors.
```

#### Example 3
```
Input: adjList = []
Output: []
Explanation: This is an empty graph; it does not have any nodes.
```

### Constraints
* The number of nodes in the graph is in the range `[0, 100]`.
* `1 <= Node.val <= 100`
* `Node.val` is unique for each node.
* There are no repeated edges and no self-loops in the graph.
* The graph is connected, and all nodes can be visited starting from the given node.

---

### Depth-First Search Solution

```swift
func cloneGraph(_ node: Node?) -> Node? {
    guard let node = node else {
        return nil
    }

    var oldToNew: [Node: Node] = [:]

    func dfs(_ node: Node) -> Node {
        if oldToNew[node] != nil {
            return oldToNew[node]!
        }

        let copy = Node(node.val)
        oldToNew[node] = copy

        for nei in node.neighbors {
            copy.neighbors.append(dfs(nei!))
        }

        return copy
    }

    return dfs(node)
}
```

#### Explanation
We can solve this problem using a hashmap and the depth-first search (backtracking) algorithm. The approach involves:
- Visiting the first node, creating its clone, checking its neighbors, creating their clones, and connecting them accordingly.
- Continuing this process recursively.

For example, with input `adjList = [[2,4],[1,3],[2,4],[1,3]]`, we:
- Visit node `1`, create its clone, check its neighbors (node `2`), create its clone, and connect nodes `1` and `2`.
- Repeat this process for nodes `2 -> 4`, `4 -> 3`, and `3 -> 1`.

![alt image](images/p-133.png#center)

##### Time/Space Complexity
* **Time Complexity:** O(V + E)
* **Space Complexity:** O(V)
* Where `V` is the number of vertices and `E` is the number of edges.

---

### Breadth-First Search Solution

```swift
func cloneGraph(_ node: Node?) -> Node? {
    guard let node = node else {
        return nil
    }

    var oldToNew: [Node: Node] = [:]
    oldToNew[node] = Node(node.val)

    var q: [Node] = [node]

    while !q.isEmpty {
        let curr = q.removeFirst()
        for nei in curr.neighbors {
            if oldToNew[nei!] == nil {
                oldToNew[nei!] = Node(nei!.val)
                q.append(nei!)
            }
            oldToNew[curr]?.neighbors.append(oldToNew[nei!])
        }
    }

    return oldToNew[node]
}
```

#### Explanation
We can also solve this problem using the breadth-first search algorithm. The approach involves:
- Creating a clone of `node` and adding it to a hashmap before starting iteration.
- Iterating through each neighbor, creating clones, and adding them to the hashmap.

##### Time/Space Complexity
* **Time Complexity:** O(V + E)
* **Space Complexity:** O(V)
* Where `V` is the number of vertices and `E` is the number of edges.

#### Thank you for reading! 😊
