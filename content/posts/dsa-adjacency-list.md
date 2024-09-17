+++
title = 'DSA - Adjacency List'
date = 2024-09-17T07:06:52+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Adjacency List", "Graph", "Swift"]
draft = false
+++

### What is an Adjacency List?

An [Adjacency List](https://en.wikipedia.org/wiki/Adjacency_list) helps store a list of connections between each vertex in a finite [graph](https://open.substack.com/pub/dmytrosblog/p/dsa-graph?r=2fepxg&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true).

| Vertex | Connects with       |
|--------|---------------------|
| 0      | 1                   |
| 1      | 0, 2, 3             |
| 2      | 1, 3                |
| 3      | 1, 2                |

### Implementation

The `addEdge` method takes vertices as input and adds an edge to the adjacency list. In this example, the adjacency list is represented as a dictionary that maps vertices to a `set` of all connected vertices.

In JSON form, it looks like this:

```json
{
    "0": [1],
    "1": [0, 2, 3],
    "2": [1, 3],
    "3": [1, 2]
}
```
#### Code Example
```swift
final class Graph {

    private(set) var graph: [Int: Set<Int>]

    init() {
        self.graph = [:]
    }

    func addEdge(u: Int, v: Int) -> [Int: Set<Int>] {
        if self.graph[u] == nil {
            self.graph[u] = [v]
        } else {
            self.graph[u]!.insert(v)
        }

        if self.graph[v] == nil {
            self.graph[v] = [u]
        } else {
            self.graph[v]!.insert(u)
        }

        return self.graph
    }
}
```

As for the implementation, the `addEdge` algorithm checks:
1. If vertex `u` is already in the `graph`, it inserts vertex `v` into the `set`.
2. Otherwise, it creates a new `set` for `u` with vertex `v`.
3. Finally, it repeats steps 1 & 2 but swaps `u` and `v`.

#### Thank you for reading! 😊
