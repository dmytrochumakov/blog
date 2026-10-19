+++
title = "LeetCode - Minimum Path Sum"
date = 2026-10-19T21:33:54+03:00
tags = ["LeetCode", "Minimum Path Sum", "2D_Dynamic_Programming", "Medium", "Swift"]
draft = false
+++

### The problem

Given an `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example 1:**

![alt image](images/minpath.jpg#center)

```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.

```

**Example 2:**

```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`

#### Explanation

When we are talking about the min path (or shortest path) with non-negative input, the first thought that comes to my mind is a Dijkstra algorithm.

In our case, it's a textbook shortest path algorithm with the smallest change: we are using the sum of all numbers as a value to compare in the Min Heap.

We are using the Dijkstra algorithm because we need to find the shortest path from the top-left to the bottom-right cell with the minimum sum, and the algorithm does exactly that. The underlying Min Heap helps minimize the sum while moving along the path.

![alt image](images/64.png#center)

The time complexity for this algorithm is `O(ROWS*COLS*log(ROWS*COLS))`, and the space complexity is `O(ROWS*COLS)`.

```swift
struct Helper: Comparable {
    let sumOfAllNumbers: Int
    let r: Int
    let c: Int
    
    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.sumOfAllNumbers < rhs.sumOfAllNumbers
    }
}

func minPathSum(_ grid: [[Int]]) -> Int {
    let rows = grid.count
    let cols = grid[0].count
    
    func shortestPath() -> Int {
        let directions = [(0, 1), (1, 0)]
        var pq: Heap<Helper> = [Helper(sumOfAllNumbers: grid[0][0], r: 0, c: 0)]
        var visited: Set<[Int]> = []
        
        while !pq.isEmpty {
            let helper = pq.removeMin()
            
            if helper.r == rows - 1 && helper.c == cols - 1 {
                return helper.sumOfAllNumbers
            }
            
            if visited.contains([helper.r, helper.c]) {
                continue
            }
            
            visited.insert([helper.r, helper.c])
            
            for direction in directions {
                let nr = direction.0 + helper.r
                let nc = direction.1 + helper.c
                if nr == rows || nc == cols || visited.contains([nr, nc]) {
                    continue
                }
                pq.insert(Helper(sumOfAllNumbers: helper.sumOfAllNumbers + grid[nr][nc], r: nr, c: nc))
            }
        }
        
        return -1
    }
    
    return shortestPath()
}
```

### Dynamic Programing Solution

#### Explanation

We can reduce the time complexity to O(m*n) by using Dynamic Programing.

We start by initializing the `dp` array with all zeros.

Then we iterate backwards.

We skip the last cell because it is our base case.

The result of each cell is based on the value from below and the right plus current value.

If a row or column is out of bounds, we are not going to skip it. Instead, we would replace it with `Int.max`. This way, we are making sure that our calculation would work correctly when finding the minimum between two values.

After the iteration is complete, the result would be stored in the first cell.

![alt image](images/64-1.png#center)

The picture does not show the complete calculation. I would recommend you go through the calculations yourself from the last to the first cell because this way you would understand it much better.

#### Code

```swift
func minPathSum(_ grid: [[Int]]) -> Int {
    let rows = grid.count
    let cols = grid[0].count
    
    var dp = Array(repeating: Array(repeating: 0, count: cols), count: rows)
    dp[rows - 1][cols - 1] = grid[rows - 1][cols - 1]
    
    for r in stride(from: rows - 1, to: -1, by: -1) {
        for c in stride(from: cols - 1, to: -1, by: -1) {
            if r == rows - 1 && c == cols - 1 {
                continue
            }
            
            let down = r + 1 < rows ? dp[r + 1][c] : Int.max
            let right = c + 1 < cols ? dp[r][c + 1] : Int.max
            
            dp[r][c] = grid[r][c] + min(down, right)
        }
    }
    
    return dp[0][0]
}
```

#### Time/ Space complexity

* Time complexity: O(ROWS*COLS)
* Space complexity: O(ROWS*COLS)

### Dynamic Programing (Space Optimized) Solution

#### Explanation

We can go even further and optimize space complexity to the size of a column by holding only one row in memory.

The key idea stays the same, but now, instead of storing values from below separately, we can reuse and update them. This way, we are not allocating additional memory.

![alt image](images/64-2.png#center)

But we will need to make a small change when allocating the `dp` array. Instead of populating it with zeros, we will need to replace them with `Int.max` because zeros will break our calculation when we start from the last row (the minimum is always going to be 0).

With this small trick, we have a working solution.

#### Code

```swift
func minPathSum(_ grid: [[Int]]) -> Int {
    let rows = grid.count
    let cols = grid[0].count
    
    var dp = Array(repeating: Int.max, count: cols)
    dp[cols - 1] = grid[rows - 1][cols - 1]
    
    for r in stride(from: rows - 1, to: -1, by: -1) {
        for c in stride(from: cols - 1, to: -1, by: -1) {
            if r == rows - 1 && c == cols - 1 {
                continue
            }
            
            let right = c + 1 < cols ? dp[c + 1] : Int.max
            
            dp[c] = grid[r][c] + min(dp[c], right)
        }
    }
    
    return dp[0]
}
```

#### Time/ Space complexity

* Time complexity: O(ROWS*COLS)
* Space complexity: O(COLS)

#### Thank you for reading! 😊
