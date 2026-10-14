+++
title = "LeetCode - Integer Break"
date = 2026-10-14T18:07:36+03:00
tags = ["LeetCode", "Integer Break", "1D_Dynamic_Programming", "Medium", "Swift"]
draft = false
+++

### The problem

Given an integer `n`, break it into the sum of `k` **positive integers**, where `k >= 2`, and maximize the product of those integers.

Return _the maximum product you can get_.

**Example 1:**

```
Input: n = 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.

```

**Example 2:**

```
Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.

```

**Constraints:**

- `2 <= n <= 58`

#### Explanation

Let's begin with the definition of the product.

> The term ["product"](https://mathworld.wolfram.com/Product.html) refers to the result of one or more multiplications.

Usually, when we are asked to break something into smaller parts, it means recursion. So we can create a decision tree based on two decisions - break the current number further or don't break it.

As always with recursion, we will need to take care of a few base cases:
- The first is the value `1` because we cannot break `1` into smaller parts.
- We also cannot break `2` as it is based on the sum of `1 + 1`, and the product will be `1 * 1 = 1`.
- The last one is `3`, as it is based on the sum of `2 + 1` and the product of `2 * 1 = 2`.

![alt image](images/343.png#center)

We can represent two decisions by the formula `x + (num - x)`, which breaks a number into two parts.

For example, with `n = 8` and `x = 3`, the two numbers will be `3 and 5` because `3 + (8 - 3) = 8`, and the formula for the product will be `x * (num - x)` - `3 * (8 - 3) = 15`.

![alt image](images/343-1.png#center)

With all this information in mind, we can create a recursive solution:
```swift
func integerBreak(_ n: Int) -> Int {
   var memo: [Int: Int] = [:]
   
   func dfs(_ num: Int) -> Int {
       if let cached = memo[num] {
           return cached
       }
       
       if num <= 3 {
           return num - 1
       }
       
       var res = 0
       for x in 1 ..< n {
           res = max(
               res,
               x * (num - x),
               x * dfs(num - x)
           )
       }
       
       memo[num] = res
       return res
   }
   
   return dfs(n)
}
```

This solution is already slightly optimized, which reduces the time complexity to O(n^2). The space complexity is O(n).

### Dynamic Programing Solution

#### Explanation

We can also solve this problem by using Dynamic Programming.

The idea and even the formula behind the solution stays the same. But instead of recursion, we are going to use two loops.

In the outer loop, we skip `1` and start from `2` because we cannot break **one** into smaller pieces.

The inner loop goes from `1` to `num`, excluding `num`. This way, we can calculate the product based on previous values.

![alt image](images/343-2.png#center)

There also exist solutions that are much faster than the recursive and DP approaches, achieving O(n) or even O(logn) time and constant space, but they are out of scope. If you are interested, you are welcome to try solving it yourself. A little hint: solutions involve some math.

#### Code
```swift
func integerBreak(_ n: Int) -> Int {
   var dp = Array(repeating: 0, count: n + 1)
   dp[1] = 1
   
   for num in 2 ... n {
       if num == n {
           dp[num] = 0
       } else {
           dp[num] = num
       }
       for i in 1 ..< num {
           dp[num] = max(dp[num], dp[i] * dp[num - i])
       }
   }
   
   return dp[n]
}
```

#### Time/ Space complexity

* Time complexity: O(n^2)
* Space complexity: O(n)

#### Thank you for reading! 😊
