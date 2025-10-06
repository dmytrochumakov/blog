+++
title = "LeetCode - 150 - Max Area of Island"
date = 2025-10-06T07:48:31+03:00
tags = ["LeetCode", "150", "Max Area of Island", "Swift"]
draft = false
+++

### The problem

You are given an `m x n` binary matrix `grid`. An island is a group of `1`s (representing land) connected 4-directionally (horizontally or vertically). You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value `1` in the island.

Return the maximum area of an island in `grid`. If there is no island, return `0`.

#### Examples

![alt image](images/maxarea1-grid.jpg#center)

```
Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]  
Output: 6  
Explanation: The answer is not 11, because the island must be connected 4-directionally.  
```

```
Input: grid = [[0,0,0,0,0,0,0,0]]  
Output: 0  
```

#### Constraints

* m == grid.length
* n == grid[i].length
* 1 <= m, n <= 50
* grid[i][j] is either 0 or 1.

#### Explanation

From the description of this problem, we learn that we need to figure out the max area of islands. We also know that the value `0` represents water and the value `1` represents land.

We also know that an island is the consecutive `1` values that are connected either horizontally or vertically. Two cells that are connected diagonally don’t count.
![alt image](images/655.png#center)

We can solve this problem in two steps:

* First, we need to figure out the area of every island
* Second, we need to find the max area

### Depth First Search Solution

```swift
func maxAreaOfIsland(_ grid: [[Int]]) -> Int {
    let rows = grid.count
    let cols = grid[0].count
    var visited: Set<[Int]> = []

    func dfs(_ r: Int, _ c: Int) -> Int {
        if (r < 0 || r == rows || c < 0 || c == cols || grid[r][c] == 0 || visited.contains([r, c])) {
            return 0
        }
        visited.insert([r, c])
        return (1 + dfs(r + 1, c) + dfs(r - 1, c) + dfs(r, c + 1) + dfs(r, c - 1))
    }

    var res = 0
    for r in 0 ..< rows {
        for c in 0 ..< cols {
            res = max(res, dfs(r, c))
        }
    }

    return res
}
```

#### Explanation

To solve this problem, we will be using the depth-first search algorithm.
![alt image](images/655-1.png#center)

* We will be visiting four directions `up, left, right, down` and verifying that the current cell is land.
* We are also going to have a `visited` set that will help us avoid processing cells that we already visited.
* After processing an island, we will be returning its area.
* Lastly, we will continue to run the DFS algorithm on every single island and keep track of which island has the maximum area.

#### Time/Space complexity

* Time complexity: O(m * n)
* Space complexity: O(m * n)
* Where `m` is the number of rows and `n` is the number of columns in the grid.
    
#### Thank you for reading! 😊
