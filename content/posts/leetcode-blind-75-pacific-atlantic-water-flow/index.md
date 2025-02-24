+++
title = "LeetCode - Blind 75 - Pacific Atlantic Water Flow"
date = 2025-02-24T12:50:30+03:00
tags = ["LeetCode", "Blind 75", "Pacific Atlantic Water Flow", "Swift"]
draft = false
+++

### The problem  
There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.  

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.  

The island receives a lot of rain, and the rainwater can flow to neighboring cells directly **north, south, east, and west** if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.  

Return a **2D list** of grid coordinates `result` where `result[i] = [ri, ci]` denotes that rainwater can flow from cell `(ri, ci)` to **both** the Pacific and Atlantic oceans.  

### Examples  
![alt image](images/waterflow-grid.jpg#center)  

```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean  
       [0,4] -> Atlantic Ocean  
[1,3]: [1,3] -> [0,3] -> Pacific Ocean  
       [1,3] -> [1,4] -> Atlantic Ocean  
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean  
       [1,4] -> Atlantic Ocean  
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean  
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean  
[3,0]: [3,0] -> Pacific Ocean  
       [3,0] -> [4,0] -> Atlantic Ocean  
[3,1]: [3,1] -> [3,0] -> Pacific Ocean  
       [3,1] -> [4,1] -> Atlantic Ocean  
[4,0]: [4,0] -> Pacific Ocean  
       [4,0] -> Atlantic Ocean  
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.
```

```
Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
```

### Constraints  
* `m == heights.length`  
* `n == heights[r].length`  
* `1 <= m, n <= 200`  
* `0 <= heights[r][c] <= 10^5`  

---

## Depth-First Search (DFS) Solution  

```swift
func pacificAtlantic(_ heights: [[Int]]) -> [[Int]] {
    let ROWS = heights.count
    let COLS = heights[0].count

    var pacific: Set<[Int]> = []
    var atlantic: Set<[Int]> = []

    func dfs(
        _ r: Int,
        _ c: Int,
        _ visit: inout Set<[Int]>,
        _ prevHeight: Int
    ) {
        if (visit.contains([r, c]) ||
            r < 0 || c < 0 || r >= ROWS || c >= COLS ||
            heights[r][c] < prevHeight
        ) {
            return
        }

        visit.insert([r, c])

        dfs(r + 1, c, &visit, heights[r][c])
        dfs(r - 1, c, &visit, heights[r][c])
        dfs(r, c + 1, &visit, heights[r][c])
        dfs(r, c - 1, &visit, heights[r][c])
    }

    for c in 0 ..< COLS {
        dfs(0, c, &pacific, heights[0][c])
        dfs(ROWS - 1, c, &atlantic, heights[ROWS - 1][c])
    }

    for r in 0 ..< ROWS {
        dfs(r, 0, &pacific, heights[r][0])
        dfs(r, COLS - 1, &atlantic, heights[r, COLS - 1])
    }

    var res: [[Int]] = []

    for r in 0 ..< ROWS {
        for c in 0 ..< COLS {
            if pacific.contains([r, c]) && atlantic.contains([r, c]) {
                res.append([r, c])
            }
        }
    }

    return res
}
```

### Explanation  
From the **description** of the problem, we learned that we need to find the cells that can reach **both** the **Atlantic and Pacific Oceans**. This can happen **only** if an adjacent cell has a **lower or equal** value than the current cell. The water can flow in **four directions** (above, left, right, and below).  

For example, with input:  
`heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]`  
![alt image](images/p-417.png#center)  

- `5 > 3`, `5 > 1`, and `5 > 4`  
- The `3` to the right of `5` has a neighbor of `1` and follows the condition `3 > 1`, meaning we can reach the **Atlantic Ocean**.  
- The `4` to the left of `5` has a neighbor of `2`, and also follows the condition `4 > 2`, meaning we can reach the **Pacific Ocean**.  
- As a result, we include the position of `5` in our result.  

One way to solve this problem is to **find every cell** that borders the **Pacific Ocean** (everything in the **first row** and **first column**). Similarly, we do the same for the **Atlantic Ocean** (everything in the **last row** and **last column**). We use **graph traversal** with **DFS** to find which positions can reach both oceans.  
![alt image](images/p-417-1.png#center)  

The first thing we do is iterate through the **first row** (Pacific) and run **DFS** to find all nodes that can reach the Pacific Ocean. We do the same for the **bottom row** (Atlantic). We maintain these values in a **set** to **avoid duplicates**.  

### Time/Space Complexity  
* **Time Complexity:** `O(m * n)`  
* **Space Complexity:** `O(m * n)`  
* Where `m` is the number of **rows**, and `n` is the number of **columns**.  

---

## Breadth-First Search (BFS) Solution  

```swift
func pacificAtlantic(_ heights: [[Int]]) -> [[Int]] {
    let ROWS = heights.count
    let COLS = heights[0].count
    let directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    var pac = Array(repeating: Array(repeating: false, count: COLS), count: ROWS)
    var atl = Array(repeating: Array(repeating: false, count: COLS), count: ROWS)

    func bfs(_ sources: [(Int, Int)], _ ocean: inout [[Bool]]) {
        var q = sources
        while !q.isEmpty {
            let (r, c) = q.removeFirst()
            ocean[r][c] = true

            for (dr, dc) in directions {
                let nr = r + dr
                let nc = c + dc
                if nr >= 0, nr < ROWS, nc >= 0, nc < COLS, !ocean[nr][nc], heights[nr][nc] >= heights[r][c] {
                    q.append((nr, nc))
                }
            }
        }
    }
}
```

### Explanation  
We can also solve this problem using **BFS** instead of **DFS**. The logic stays the same, but we use an **iterative approach** instead of recursion.  

Time/Space Complexity remains **O(m * n)**.

#### Thank you for reading! 😊
