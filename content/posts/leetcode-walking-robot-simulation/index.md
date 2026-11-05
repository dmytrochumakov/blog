+++
title = "LeetCode - Walking Robot Simulation"
date = 2026-11-05T22:58:51+03:00
tags = ["LeetCode", "Walking Robot Simulation", "Math", "Medium", "Swift"]
draft = false
+++

### The problem

A robot on an infinite XY-plane starts at point `(0, 0)` facing north. The robot receives an array of integers `commands`, which represents a sequence of moves that it needs to execute. There are only three possible types of instructions the robot can receive:

- `-2`: Turn left `90` degrees.
- `-1`: Turn right `90` degrees.
- `1 <= k <= 9`: Move forward `k` units, one unit at a time.

Some of the grid squares are `obstacles`. The `ith` obstacle is at grid point `obstacles[i] = (xi, yi)`. If the robot runs into an obstacle, it will stay in its current location (on the block adjacent to the obstacle) and move on to the next command.

Return the **maximum squared Euclidean distance** that the robot reaches at any point in its path (i.e., if the distance is `5`, return `25`).

**Note:**

- There can be an obstacle at `(0, 0)`. If this happens, the robot will ignore the obstacle until it has moved off the origin. However, it will be unable to return to `(0, 0)` due to the obstacle.
- North means +Y direction.
- East means +X direction.
- South means -Y direction.
- West means -X direction.

**Example 1:**

**Input:** commands = [4,-1,3], obstacles = []

**Output:** 25

**Explanation:**

The robot starts at `(0, 0)`:

1. Move north 4 units to `(0, 4)`.
2. Turn right.
3. Move east 3 units to `(3, 4)`.

The furthest point the robot ever gets from the origin is `(3, 4)`, which squared is `3² + 4² = 25` units away.

**Example 2:**

**Input:** commands = [4,-1,4,-2,4], obstacles = [[2,4]]

**Output:** 65

**Explanation:**

The robot starts at `(0, 0)`:

1. Move north 4 units to `(0, 4)`.
2. Turn right.
3. Move east 1 unit and get blocked by the obstacle at `(2, 4)`. The robot is at `(1, 4)`.
4. Turn left.
5. Move north 4 units to `(1, 8)`.

The furthest point the robot ever gets from the origin is `(1, 8)`, which squared is `1² + 8² = 65` units away.

**Example 3:**

**Input:** commands = [6,-1,-1,6], obstacles = [[0,0]]

**Output:** 36

**Explanation:**

The robot starts at `(0, 0)`:

1. Move north 6 units to `(0, 6)`.
2. Turn right.
3. Turn right.
4. Move south 5 units and get blocked by the obstacle at `(0,0)`. The robot is at `(0, 1)`.

The furthest point the robot ever gets from the origin is `(0, 6)`, which squared is `6² = 36` units away.

**Constraints:**

- `1 <= commands.length <= 10^4`
- `commands[i]` is either `-2`, `-1`, or an integer in the range `[1, 9]`.
- `0 <= obstacles.length <= 10^4`
- `-3 * 10^4 <= xi, yi <= 3 * 10^4`
- The answer is guaranteed to be less than `2^31`.

#### Explanation

This problem is a good one. I recommend trying to solve it yourself, at least at a high level.

First, I thought that it was a grid problem, but when I started drawing it, I understood that it was not a grid but a coordinate system problem.

![alt image](images/874.png#center)

So we do not need to make a grid and walk there.

One of the ways to solve it is by using `y, -y, x, -x` to keep track of the current direction, but the code would be messy, so it would be better to represent directions like we usually do when we have matrix or grid problems: `(0, 1), (1, 0), (0, -1), (-1, 0)`.

The rest is easy:
- We convert `obstacles` to a set in order to have O(1) lookups.
- We are going to initialize four properties: `x`, `y`, `direction`, and `res`.
- Next, we are going to iterate over `commands` and update the direction or coordinates.

We are using modulo `4` to avoid moving out of bounds.

#### Code
```swift
struct Point: Hashable {
    let x: Int
    let y: Int
}

func robotSim(_ commands: [Int], _ obstacles: [[Int]]) -> Int {
    let obstacles = Set(obstacles.map { Point(x: $0[0], y: $0[1]) })
    let directions = [[0, 1], [1, 0], [0, -1], [-1, 0]] // north, east, south, west

    var x = 0
    var y = 0
    var direction = 0
    var res = 0
    
    for command in commands {
        if command == -1 {
            direction = (direction + 1) % 4
        } else if command == -2 {
            direction = (direction + 3) % 4
        } else {
            let dx = directions[direction][0]
            let dy = directions[direction][1]
            for _ in 0 ..< command {
                let nx = x + dx
                let ny = y + dy
                if obstacles.contains(Point(x: nx, y: ny)) {
                    break
                }
                x = nx
                y = ny
            }
        }
        
        res = max(res, x*x + y*y)
    }
    
    return res
}
```

#### Time/ Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m)
- Where `n` is the number of commands and `m` is the number of obstacles.

#### Thank you for reading! 😊
