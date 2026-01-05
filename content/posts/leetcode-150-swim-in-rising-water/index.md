+++
title = "LeetCode - 150 - Swim in Rising Water"
date = 2026-01-05T14:13:59+03:00
tags = ["LeetCode", "150", "Swim in Rising Water", "Graph", "Swift"]
draft = false
+++

### The problem

You are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

It starts raining, and water gradually rises over time. At time `t`, the water level is `t`, meaning any cell with elevation less than or equal to `t` is submerged or reachable.

You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually is at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the minimum time until you can reach the bottom right square `(n - 1, n - 1)` if you start at the top left square `(0, 0)`.

#### Examples

![alt image](images/swim1-grid.jpg#center)

```
Input: grid = [[0,2],[1,3]]
Output: 3
Explanation:
At time 0, you are in grid location (0, 0).
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
You cannot reach point (1, 1) until time 3.
When the depth of water is 3, we can swim anywhere inside the grid.
```

![alt image](images/swim2-grid-1.jpg#center)

```
Input: grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
Output: 16
Explanation: The final route is shown.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.
```

#### Constraints

* `n == grid.length`
* `n == grid[i].length`
* `1 <= n <= 50`
* `0 <= grid[i][j] < n^2`
* Each value `grid[i][j]` is unique.

#### Explanation

From the description of the problem, we learn that we are given an `n x n` grid where each square `grid[i][j]` represents an elevation point.

As for the variable with water `t`, we don't need to worry about it because we are allowed to swim from one square to another square if and only if the elevation of both squares is less than or equal to `t`.

We also learned that we can swim infinite distances in zero time.

In our output, we want to find the least time that we can reach from position `[0, 0]` to the bottom right cell `[N - 1, N - 1]`.

Let's look at the example.
![alt image](images/778.png#center)

We start from the top left cell with value `0`, and we want to get to the bottom right cell with value `2`.

You can see that both paths to get to the bottom right cell are the same.
In order to find the minimum time to reach the bottom right cell, we must find a **path** where the **maximum height** of water is **minimized**. Our bottleneck is going to be whatever the maximum height is.

For example, if our `height = 5`, we are going to have to wait until `time >= height = 5`.

Now, when we know that we must find the **height** of water, we need to find a **path** that minimizes `t` and return it as the result.

### Dijkstra Algorithm Solution

```swift
struct Helper: Comparable {
    let time: Int
    let r: Int
    let c: Int

    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.time < rhs.time
    }

}

func swimInWater(_ grid: [[Int]]) -> Int {
    let N = grid.count
    var visited: Set<[Int]> = []
    var minHeap: Heap<Helper> = [Helper(time: grid[0][0], r: 0, c: 0)]
    let directions: [[Int]] = [[0, 1], [0, -1], [1, 0], [-1, 0]]
    visited.insert([0, 0])

    while !minHeap.isEmpty {
        let helper = minHeap.removeMin()
        let t = helper.time
        let r = helper.r
        let c = helper.c

        if r == N - 1 && c == N - 1 {
            return t
        }

        for direction in directions {
            let dr = direction[0]
            let dc = direction[1]
            let neiR = r + dr
            let neiC = c + dc

            if (neiR < 0 || neiC < 0 ||
                neiR == N || neiC == N ||
                visited.contains([neiR, neiC])) {
                continue
            }

            visited.insert([neiR, neiC])
            minHeap.insert(Helper(time: max(t, grid[neiR][neiC]), r: neiR, c: neiC))
        }
    }

    return -1
}
```

#### Explanation

We are going to solve this problem by using the Dijkstra algorithm, but it is going to be a slightly modified version. Instead of using the usual **queue**, we are going to use a **priority queue** (Min Heap).
![alt image](images/778-1.png#center)

* We are going to start from the top left cell and add to the **Min Heap** its adjacent **right** and **bottom** cell coordinates.

We are going to figure out what is going to be the **path** that we can take from `[0, 0]` to `[N - 1, N - 1]` such that our **maximum height** along that path is **minimized**.

We are using a `Min Heap` because it is going to help us find the **minimum height** in our path.
The `Min Heap` is going to have three values: `height`, `row`, and `column`, where the main element for comparison is going to be the `height`.

We are also going to use a `visited` hash set that is going to help us track coordinates that we have already visited.

Lastly, we are going to modify the Dijkstra algorithm so that we can track the **maximum height** that we have seen so far. This will help us **find a path with the smallest maximum height**.

For example, if we try to take two paths:
![alt image](images/778-2.png#center)

* If we decide to go by the **brown** path, you can see that the maximum height along that path equals `3`
* As for the **green** path, the maximum height equals `2`

Now, when we figure out that **we are not looking for the shortest path**, but instead we are looking for a **path with the smallest maximum height**, we are going to modify the Dijkstra algorithm so that when we are adding a height to the `Min Heap`, we will be adding the **maximum height** that we have seen so far.

#### Time/ Space complexity

* Time complexity: `O(n^2 * log n)`
* Space complexity: `O(n^2)`

#### Thank you for reading! 😊
