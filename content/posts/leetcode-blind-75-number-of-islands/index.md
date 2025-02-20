+++
title = "LeetCode - Blind 75 - Number of Islands"
date = 2025-02-20T12:55:16+03:00
tags = ["LeetCode", "Blind 75", "Number of Islands", "Swift"]
draft = false
+++

### The Problem  
Given an `m x n` 2D binary grid `grid`, which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.  

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are surrounded by water.  

#### Examples  

```  
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```  

```  
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```  

#### Constraints  
* `m == grid.length`  
* `n == grid[i].length`  
* `1 <= m, n <= 300`  
* `grid[i][j]` is `'0'` or `'1'`.  

---

### Depth-First Search Solution  

```swift  
func numIslands(_ grid: [[Character]]) -> Int {
    var grid = grid
    let ROWS = grid.count
    let COLS = grid[0].count
    let directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
    var islands = 0

    func dfs(_ r: Int, _ c: Int) {
        if (r < 0 || c < 0 || r >= ROWS ||
            c >= COLS || grid[r][c] == "0"
        ) {
            return
        }

        grid[r][c] = "0"

        for direction in directions {
            let dr = direction[0]
            let dc = direction[1]
            dfs(r + dr, c + dc)
        }
    }

    for r in 0 ..< ROWS {
        for c in 0 ..< COLS {
            if grid[r][c] == "1" {
                dfs(r, c)
                islands += 1
            }
        }
    }

    return islands
}
```  

#### Explanation  
From the description of the problem, we can see that we have `islands` formed by connecting adjacent land horizontally or vertically. If we draw a visual representation, we can find islands that are connected vertically or horizontally and surrounded by water.  

![alt image](images/p-200.png#center)  

When the problem has adjacent elements, this means that it is a graph problem, and we have two common ways to solve it: using DFS or BFS algorithms.  

To do that, we need to visit neighboring cells and check if they are part of the island (have `"1"`).  

The function `dfs` will help us check for an out-of-bounds case and if we have already visited this cell. After that, we will mark the current cell as `"0"`, meaning that we have visited this cell, and recursively call `dfs` to search neighbors in four directions.  

##### Time/Space Complexity  
* **Time complexity:** O(m*n)  
* **Space complexity:** O(m*n)  
* Where `m` is the number of rows and `n` is the number of columns in the `grid`.  

---

### Breadth-First Search Solution  

```swift  
func numIslands(_ grid: [[Character]]) -> Int {
    let ROWS = grid.count
    let COLS = grid[0].count
    var visit: Set<[Int]> = []
    var islands = 0

    func bfs(_ r: Int, _ c: Int) {
        var q: [[Int]] = []
        visit.insert([r, c])
        q.append([r, c])

        while !q.isEmpty {
            let val = q.removeFirst()
            let row = val[0]
            let col = val[1]
            let directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
            for direction in directions {
                let r = row + direction[0]
                let c = col + direction[1]
                if ((0 ..< ROWS).contains(r) &&
                    (0 ..< COLS).contains(c) &&
                    grid[r][c] == "1" &&
                    !visit.contains([r, c])
                ) {
                    q.append([r, c])
                    visit.insert([r, c])
                }
            }
        }
    }

    for r in 0 ..< ROWS {
        for c in 0 ..< COLS {
            if grid[r][c] == "1" && !visit.contains([r, c]) {
                bfs(r, c)
                islands += 1
            }
        }
    }

    return islands
}
```  

#### Explanation  
We can solve this problem using the BFS algorithm. The idea behind it is that we add an additional `visit` set and move layer by layer, starting from position `(0,0)`.  

![alt image](images/p-200-1.png#center)  

The BFS algorithm is slightly different from DFS because it is iterative and uses a queue data structure. However, the logic remains the same: we visit neighboring cells in four directions only if 
- we are not out of bounds
- the cell is equal to `"1"`
- and we have not visited the cell before.  

##### Time/Space Complexity  
* **Time complexity:** O(m*n)  
* **Space complexity:** O(m*n)  
* Where `m` is the number of rows and `n` is the number of columns in the `grid`.  

#### Thank you for reading! 😊
