+++
title = 'DSA - Graph'
date = 2024-09-14T06:57:03+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Graph", "Swift"]
draft = false
+++

### What is a graph?
A [graph](https://en.wikipedia.org/wiki/Graph_(abstract_data_type)) is an abstract data type that represents vertices and edges that connect those vertices.

![alt image](images/ada_cs_struct_graph_components.svg#center)

[Source](https://adacomputerscience.org/images/content/computer_science/data_structures_and_algorithms/data_structures/figures/ada_cs_struct_graph_components.svg)

### Implementation
A graph can be represented as a matrix with edges connecting each pair of vertices. For example, a graph with vertices 0, 1, 2, 3, 4 and edges between them can be represented as a matrix:

|   | 0    | 1    | 2    | 3    | 4    |
|---|------|------|------|------|------|
| 0 | false | true  | false | false | true  |
| 1 | true  | false | true  | true  | true  |
| 2 | false | true  | false | true  | false |
| 3 | false | true  | true  | false | true  |
| 4 | true  | true  | false | true  | false |

In Swift, you can use a list of lists (2D array) to represent the matrix:

```swift
[
    [false, true, false, false, true],
    [true, false, true, true, true],
    [false, true, false, true, false],
    [false, true, true, false, true],
    [true, true, false, true, false]
]
```

In any cell where `true` is found, the corresponding vertices are connected by an edge.

#### Code Example

```swift
final class Graph {

    private(set) var graph: [[Bool]]

    init(numVertices: Int) {
        self.graph = Array(
            repeating: Array(
                repeating: false,
                count: numVertices
            ),
            count: numVertices
        )
    }

    func addEdge(u: Int, v: Int) {
        graph[u][v] = true
        graph[v][u] = true
    }

}
```

The `addEdge` method takes `u` and `v` vertices and adds an edge between them by setting the corresponding cells to `true`. There are two cells in the matrix for each pair of vertices. For example, (0, 1) corresponds to these cells:

|   | 0    | 1    | 2    | 3    | 4    |
|---|------|------|------|------|------|
| 0 | false | true  | false | false | false |
| 1 | true  | false | false | false | false |
| 2 | false | false | false | false | false |
| 3 | false | false | false | false | false |
| 4 | false | false | false | false | false |


#### Thank you for reading! 😊
