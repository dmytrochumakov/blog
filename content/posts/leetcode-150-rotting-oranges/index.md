+++
title = "LeetCode - 150 - Rotting Oranges"
date = 2025-10-17T07:58:23+03:00
tags = ["LeetCode", "150", "Rotting Oranges", "Swift"]
draft = false
+++

### The problem

You are given an `m x n grid` where each cell can have one of three values:

* `0` representing an empty cell,
* `1` representing a fresh orange, or
* `2` representing a rotten orange.

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

#### Examples

![alt image](images/oranges.png#center)

```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

```
Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten because rotting only happens 4-directionally.
```

```
Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
```

#### Constraints

* m == grid.length
* n == grid[i].length
* 1 <= m, n <= 10
* grid[i][j] is 0, 1, or 2.

#### Explanation

From the description of the problem, we learn that we are given an `m*n` grid that represents empty, fresh, or rotten oranges. We also know that every minute that passes, any fresh oranges that are 4-directionally adjacent to a rotten orange become rotten.
Let's look at the first example:
![alt image](images/994.png#center)

* At time `0`, we only have `one` rotten orange.
* When time passes and we are at time `1`, adjacent oranges at positions `(0, 1)` and `(1, 0)` become rotten.
* When we are at time `2`, adjacent oranges at positions `(0, 2)` and `(1, 1)` become rotten too.
* Next, at time `3`, the adjacent orange at position `(2, 1)` also becomes rotten.
* Lastly, at time `4`, the orange at position `(2, 2)` also becomes rotten.
  As you can see, `four` minutes later after the start, all oranges are rotten; therefore, we return `four` in our result.

Now let's look at a slightly different example and try to figure out what algorithm we can use to make sure that adjacent oranges become rotten and track the time that it takes for oranges to become rotten.
![alt image](images/994-1.png#center)

* We can try using the depth-first search algorithm.
  ![alt image](images/994-2.png#center)
  Imagine that we start from the rotten orange with position `(0,0)`.
* At time equals `1`, the orange at position `(1, 0)` becomes rotten.
* Next, at time `2` and position `(1, 1)`, the orange becomes rotten too.
* At time equals `3` and position `(1, 2)`, the orange becomes rotten.
* Next, at time equals `4` and positions `(1, 3)` and `(2, 2)`, oranges become rotten.
* Lastly, at time equals `5` and position `(2, 3)`, the orange becomes rotten.

As a result, it took us `five` units of time to make the given oranges rotten. But `five` is not the right answer because our first initial rotten oranges will be making adjacent oranges rotten simultaneously; therefore, we can’t use the depth-first search algorithm.

### Breadth First Search Solution

```swift
func orangesRotting(_ grid: [[Int]]) -> Int {
    var grid = grid
    var q: [[Int]] = []
    var time = 0
    var fresh = 0
    let rows = grid.count
    let cols = grid[0].count

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if grid[r][c] == 1 {
                fresh += 1
            }
            if grid[r][c] == 2 {
                q.append([r, c])
            }
        }
    }

    let directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
    while !q.isEmpty && fresh > 0 {
        for i in 0 ..< q.count {
            let value = q.removeFirst()
            let r = value[0]
            let c = value[1]
            for direction in directions {
                let dr = direction[0]
                let dc = direction[1]
                let row = dr + r
                let col = dc + c
                if (row < 0 || row == rows || col < 0 || col == cols || grid[row][col] != 1) {
                    continue
                }
                grid[row][col] = 2
                q.append([row, col])
                fresh -= 1
            }
        }
        time += 1
    }

    return fresh == 0 ? time : -1
}
```

#### Explanation

Let's look at an example:
![alt image](images/994-3.png#center)

* At time equals `1`, oranges at positions `(1, 0)` and `(1, 3)` will become rotten.
* Next, at time equals `2`, oranges at positions `(1, 1)`, `(1, 2)`, and `(2, 3)` will become rotten.
* Lastly, at time equals `3`, the orange at position `(2, 2)` will become rotten because of the rotten orange above it or to the right of it.

As a result, in `three` units of time, all of the oranges became rotten. You may notice that we used the breadth-first search algorithm. BFS is very helpful in this case because we can run it simultaneously on multiple sources at the same time.

The algorithm will look like this:
![alt image](images/994-4.png#center)

* We are going to initialize our queue with the initial rotten oranges.
* Next, we are going to pop the initial oranges, and we will call it `1` unit of time.
  ![alt image](images/994-5.png#center)
* After that, we are going to add the next rotten oranges to our queue, pop them, and call it unit time `2`.
* Lastly, we will continue following the steps above until our queue is empty.

> It’s not guaranteed that we can make every single orange rotten. For example:

![alt image](images/994-6.png#center)
If we take our previous example and add another orange to the bottom left cell, we know that in that example, every orange will be rotten except the orange in the bottom left cell.

Now, with the information that we learned, we can add an additional property to our algorithm that will help us track how many oranges we had initially.

> If after our algorithm is done we still have fresh oranges, we would return `-1` as our result.

#### Time/Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m*n)
* Where `m` is the number of rows and `n` is the number of columns in the grid.

#### Thank you for reading! 😊
