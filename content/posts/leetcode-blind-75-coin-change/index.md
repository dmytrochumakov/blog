+++
title = "LeetCode - Blind 75 - Coin Change"
date = 2025-03-26T07:51:46+03:00
tags = ["LeetCode", "Blind 75", "Coin Change", "Swift"]
draft = false
+++

### The problem  
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

#### Examples

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

```
Input: coins = [2], amount = 3
Output: -1
```

```
Input: coins = [1], amount = 0
Output: 0
```

#### Constraints
* 1 <= coins.length <= 12  
* 1 <= coins[i] <= 2³¹ - 1  
* 0 <= amount <= 10⁴  

### Dynamic programming top-down solution  

```swift
func coinChange(_ coins: [Int], _ amount: Int) -> Int {
    var memo: [Int: Int] = [:]
    let int1e9 = Int(1e9)

    func dfs(_ amount: Int) -> Int {
        if amount == 0 {
            return 0
        }

        if let val = memo[amount] {
            return val
        }

        var res = int1e9
        for coin in coins {
            if amount - coin >= 0 {
                res = min(res, 1 + dfs(amount - coin))
            }
        }

        memo[amount] = res
        return res
    }

    let minCoins = dfs(amount)
    if minCoins >= int1e9 {
        return -1
    } else {
        return minCoins
    }
}
```

#### Explanation  
One of the ways that we can solve this problem is by using a depth-first search algorithm with memoization to optimize the overall time complexity.  

For example, if we have input `[1,3,4,5]` and `amount = 7`, this means that in our decision tree, we have four possible choices.  
![alt image](images/p-322.png#center)  

- We can choose `1`, `3`, `4`, or `5`, and  
- The remainder amount will be `6`, `4`, `3`, and `2` respectively.  

If we continue going down the path of `5` and choose `2`, we still have an unlimited quantity of coins. We can choose `1`, `3`, `4`, or `5`.  

- If we choose coin `1`, we will have a remainder amount of `1`.  
- If we choose coin `3`, we get a negative `-1`, which tells us that `5 + 3` cannot be our amount, and we don't need to continue searching that path.  

Now, if we choose coin `1`, we can also choose `3, 4, 5`, but we see that it won’t work because we will end up with negative values.  
If we choose `1`, we finally get to `0`, meaning we took `5`, then `1`, then `1`. Counting the coins used, we see that we needed 3 coins. This means we found one possible way to sum up to `amount = 7`, and we have a working algorithm that provides a solution.  

From the above, we learn that we need to take care of two base cases:  
- When the amount reaches `0`, and  
- When the amount is less than `0`.  

We can also see that we can break down the problem into subproblems, and these subproblems repeat themselves.  
For example, if we have an amount of `1`, we don’t need to compute it because we already know that we only need a single coin `1` to reduce it to `0`. Since that subproblem repeats, we can store its result instead of recomputing it.  

This solution is called the dynamic programming top-down approach with memoization.  

#### Time/space complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

### Dynamic programming bottom-up solution  

```swift
func coinChange(_ coins: [Int], _ amount: Int) -> Int {
    var dp = Array(repeating: amount + 1, count: amount + 1)
    dp[0] = 0

    for a in 1 ..< amount + 1 {
        for c in coins {
            if a - c >= 0 {
                dp[a] = min(dp[a], 1 + dp[a - c])
            }
        }
    }

    if dp[amount] != amount + 1 {
        return dp[amount]
    } else {
        return -1
    }
}
```

#### Explanation  
We can also solve this problem with a dynamic programming bottom-up algorithm. Instead of solving the original problem with `amount = 7`, we solve it in reverse order.  
We start with the smallest amount, `0`.  

![alt image](images/p-322-1.png#center)  

We want to know the minimum number of coins required to sum to `0`, which we call `dp[0]`. We know that it takes `0` coins.  

When we want to know the minimum number of coins needed to sum up to `1`, we can take only `1` coin. Then, we repeat that process.  
For `dp[2]`, we take `1 + dp[1]`, resulting in `dp[2] = 2`. We repeat this process all the way to `7`.  

The computed version of all values will look like this:  
![alt image](images/p-322-2.png#center)  

To compute `dp[7]`, we take `1 + dp[6]`, which results in `3`, but this is not the minimal solution.  

- If we take coin `3`, we need `1 + dp[4] = 2`, because `amount 7 - 3 = 4`.  
We need to compute all our values from `input = [1, 3, 4, 5]`. We have already computed values `1` and `3`, so now we compute the rest.  
- If we take coin `4`, the result of `1 + dp[3]` will be `2`.  
- If we take coin `5`, the result of `1 + dp[2]` will be `3`.  

Lastly, we find the minimum of the computed values, so the result is `2`.  

#### Time/space complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

#### Thank you for reading! 😊
