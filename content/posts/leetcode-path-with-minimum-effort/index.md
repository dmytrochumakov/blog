+++
title = "LeetCode - Path With Minimum Effort"
date = 2026-10-09T18:24:59+03:00
tags = ["LeetCode", "Path With Minimum Effort", "Advanced_Graphs", "Medium", "Swift"]
draft = false
+++

### The problem

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., 0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

Example 1:
![alt image](images/ex1.png#center)

```
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
```

Example 2:
![alt image](images/ex2.png#center)

```
Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1
Explanation: The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than the route of [1,3,5,3,5].
```

Example 3:
![alt image](images/ex3.png#center)

```
Input: heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
Output: 0
Explanation: This route does not require any effort.
```

Constraints:

- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 10^6`

#### Explanation

When the problem asks us to find a minimum effort or shortest path, it usually implies graph algorithms.

If we tried to rephrase **route effort**, it would be a sequence of adjacent cells that you have to visit to get from point A - (0, 0) to point B - (rows - 1, cols - 1).

In our case, we have a grid where we can move in four directions while not forgetting about out-of-bounds edge cases.

As an analogy, we can consider **route effort** as a weight, which suggests that we can apply **Dijkstra's (shortest path)** algorithm.

Dijkstra's algorithm is, in some sense, similar to the BFS algorithm, but instead of using a usual queue, it uses a priority queue (Heap).

So we are going to store the **maximum absolute difference** and prioritize the shortest path using a Min Heap, and we are going to look for the **minimum effort** with the **maximum difference** in heights.

![alt image](images/1631.png#center)

Our next goal is to manage base cases when we have already visited a cell and when we have reached the target (rows - 1, cols - 1).

To find the **maximum absolute difference**, we are going to check the current cell's neighbors in the **top, left, right, and bottom** directions.

The result of the shortest path algorithm will be the **minimum effort** to travel from the top-left to the bottom-right cell.

The time complexity will be `O(R * C * log(R * C))`, where the additional `log(R * C)` comes from the Min Heap.

### Solution

#### Code

```swift
struct Helper: Comparable {
    let diff: Int
    let r: Int
    let c: Int

    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.diff < rhs.diff
    }
}

func minimumEffortPath(_ heights: [[Int]]) -> Int {
    let rows = heights.count
    let cols = heights[0].count
    var visited: Set<[Int]> = []

    func shortestPath() -> Int {
        var pq: Heap<Helper> = [Helper(diff: 0, r: 0, c: 0)]
        let directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

        while !pq.isEmpty {
            let helper = pq.removeMin()

            if visited.contains([helper.r, helper.c]) {
                continue
            }

            visited.insert([helper.r, helper.c])

            if helper.r == rows - 1 && helper.c == cols - 1 {
                return helper.diff
            }

            for direction in directions {
                let nr = helper.r + direction.0
                let nc = helper.c + direction.1

                if nr == rows || nc == cols || nr < 0 || nc < 0 || visited.contains([nr, nc]) {
                    continue
                }

                let newDiff = max(helper.diff, abs(heights[helper.r][helper.c] - heights[nr][nc]))
                pq.insert(Helper(diff: newDiff, r: nr, c: nc))
            }
        }

        return -1
    }

    return shortestPath()
}
```

#### Time/ Space complexity

- Time complexity: O(R \* C \* log(R \* C))
- Space complexity: O(R \* C)
- Where `R` is the number of rows and `C` is the number of columns

#### Thank you for reading! 😊
