+++
title = "LeetCode - 150 - Coin Change II"
date = 2026-02-18T09:09:47+03:00
tags = ["LeetCode", "150", "Coin Change II", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: There are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**

```
Input: amount = 3, coins = [2]
Output: 0
Explanation: The amount of 3 cannot be made up just with coins of 2.
```

**Example 3:**

```
Input: amount = 10, coins = [10]
Output: 1
```

**Constraints:**

* `1 <= coins.length <= 300`
* `1 <= coins[i] <= 5000`
* All the values of `coins` are **unique**.
* `0 <= amount <= 5000`

#### Explanation

From the description of the problem, we learn that we are given an integer array `coins`, where each coin has a different denomination. We are also given the second parameter `amount` that we need to sum up using our coins. In the output, if it is possible, we must return how many combinations can sum up to the given `amount` parameter.

### Dynamic Programming (Top-Down) Solution

```swift
func change(_ amount: Int, _ coins: [Int]) -> Int {
    let coins = coins.sorted()
    var cache = Array(repeating: Array(repeating: -1, count: amount + 1), count: coins.count + 1)

    func dfs(_ i: Int, _ a: Int) -> Int {
        if a == 0 {
            return 1
        }

        if i >= coins.count {
            return 0
        }

        if cache[i][a] != -1 {
            return cache[i][a]
        }

        var res = 0
        if a >= coins[i] {
            res = dfs(i + 1, a)
            res += dfs(i, a - coins[i])
        }

        cache[i][a] = res
        return res
    }

    return dfs(0, amount)
}
```

#### Explanation

Let's visualize one of the examples to better understand the brute-force solution.
![alt image](images/518.png#center)

Initially, we have three choices `1, 2, and 5`, and we need to avoid adding any duplicates. In order to do that, we are going to remove previous values from consideration while we are moving to the right.
![alt image](images/518-1.png#center)

For example, if we are at value `2`, we are going to exclude value `1`, or if we are at value `5`, then we are going to exclude previous values `1 and 2`. We can do this by having an `i` pointer and excluding every value to the left of it.

If we move over our target amount, we are going to stop moving down that decision.

The brute-force solution is going to be inefficient because the time complexity will be `O(m^n)`.
We can optimize it by using the memoization technique, which will help us reduce the time complexity to `O(m*n)`, and the space complexity will be the same.

> One quick note that I forgot to mention: in order for our solution to work, we must first sort our `coins` array, which helps skip unnecessary computation.

#### Time/ Space complexity

* Time complexity: O(n*a)
* Space complexity: O(n*a)
* Where `n` is the number of coins and `a` is the given amount

### Dynamic Programming (Bottom-Up) Solution

```swift
func change(_ amount: Int, _ coins: [Int]) -> Int {
    let n = coins.count
    let coins = coins.sorted()
    var dp = Array(
        repeating: Array(repeating: 0, count: amount + 1),
        count: n + 1
    )

    for i in 0 ... n {
        dp[i][0] = 1
    }

    for i in stride(from: n - 1, through: 0, by: -1) {
        for a in 0 ... amount {
            let base = dp[i + 1][a]
            if a >= coins[i] {
                let addend = dp[i][a - coins[i]]
                if base > Int.max - addend {
                    dp[i][a] = 0
                } else {
                    dp[i][a] = base + addend
                }
            } else {
                dp[i][a] = base
            }
        }
    }

    return dp[0][amount]
}
```

#### Explanation

We can solve this problem using a dynamic programming bottom-up solution that is going to help us come up with a space-optimized solution.

Let's visualize the solution to better understand what we are going to do.
![alt image](images/518-2.png#center)

We are going to have a 2D grid that represents `coins` and `amount`.

We are going to fill our 2D grid from bottom to top. The column with `amount = 0` will be filled with the value `1` because we only have **one** way to sum up the **amount** we are at to **zero**.

![alt image](images/518-3.png#center)

If we choose the cell with `amount = 1` and `coin = 5`, we will get a negative index `1 - 5 = -4`, and since we can't get a value if we are out of bounds, we will fill our cell with the value `0`.

![alt image](images/518-4.png#center)

When we are at the cell with `amount = 1` and `coin = 2`, we have two choices:

* we can take `coin = 2`, and our index would be `1 - 2 = -1`, meaning that we cannot make that choice, or
* we can take the already calculated value from below (for amount `1`), which equals `0`.

Since we do not have values that are greater than `0`, we are going to place `0` into the current cell.

![alt image](images/518-5.png#center)

When we are at the cell with `amount = 1` and `coin = 1`, our index will be `1 - 1 = 0`; therefore, we are going to take the value from the right cell and add it to the value from the bottom cell, and put the result into the current cell.

We are going to continue calculating values for each row until we reach the cell in the top-left corner.

![alt image](images/518-6.png#center)

> As you can see, with this dynamic programming approach, when we need to calculate values for each cell, we must have the entire array that stores them. That creates additional space that we can avoid.

This solution is going to have the same time and space complexity as the previous one, but now we can come up with space optimization.

#### Time/ Space complexity

* Time complexity: O(n*a)
* Space complexity: O(n*a)
* Where `n` is the number of coins and `a` is the given amount

### Dynamic Programming (Space Optimized) Solution

```swift
func change(_ amount: Int, _ coins: [Int]) -> Int {
    var dp = [Int](repeating: 0, count: amount + 1)
    dp[0] = 1

    for i in stride(from: coins.count - 1, through: 0, by: -1) {
        var nextDP = [Int](repeating: 0, count: amount + 1)
        nextDP[0] = 1

        for a in 1 ..< (amount + 1) {
            nextDP[a] = dp[a]
            if a - coins[i] >= 0 {
                let addend = nextDP[a - coins[i]]
                if nextDP[a] > Int.max - addend {
                    nextDP[a] = 0
                } else {
                    nextDP[a] += addend
                }
            }
        }

        dp = nextDP
    }

    return dp[amount]
}
```

#### Explanation

Instead of using the entire array and filling every cell, we can use only the previous row.
![alt image](images/518-7.png#center)

* Firstly, we are going to calculate the bottom row.
  ![alt image](images/518-8.png#center)

* Next, we are going to calculate the row above that by using values from the previous row.

> We are going to use memory for one row because we only need to look down **one** row at a time, while when we are trying to find the index with the value for the change, we can look at **multiple** positions to the right.

![alt image](images/518-9.png#center)

* Lastly, we are going to calculate the first row.

#### Time/ Space complexity

* Time complexity: O(n*a)
* Space complexity: O(a)
* Where `n` is the number of coins and `a` is the given amount

#### Thank you for reading! 😊
