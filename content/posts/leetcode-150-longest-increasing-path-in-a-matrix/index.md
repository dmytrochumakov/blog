+++
title = "LeetCode - 150 - Longest Increasing Path in a Matrix"
date = 2026-03-23T08:15:06+03:00
tags = ["LeetCode", "150", "Longest Increasing Path in a Matrix", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

Given an `m x n` integer `matrix`, return *the length of the longest increasing path in* `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![alt image](images/grid1.jpg#center)

```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

**Example 2:**

![alt image](images/tmp-grid.jpg#center)

```
Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
Output: 4
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

**Example 3:**

```
Input: matrix = [[1]]
Output: 1
```

**Constraints:**

* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 200`
* `0 <= matrix[i][j] <= 2^31 - 1`

#### Explanation

I assume that you have read the description of the problem; if not, please do so and come back.
Let's look at the first example. You can see that:

![alt image](images/329.png#center)
* we can start creating a path from any position

* we must include only unique values — duplicates are not allowed
* the next value must be greater than the previous one (I know that it is obvious, but I will leave it here because sometimes I am so focused on solving the problem that I miss key details)
* we are allowed to move only in four directions **(top, left, right, bottom)**; we are not allowed to move diagonally or out of bounds

### Dynamic Programming (Top-Down) Solution

#### Explanation

When we are solving path problems, we can use the DFS algorithm to walk in different directions.

To solve this problem efficiently, we are going to create a **grid** with `matrix` dimensions where we will store our result.

![alt image](images/329-1.png#center)

If we start from the top-left corner with value `9`:

* We can't go to the top or to the left because those cells are out of bounds.
* If we try to go right, we find that the values in both cells are equal `9 == 9`, therefore we are not allowed to increase the path.
* If we try to go down, we find that the value below `6` is less than the current value `9`, therefore we can't increase our path, and as a result, we would put **one** in our result grid.

If we start from the cell at position `(0, 1)` with value `9`, we can't go in any of the four directions because:
![alt image](images/329-2.png#center)

* If we try to go up, we would be out of bounds.
* If we try to go left, the values would be equal `9 == 9`.
* If we try to go right, the value would be less than the current value `4 < 9`; the same goes for the bottom value. As a result, we would put **one** in our grid.

If we start from the next cell `(0, 2)` with value `4`:
![alt image](images/329-3.png#center)

* We are allowed to go left as `9 > 4`, and we are allowed to go down for the same reason `8 > 4`.
* Next, we are going to try to find the longest path from positions with greater values, and as a result, we are going to put a maximum value of **two** in our grid.

We are going to continue executing the algorithm until we have visited every cell, and as a result, we are going to find and return the **maximum** value from our grid.
![alt image](images/329-4.png#center)

```swift
func longestIncreasingPath(_ matrix: [[Int]]) -> Int {
    let rows = matrix.count
    let cols = matrix[0].count

    struct Key: Hashable {
        let r: Int
        let c: Int
    }

    var cache: [Key: Int] = [:]

    func dfs(_ r: Int, _ c: Int, _ prevVal: Int) -> Int {
        if (r < 0 || r == rows ||
            c < 0 || c == cols ||
            matrix[r][c] <= prevVal
        ) {
            return 0
        }

        if let cached = cache[Key(r: r, c: c)] {
            return cached
        }

        var res = 1
        res = max(res, 1 + dfs(r + 1, c, matrix[r][c]))
        res = max(res, 1 + dfs(r - 1, c, matrix[r][c]))
        res = max(res, 1 + dfs(r , c + 1, matrix[r][c]))
        res = max(res, 1 + dfs(r , c - 1, matrix[r][c]))

        cache[Key(r: r, c: c)] = res

        return res
    }

    var res = 0
    for r in 0 ..< rows {
        for c in 0 ..< cols {
            res = max(res, dfs(r, c, -1))
        }
    }

    return res
}
```

#### Time/ Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m*n)

#### Thank you for reading! 😊
