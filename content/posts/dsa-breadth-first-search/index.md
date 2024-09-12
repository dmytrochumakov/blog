+++
title = 'DSA - Breadth First Search'
date = 2024-09-12T07:14:47+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Breadth First Search", "BFS", "Swift"]
draft = false
+++

### What is Breadth First Search?
[Breadth-first search (BFS)](https://en.wikipedia.org/wiki/Breadth-first_search) is an algorithm for traversing tree or graph data structures. It starts at the root and explores all the neighboring nodes at the current depth before moving on to nodes at the next depth level.

![alt image](images/Breadth-first-tree.png#center)  
[Source](https://upload.wikimedia.org/wikipedia/commons/3/33/Breadth-first-tree.svg)

### Implementation
The implementation of BFS may vary depending on the problem. The main idea of BFS is:
- It has a `visited` array that collects all elements that have already been visited.
- It has a `queue` with all elements it's going to visit.
- It loops through the `queue`, removes the first element, and appends it to the `visited` list.
- Finally, it loops through all the `neighbors` and appends the neighbor to the `queue` if it has not been visited and is not already in the queue.

#### Code Example
```swift
func bfs(_ value: String) -> [String] {
    let graph: [String: [String]] = [
        "New York": ["Buenos Aires", "Cairo", "Tokyo", "London"]
    ]

    var visited: [String] = []
    var queue: [String] = []

    queue.append(value)

    while !queue.isEmpty {
        let tmp = queue.removeFirst()
        visited.append(tmp)
        
        if let neighbors = graph[tmp] {
            for neighbor in neighbors.sorted() {
                if !visited.contains(neighbor) && !queue.contains(neighbor) {
                    queue.append(neighbor)
                }
            }
        }
    }

    return visited
}
```

#### Thank you for reading! 😊
