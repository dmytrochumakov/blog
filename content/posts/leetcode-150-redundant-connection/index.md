+++
title = "LeetCode - 150 - Redundant Connection"
date = 2025-11-05T08:01:51+03:00
tags = ["LeetCode", "150", "Redundant Connection", "Swift"]
draft = false
+++

### The problem

In this problem, a tree is an undirected graph that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1` to `n`, with one additional edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the graph.

Return an edge that can be removed so that the resulting graph is a tree of `n` nodes. If there are multiple answers, return the answer that occurs last in the input.

#### Examples

![alt image](images/reduntant1-1-graph.jpg#center)

```
Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]
```

![alt image](images/reduntant1-2-graph.jpg#center)

```
Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
Output: [1,4]
```

#### Constraints

* n == edges.length
* 3 <= n <= 1000
* edges[i].length == 2
* 1 <= ai < bi <= edges.length
* ai != bi
* There are no repeated edges.
* The given graph is connected.

#### Explanation

From the description of the problem, we learn that we are given an undirected graph. We also learn that the given graph does not have any cycles, and we want to remove an edge such that the resulting graph is going to be a tree, and if there are multiple edges that could be removed, we would return the last one.

Let's look at the first example:
The definition of a tree says that it must be connected and should not have any cycles.

If we remove edge `[2, 3]`, we will get a valid tree.
![alt image](images/684.png#center)

You can see that our given graph forms a cycle, and removing an edge helps us transform it into a tree, so now we can say that our problem is about cycle detection.

One way to detect a cycle is by using a depth-first search algorithm with an adjacency list, but in this problem, we will be using Union-Find (Disjoint Set) because it’s more efficient.

### Union-Find (Disjoint Set) Solution

```swift
func findRedundantConnection(_ edges: [[Int]]) -> [Int] {
    let n = edges.count
    var par = Array(0 ... n)
    var rank = Array(repeating: 1, count: n + 1)

    func find(_ n: Int) -> Int{
        if n != par[n] {
            par[n] = find(par[n])
        }
        return par[n]
    }

    func union(_ n1: Int, _ n2: Int) -> Bool {
        let p1 = find(n1)
        let p2 = find(n2)
        if p1 == p2 {
            return false
        }
        if rank[p1] > rank[p2] {
            par[p2] = p1
            rank[p1] += rank[p2]
        } else {
            par[p1] = p2
            rank[p2] += rank[p1]
        }
        return true
    }

    for edge in edges {
        let n1 = edge[0]
        let n2 = edge[1]
        if union(n1, n2) == false {
            return [n1, n2]
        }
    }

    return []
}
```

#### Explanation

To better understand how Union-Find works, let’s look at another example:
![alt image](images/684-1.png#center)
From the description, we know that we have nodes from `1...n`. When we start with the assumption that we have a tree, this means that when we have `n` nodes, we must have `n - 1` edges, because if we introduce one more edge in the graph (assuming we can’t have duplicates), this is going to result in a cycle.
![alt image](images/684-2.png#center)
We can conclude that in order for a graph to be a tree, the number of edges must be equal to the number of nodes.

Now, let’s look at how we can use the Union-Find algorithm on the second example:
![alt image](images/684-3.png#center)
The Union-Find will connect two components via the edge:

* We are going to go edge by edge and keep connecting two nodes that the edge belongs to.
  ![alt image](images/684-4.png#center)
* When we get to edge `[1, 4]`, we are not going to connect again because nodes `1` and `4` are already connected.

Union-Find can tell us that nodes `1` and `4` belong to the same component before we introduce edge `[1, 4]`, and since we know that edges are undirected, we can detect a cycle and return that node as a result.

Notes about how the Union-Find algorithm works:
![alt image](images/684-5.png#center)

* We are going to go edge by edge.
* Next, we are going to connect nodes `1` and `2`, because we can use node `1` as the parent of node `2`, and the rest of the nodes are connected to themselves.
* Next, we are going to find the parent of `3` by going to node `2` and traversing the tree up to find the root parent, and we are going to connect the smaller component with node `3`.
* Next, we are going to repeat the previous step for node `4` and connect with root parent node `1`.
* Lastly, we are going to connect edge `[1, 4]` and find the root node for node `4` that will be node `1`. This will help us detect a cycle because nodes `1` and `4` have the same root; therefore, we now know that edge `[1, 4]` will introduce the cycle, and this is the edge that we want to return in our result.

#### Time/Space complexity

* Time complexity: O(V + (E * α(V)))
* Space complexity: O(V)
* Where `V` is the number of vertices and `E` is the number of edges in the graph. `α()` is used for amortized complexity.

#### Thank you for reading! 😊
