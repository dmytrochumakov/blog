+++
title = "LeetCode - 150 - Network Delay Time"
date = 2025-11-28T09:02:09+03:00
tags = ["LeetCode", "150", "Network Delay Time", "Graph", "Swift"]
draft = false
+++

### The problem

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return the minimum time it takes for all the `n` nodes to receive the signal. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

#### Examples

![alt image](images/931_example_1.png#center)

```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
```

```
Input: times = [[1,2,1]], n = 2, k = 1
Output: 1
```

```
Input: times = [[1,2,1]], n = 2, k = 2
Output: -1
```

#### Constraints

* 1 <= k <= n <= 100
* 1 <= times.length <= 6000
* times[i].length == 3
* 1 <= ui, vi <= n
* ui != vi
* 0 <= wi <= 100
* All the pairs (ui, vi) are unique. (i.e., no multiple edges.)

#### Explanation

From the description of the problem, we learn that we are given `n` nodes labeled from `1` to `n`. We are also given the array `times` that represents our **directed** edges. The `times[i]` is a triple value where:

* the first value `ui` is a source node
* the second value `vi` is a target node
* the third value `wi` is the weight of the edge

Lastly, we are given node `k` that represents our **starting** point, and we want to know how long it will take the signal to reach every single node.

![alt image](images/743.png#center)
In case we have a node that is not connected to the starting node `k`, we would return `-1`, because it is impossible to send a signal to a disconnected node.

Let's look at the first example and figure out how long it will take the signal to reach every node.
![alt image](images/743-1.png#center)

* Starting from node `2`, it will take us **one** unit of time to reach node `1` because the weight of the edge is **one**.
* For node `3`, it will take **one** unit of time to get the signal because the weight of the edge is **one**.
* As for node `2`, it will take **zero** units of time to receive the signal because it's our starting point.
* Lastly, for node `4`, it will take **two** units of time to receive the signal because we started from node `2` (edge weight **one**), then went to node `3` (edge weight **one**). As a result, our path looks like this `2 -> 3 -> 4`, and the travel time is `1 + 1 = 2`.

In our output we will be returning the value `2` because it's the largest time that any node needs to receive the signal.

### Dijkstra-Shortest Path Solution

```swift
func networkDelayTime(_ times: [[Int]], _ n: Int, _ k: Int) -> Int {
    var edges: [Int : [[Int]]] = [:]
    for time in times {
        let u = time[0]
        let v = time[1]
        let w = time[2]
        edges[u, default: []].append([v, w])
    }

    var minHeap: Heap<Helper> = [Helper(w: 0, n: k)]
    var visit: Set<Int> = []
    var t = 0

    while !minHeap.isEmpty {
        let helper = minHeap.removeMin()

        if visit.contains(helper.n) {
            continue
        }

        visit.insert(helper.n)
        t = helper.w

        for edge in edges[helper.n, default: []] {
            let n2 = edge[0]
            let w2 = edge[1]
            if !visit.contains(n2) {
                minHeap.insert(Helper(w: helper.w + w2, n: n2))
            }
        }
    }

    return visit.count == n ? t : -1
}

struct Helper: Comparable {
    let w: Int
    let n: Int
    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.w < rhs.w
    }
}
```

#### Explanation

We are going to solve this problem using Dijkstra's algorithm, which underneath uses the Breadth First Search algorithm with a queue that uses a heap. In our case, we will be using a Min Heap because we want to find the shortest path that allows the signal to reach nodes first.

Let's look at the example:
![alt image](images/743-2.png#center)

* The first step in solving this problem is to create an adjacency list that we will use to map a node to its neighbor nodes. We will need the adjacency list later in our Dijkstra algorithm.

We are also going to use a Min Heap as a priority queue that will have two properties — `path` and `node`. The `path` is the distance that it will take to reach the `node`.

![alt image](images/743-3.png#center)

After all preparation steps, we are going to start Dijkstra's algorithm from node `1`, and we will add it to our Min Heap with `path = 0` because it does not cost us anything to get there.
  ![alt image](images/743-4.png#center)
* Next, we are going to pop `node = 1` and add its neighbors `node = 3` and `node = 2` to our Min Heap with `path` equal to `1` and `4` accordingly.
  ![alt image](images/743-5.png#center)
* Next, we are going to pop `node = 3` because it has the shortest `path = 1` (`1 < 4`). We are also going to add neighbor `node = 4` to our heap. For the `path`, we are going to take the total it took us to reach `node = 3` plus the `path from node = 3 to node = 4` for a total `1 -> 3 -> 4` equals `1 + 1 = 2`
  ![alt image](images/743-6.png#center)
* Next, we are going to pop `node = 4` because it has the shortest `path = 2`. You can see that `node = 4` has neighbor `node = 2`, and it has already been added to our Min Heap, so we are going to calculate the distance from `node = 4` to `node = 2`, which equals `1 -> 3 -> 4 -> 2` (`1 + 1 + 1 = 3`), and add it to our Min Heap.
  ![alt image](images/743-7.png#center)
* Lastly, we are going to pop `node = 2` with `path = 3` because it has the minimum path, and we will return it as the result because we visited all of our nodes and found the minimum time needed to send the signal.

#### Time/ Space complexity

* Time complexity: O(E*logV)
* Space complexity: O(V + E)
* Where `E` is the number of edges and `V` is the number of vertices in the graph.

#### Thank you for reading! 😊
