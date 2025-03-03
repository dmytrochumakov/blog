+++
title = "LeetCode - Blind 75 - Number of Connected Components in an Undirected Graph"
date = 2025-03-03T07:39:10+03:00
tags = ["LeetCode", "Blind 75", "Number of Connected Components in an Undirected Graph", "Swift"]
draft = false
+++

### The Problem  
You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.  

Return the number of connected components in the graph.  

#### Examples  

![alt image](images/conn1-graph.jpg#center)  
``` 
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```
  
![alt image](images/conn2-graph.jpg#center)  
```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

#### Constraints  
* 1 <= n <= 2000  
* 1 <= edges.length <= 5000  
* edges[i].length == 2  
* 0 <= ai, bi < n  
* ai != bi  
* There are no repeated edges.  

### Depth-First Search Solution  
```swift
func countComponents(_ n: Int, _ edges: [[Int]]) -> Int {
    var adj: [Int: [Int]] = [:]
    for i in 0 ..< n {
        adj[i] = []
    }
    var visit = Array(repeating: false, count: n)

    for value in edges {
        let u = value[0]
        let v = value[1]
        adj[u]!.append(v)
        adj[v]!.append(u)
    }

    func dfs(_ node: Int) {
        for nei in adj[node]! {
            if !visit[nei] {
                visit[nei] = true
                dfs(nei)
            }
        }
    }

    var res = 0
    for node in 0 ..< n {
        if !visit[node] {
            visit[node] = true
            dfs(node)
            res += 1
        }
    }

    return res
}
```

#### Explanation  
From the description, we learn that we need to find the number of connected components in a graph. Let's clarify what a connected component means:  

- If we are given two nodes, for example, `3` and `4`, without edges, then we just return the number of nodes given. In this case, we have two nodes, so we return `2` because:  
  - One node by itself counts as a connected component.  

To solve this problem, we are going to:  
1. Create an adjacency list.  
2. Traverse every single node using the DFS algorithm.  
3. Mark nodes as visited.  
4. Count the number of connected components by the number of DFS calls.  

##### Time/Space Complexity  
* Time complexity: O(V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of vertices and `E` is the number of edges.  

---

### Union-Find Solution  
```swift
final class DSU {
    private var parent: [Int]
    private var rank: [Int]

    init(_ n: Int) {
        self.parent = Array(0 ..< n)
        self.rank = Array(repeating: 1, count: n)
    }

    func find(_ node: Int) -> Int {
        var cur = node
        while cur != self.parent[cur] {
            self.parent[cur] = self.parent[self.parent[cur]]
            cur = self.parent[cur]
        }
        return cur
    }

    func union(_ u: Int, _ v: Int) -> Bool {
        var pu = self.find(u)
        var pv = self.find(v)
        if pu == pv {
            return false
        }
        if self.rank[pv] > self.rank[pu] {
            let tmp = pu
            pu = pv
            pv = tmp
        }
        self.parent[pv] = pu
        self.rank[pu] += self.rank[pv]
        return true
    }
}

func countComponents(_ n: Int, _ edges: [[Int]]) -> Int {
    let dsu = DSU(n)
    var res = n

    for value in edges {
        let u = value[0]
        let v = value[1]
        if dsu.union(u, v) {
            res -= 1
        }
    }

    return res
}
```  

#### Explanation  
We can also solve this problem using the Union-Find algorithm. This algorithm was literally designed for problems like this—it helps find connected components and identify disjoint sets.  

We maintain two arrays:  
- `parent`, where each node initially points to itself. For example, `[0,1,2,3,4]` means each node is its own parent.  
- `rank`, which helps track the size of each connected component.  

Union-Find works like a forest of trees. Initially, we have `n` trees (one for each node). As we traverse through each edge, we:  
1. Connect the nodes (merge them).  
2. Decrease the number of connected components by `1`.  

![alt image](images/p-323.png#center)  

We add one small optimization by maintaining the `rank`, which helps keep track of the size of connected components. As we continue connecting nodes, we always attach a smaller tree to the root of a larger tree to minimize tree height.  

For example, if we connect `[1,2]`, we make `0` the root and add `2` as a child of `0`. This means the `rank` of `0` increases to `3`, and the `parent` array changes from `[0,1,2,3,4]` to `[0,0,0,3,4]`.  

![alt image](images/p-323-1.png#center)  

Once we process edges `[3,4]`, the `rank` of `3` increases to `2`, and the parent array updates to `[0,0,0,3,3]`. Each time we perform a union operation, we decrement the number of connected components by `1`.  

If we encounter an edge like `[0,2]`, we immediately return before making changes because they already share the same parent.  

![alt image](images/p-323-2.png#center)  

##### Time/Space Complexity  
* Time complexity: O(V + (E * α(V)))  
* Space complexity: O(V)  
* Where `V` is the number of vertices, `E` is the number of edges, and `α(V)` is the inverse Ackermann function (which is nearly constant in practical use).  

#### Thank you for reading! 😊
