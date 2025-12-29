+++
title = "LeetCode - 150 - Min Cost to Connect All Points"
date = 2025-12-29T08:06:06+03:00
tags = ["LeetCode", "150", "Min Cost to Connect All Points", "Graph", "Swift"]
draft = false
+++

### The problem

You are given an array `points` representing integer coordinates of some points on a 2D plane, where `points[i] = [xi, yi]`.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the Manhattan distance between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

#### Examples

![alt image](images/exp1.png#center)

```
Input: points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
Output: 20
```

Explanation:
![alt image](images/exp2.png#center)
We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.

```
Input: points = [[3,12],[-2,5],[-4,1]]
Output: 18
```

#### Constraints

* `1 <= points.length <= 1000`
* `-10^6 <= xi, yi <= 10^6`
* All pairs `(xi, yi)` are distinct.

#### Explanation

From the description of the problem, we learn that we are given an array of `points` that represents coordinates. We also know that the cost of connecting two points is the **Manhattan distance**. As a result, we must return the **minimum cost to make all points connected**. This means that we need to create **all possible** edges between every `point` and calculate the **Manhattan distance** between each coordinate pair. In other words, we are going to create an **adjacency list**.

For example, we can start connecting points from node `[3, 10]`, and our edges will look like this:
![alt image](images/1584.png#center)

Another example would be if we start connecting points from node `[2, 2]`:
![alt image](images/1584-1.png#center)

### Prim's Algorithm Solution

```swift
func minCostConnectPoints(_ points: [[Int]]) -> Int {
    let N = points.count

    var adj: [Int: [[Int]]] = [:]
    for i in 0 ..< N {
        let x1 = points[i][0]
        let y1 = points[i][1]
        for j in i + 1 ..< N {
            let x2 = points[j][0]
            let y2 = points[j][1]

            let dist = abs(x1 - x2) + abs(y1 - y2)
            adj[i, default: []].append([dist, j])
            adj[j, default: []].append([dist, i])
        }
    }

    var res = 0
    var visited: Set<Int> = []
    var minHeap: Heap<Helper> = [Helper(node: 0, cost: 0)]

    while visited.count < N {
        let helper = minHeap.removeMin()
        let cost = helper.cost
        let node = helper.node

        if visited.contains(node) {
            continue
        }

        res += cost
        visited.insert(node)

        for val in adj[node, default: []] {
            let neiCost = val[0]
            let nei = val[1]
            if !visited.contains(nei) {
                minHeap.insert(Helper(node: nei, cost: neiCost))
            }
        }
    }

    return res
}

struct Helper: Comparable {
    let node: Int
    let cost: Int

    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.cost < rhs.cost
    }
}
```

#### Explanation

We are going to solve this problem using Prim's algorithm. It is similar to Dijkstra's algorithm, where you find the shortest path from a starting node to all other nodes, while Prim's algorithm uses a Minimum Spanning Tree (MST) to find the cheapest way to connect all vertices in a graph with no cycles, and it helps find the minimum total edge weight.

Above, we learned that one of the conditions of Prim's algorithm is that we must connect all vertices without any cycles. We can achieve this by using `n - 1` edges.

Next, we must satisfy the last condition of Prim's algorithm, which says that we must minimize the total weight of the edges.

Let's look more deeply at how Prim's algorithm works:

* We can choose to start at any single node in the entire graph.
* Next, we are going to perform Breadth-First Search (BFS) on the chosen node.
* We will also be using a `visited` hash set where we will put nodes that we are visiting to help us avoid any cycles.
* Lastly, we are going to use a `MinHeap` data structure that will help us track the cost from the current node to every single node in our graph. `MinHeap` will help us find the next node with the **minimum possible cost**. When we pop a node with the minimum cost, we will add the `node` to the `visited` hash set.

An example with custom input will look like this:
![alt image](images/1584-2.png#center)

Note, we are going to create an **adjacency list** for every node to track its neighbors.

Let's look at a dry run example:
![alt image](images/1584-3.png#center)

* At the first step, we are going to start from node `0` and connect it to all its neighbors.
* At the second step, we are going to **pop** the node with the **minimum cost**; in our case, it will be node `1`.
* Next, we are going to repeat the first step and connect all neighbors of node `1`.
* Next, we are going to repeat the second step and find the node with the **minimum cost** and **pop** it.
* It is possible that we add the same node multiple times. To avoid redundant work, we will use our `visited` hash set, and if we find that the node we are using has already been visited, we will skip it.
* We will continue repeating the first and second steps until we connect all our nodes in the graph.

Lastly, in order to find the **minimum cost**, we will be maintaining a `cost` property that we will update after each iteration.

#### Time/ Space complexity

* Time complexity: O(n² * log n)
* Space complexity: O(n²)

#### Thank you for reading! 😊
