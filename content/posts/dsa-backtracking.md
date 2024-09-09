+++
title = 'DSA - Backtracking'
date = 2024-09-09T07:19:46+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Backtracking", "DFS", "Swift"]
draft = false
+++

### What is Backtracking?

[Backtracking](https://en.wikipedia.org/wiki/Backtracking) is a class of algorithms for finding solutions to complex problems. A backtracking algorithm uses [recursion](https://en.wikipedia.org/wiki/Recursion_(computer_science)) and is based on [depth-first search](https://en.wikipedia.org/wiki/Depth-first_search) (DFS).

### Depth First Search (DFS)

Depth First Search (DFS) is an essential part of backtracking. DFS is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at the root node (or an arbitrary node in the case of a graph) and explores as far as possible along each branch before backtracking.

#### Code Example

The implementation of the DFS algorithm for a graph looks like this:

```swift
func dfs(_ r: Int, _ c: Int, _ visited: inout [[Int]]) {
    guard r >= 0, r < visited.count, c >= 0, c < visited[0].count, visited[r][c] == 0 else {
        return
    }

    visited[r][c] = 1

    dfs(r - 1, c, &visited)
    dfs(r + 1, c, &visited)
    dfs(r, c - 1, &visited)
    dfs(r, c + 1, &visited)
}
```

It can vary depending on the problem, but the main idea is that it first checks if the `r` and `c` parameters are within bounds and if the value has not been `visited` before. After that, it recursively walks between the top, bottom, left, and right neighboring cells.

#### Thank you for reading! 😊
