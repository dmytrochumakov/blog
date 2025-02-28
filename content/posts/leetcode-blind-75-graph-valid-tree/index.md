+++
title = "LeetCode - Blind 75 - Graph Valid Tree"
date = 2025-02-28T07:24:12+03:00
tags = ["LeetCode", "Blind 75", "Graph Valid Tree", "Swift"]
draft = false
+++

### The problem  
You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer `n` and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.  

Return `true` if the edges of the given graph make up a valid tree, and `false` otherwise.  

#### Examples  
![alt image](images/tree1-graph.jpg#center)  
``` 
Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
```  

![alt image](images/tree2-graph.jpg#center)  
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
Output: false
```  

#### Constraints  
* 1 <= n <= 2000  
* 0 <= edges.length <= 5000  
* edges[i].length == 2  
* 0 <= ai, bi < n  
* ai != bi  
* There are no self-loops or repeated edges.  

### Depth First Search Solution  
```swift  
func validTree(_ n: Int, _ edges: [[Int]]) -> Bool {
    if n == 0 {
        return true
    }

    var adj: [Int: [Int]] = [:]
    for i in 0 ..< n {
        adj[i] = []
    }

    for value in edges {
        let u = value[0]
        let v = value[1]
        adj[u]!.append(v)
        adj[v]!.append(u)
    }

    var visit: Set<Int> = []

    func dfs(_ i: Int, _ prev: Int) -> Bool {
        if visit.contains(i) {
            return false
        }

        visit.insert(i)

        for j in adj[i]! {
            if j == prev {
                continue
            }
            if !dfs(j, i) {
                return false
            }
        }

        return true
    }

    return dfs(0, -1) && n == visit.count
}
```  

#### Explanation  
From reading the description, we learn that we need to validate a graph tree and check if it is valid. However, the description does not define what a valid tree is. Let's start there.  
- A valid tree cannot have loops (cycles), for example:  
  ![alt image](images/p-261.png#center)  
- A tree needs to be connected; if it is not, it is an invalid tree, for example:  
  ![alt image](images/p-261-1.png#center)  
- An empty graph counts as a valid tree.  

We take the input nodes and edges and check if they are connected and if they do not form a loop (cycle). To do that, we need to create an adjacency list for every single input node.  

We use the DFS algorithm and visit the neighbors of every single node recursively. If no cycle is detected and the length of the `visit` set equals `n`, this means the graph is a valid tree, and we return `true`; otherwise, we return `false`. To avoid false positive results when backtracking, we add a `prev` property that tracks the previous value, compares it to the current index, and skips it if they are equal.  

##### Time/Space Complexity  
* Time complexity: O(V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of vertices and `E` is the number of edges.  

### Breadth First Search Solution  
```swift  
func validTree(_ n: Int, _ edges: [[Int]]) -> Bool {
    if n == 0 {
        return true
    }

    var adj: [Int: [Int]] = [:]
    for i in 0 ..< n {
        adj[i] = []
    }

    for value in edges {
        let u = value[0]
        let v = value[1]
        adj[u]!.append(v)
        adj[v]!.append(u)
    }

    var visit: Set<Int> = []
    var q: [(Int, Int)] = [(0, -1)]

    visit.insert(0)

    while !q.isEmpty {
        let value = q.removeFirst()
        let node = value.0
        let parent = value.1
        for nei in adj[node]! {
            if nei == parent {
                continue
            }
            if visit.contains(nei) {
                return false
            }
            visit.insert(nei)
            q.append((nei, node))
        }
    }

    return visit.count == n
}
```  

#### Explanation  
We can also solve this problem using the Breadth First Search algorithm. Instead of recursion, we use iteration. The logic behind the solution remains the same: we create an adjacency list, iterate over every single node and its neighbors, and update our `visit` set.  

##### Time/Space Complexity  
* Time complexity: O(V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of vertices and `E` is the number of edges.  

#### Thank you for reading! 😊
