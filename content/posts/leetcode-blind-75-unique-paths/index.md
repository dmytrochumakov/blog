+++
title = "LeetCode - Blind 75 - Unique Paths"
date = 2025-04-05T07:43:55+03:00
tags = ["LeetCode", "Blind 75", "Unique Paths", "Swift"]
draft = false
+++

### The problem 
There is a robot on an `m x n` grid. The robot is initially located at the top-left corner (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to `2 * 10^9`.

#### Examples
![alt image](images/robot_maze.png#center)
``` 
Input: m = 3, n = 7
Output: 28
```

```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

#### Constraints
* 1 <= m, n <= 100

### Recursion Solution 
``` swift 
func uniquePaths(_ m: Int, _ n: Int) -> Int {

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == (m - 1) && j == (n - 1) {
            return 1
        }
        if i >= m || j >= n {
            return 0
        }
        return dfs(i, j + 1) + dfs(i + 1, j)
    }

    return dfs(0, 0)
}
```

#### Explanation
We can solve this problem in a brute force way by using recursion.  
![alt image](images/62.png#center)

From the description of the problem, we learn that we have two choices: we can go right or we can go down, and we are going to repeat this.

It is not a very efficient solution, and it will take O(2^(m+n)) time because we do a lot of repetitive work, but we can use cache to optimize the time complexity and avoid repetitive work.

#### Time/Space complexity
* Time complexity: O(2^(m+n))
* Space complexity: O(m+n)
* where `m` is the number of rows and `n` is the number of columns

### Depth Dirst Search Top-Down Solution 
``` swift 
func uniquePaths(_ m: Int, _ n: Int) -> Int {
    var memo: [[Int]] = Array(repeating: Array(repeating: -1, count: n), count: m)

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == (m - 1) && j == (n - 1) {
            return 1
        }
        if i >= m || j >= n {
            return 0
        }
        if memo[i][j] != -1 {
            return memo[i][j]
        }
        memo[i][j] = dfs(i, j + 1) + dfs(i + 1, j)
        return memo[i][j]
    }

    return dfs(0, 0)
}
``` 

#### Explanation
We can optimize our brute force solution by adding caching to it. We are going to do it by calculating the result from each position and adding them together.  
![alt image](images/62-1.png#center)

But first, we need to figure out our base cases.
- We will define our destination row with a value of `1`.
- We will also define our out-of-bounds rows with a value of `0`.
- The value that we can place in the cell will be calculated by the sum of the right value and the bottom value, and we will repeat this process.

As a result, caching helped us optimize our time complexity. But we can go even further and optimize space complexity.

#### Time/Space complexity
* Time complexity: O(m*n)
* Space complexity: O(m*n)

### Dynamic Programming Bottom-Up (Space Optimized) solution 
``` swift 
func uniquePaths(_ m: Int, _ n: Int) -> Int {
    var row = Array(repeating: 1, count: n)

    for i in 0 ..< m - 1 {
        var newRow = Array(repeating: 1, count: n)
        for j in stride(from: n - 2, to: -1, by: -1) {
            newRow[j] = newRow[j + 1] + row[j]
        }
        row = newRow
    }

    return row[0]
}
```

#### Explanation
Another way to solve this problem is by using a dynamic programming bottom-up approach.  
![alt image](images/62-2.png#center)

We start from our destination and we go backwards to our start point, calculating the value for each cell.  
We can do that by calculating the sum of the right value and the bottom value, and we repeat this process until we end up at the start position with our result.

#### Time/Space complexity
* Time complexity: O(m*n)
* Space complexity: O(n)

#### Thank you for reading! 😊
