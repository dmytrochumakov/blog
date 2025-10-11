+++
title = "LeetCode - 150 - Walls and Gates"
date = 2025-10-11T08:04:42+03:00
tags = ["LeetCode", "150", "Walls and Gates", "Swift"]
draft = false
+++

### The problem

You are given an `m*n` 2D `grid` initialized with these three possible values:

* `-1` - A wall or obstacle.
* `0` - A gate.
* `INF` - Infinity means an empty room. We use the value `2^31 - 1 = 2147483647` to represent `INF`.

Fill each empty room with the distance to its nearest gate. If it’s impossible to reach the gate, it should be filled with `INF`.

Assume the grid can only be traversed up, down, left, or right.

Modify the `grid` in place.

#### Examples

![alt image](images/ex1.png#center)

```
Input: [
  [2147483647,-1,0,2147483647],
  [2147483647,2147483647,2147483647,-1],
  [2147483647,-1,2147483647,-1],
  [0,-1,2147483647,2147483647]
]

Output: [
  [3,-1,0,1],
  [2,2,1,-1],
  [1,-1,2,-1],
  [0,-1,3,4]
]
```

```
Input: [
  [0,-1],
  [2147483647,2147483647]
]

Output: [
  [0,-1],
  [1,2]
]
```

#### Constraints

* m == grid.length
* n == grid[i].length
* 1 <= m, n <= 100
* grid[i][j] is one of {-1, 0, 2147483647}

#### Explanation

From the description of the problem, we learn that we are given an `m*n` grid where each position could have three possible values:

* `-1` that represents a wall
* `0` that represents a gate
* `INF` that represents an empty room

We also learned that we need to identify the nearest gate to the empty room (`INF`) and replace it with the distance from the nearest gate to the empty room. If we can’t do that, we should fill it with `INF`.

Now, let’s look at the first example:
![alt image](images/111.png#center)

* If we start from the top right cell and we are trying to determine how far away the gate is from the empty room, you can see that it’s only one position away from a gate. We can’t go up or right because those positions are out of bounds, and we can’t go down because we have a wall.
  ![alt image](images/111-1.png#center)
* If we start from the top left cell, we have three positions to the gate.
  ![alt image](images/111-2.png#center)
* If we start from the second row and second column and we are trying to find the distance to the `bottom left gate`, we will need three positions to get to this gate.
  ![alt image](images/111-3.png#center)
* But if you look at the `third column` of the `second row`, you can see that we have a gate with a closer distance of two.

The brute force way to solve this problem is to use a backtracking algorithm and find the distance for every single position of the entire grid. This solution will work but it will take O(m*n*4^(m*n)), which is not very efficient.

### Breadth First Search Solution

```swift
func wallsAndGates(_ grid: inout [[Int]]) {
    let rows = grid.count
    let cols = grid[0].count
    var visit: Set<[Int]> = []

    func addRoom(_ r: Int, _ c: Int) {
        if (r < 0 || r == rows || c < 0 || c == cols || visit.contains([r, c]) || grid[r][c] == -1) {
            return
        }
        visit.insert([r, c])
        q.append([r, c])
    }

    var q: [[Int]] = []
    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if grid[r][c] == 0 {
                q.append([r, c])
                visit.insert([r, c])
            }
        }
    }

    var dist = 0
    while !q.isEmpty {
        for _ in 0 ..< q.count {
            let cell = q.removeFirst()
            let r = cell[0]
            let c = cell[1]
            grid[r][c] = dist
            addRoom(r + 1, c)
            addRoom(r - 1, c)
            addRoom(r, c + 1)
            addRoom(r, c - 1)
        }
        dist += 1
    }
}
```

#### Explanation

We can optimize our brute force solution by using the breadth first search algorithm.
Let’s look at an example when we start from the second row and first column position:
![alt image](images/111-4.png#center)

* We are going to visit all four directions and find what is the closest gate that we can get to.
* To avoid a scenario where we visit the same position twice, we are going to create a `visited` set.
  ![alt image](images/111-5.png#center)
  We can’t achieve an `O(m*n)` time solution if we are going to do breadth first search from the rooms, but if we do BFS from the gates, we can mark all adjacent rooms with distance `1`. You can see if we start calculating distance from the gate with the position of the first row and third column, we have an incorrect distance for the bottom left gate.
  ![alt image](images/111-6.png#center)
  To avoid this scenario, we are going to simultaneously do a BFS from both gates.
  When we do our BFS from the bottom left gate, we are not going to visit the second column because it has already been visited.

#### Time/Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m*n)
* Where `m` is the number of rows and `n` is the number of columns in the grid

#### Thank you for reading! 😊
