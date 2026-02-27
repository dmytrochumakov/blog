+++
title = "LeetCode - 150 - Target Sum"
date = 2026-02-27T07:53:51+03:00
tags = ["LeetCode", "150", "Target Sum", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of `nums` by adding one of the symbols `'+'` and `'-'` before each integer in `nums` and then concatenate all the integers.

* For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build that evaluate to `target`.

**Example 1:**

```
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums equal to target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**Example 2:**

```
Input: nums = [1], target = 1
Output: 1
```

**Constraints:**

* `1 <= nums.length <= 20`
* `0 <= nums[i] <= 1000`
* `0 <= sum(nums[i]) <= 1000`
* `-1000 <= target <= 1000`

#### Explanation

From the description of the problem, we learn that we are given an integer array `nums` that we want to sum up to the `target` value. We are also given symbols `+` and `-` that we can add to each number in the array to build an `expression` and concatenate those values to reach the `target`.

### Brute Force (Recursion) Solution

```swift
func findTargetSumWays(_ nums: [Int], _ target: Int) -> Int {
    func dfs(_ i: Int, _ curSum: Int) -> Int {
        if i == nums.count {
            return curSum == target ? 1 : 0
        }

        let res = (
            dfs(i + 1, curSum + nums[i]) +
            dfs(i + 1, curSum - nums[i])
        )

        return res
    }

    return dfs(0, 0)
}
```

#### Explanation

The brute force way to solve this problem is to use recursion.
The decision tree will look like this
![alt image](images/494.png#center)

The solutions are going to look like this:
![alt image](images/494-1.png#center)

* We are going to have a property with index `i` that helps us track the position that we are currently at in our array of `nums`.
* We are also going to have a second parameter `curSum` that helps us track our current sum.

It's not a very efficient solution because it takes O(2^n) time complexity. But we can optimize it by adding caching.

#### Time/ Space complexity

* Time complexity: O(2^n)
* Space complexity: O(n)

### Brute Force (Caching) Solution

```swift
func findTargetSumWays(_ nums: [Int], _ target: Int) -> Int {
    struct Helper: Hashable {
        let i: Int
        let curSum: Int
    }

    var cache: [Helper: Int] = [:]

    func dfs(_ i: Int, _ curSum: Int) -> Int {
        if let cached = cache[Helper(i: i, curSum: curSum)] {
            return cached
        }

        if i == nums.count {
            return curSum == target ? 1 : 0
        }

        let res = (
            dfs(i + 1, curSum + nums[i]) +
            dfs(i + 1, curSum - nums[i])
        )

        cache[Helper(i: i, curSum: curSum)] = res
        return res
    }

    return dfs(0, 0)
}
```

#### Time/ Space complexity

* Time complexity: O(n * m)
* Space complexity: O(n * m)
* Where `n` is the length of `nums` and `m` is the sum of all elements in the array.

### Dynamic Programming (Bottom-Up) Solution

```swift
func findTargetSumWays(_ nums: [Int], _ target: Int) -> Int {
    let n = nums.count
    var dp = Array(repeating: [Int: Int](), count: n + 1)
    dp[0][0] = 1

    for i in 0 ..< n {
        for (curSum, count) in dp[i] {
            dp[i + 1][curSum + nums[i], default: 0] += count
            dp[i + 1][curSum - nums[i], default: 0] += count
        }
    }

    return dp[n][target] ?? 0
}
```

#### Explanation

We can solve this problem in the most efficient way by using a bottom-up dynamic programming algorithm.

We are going to have a 2D array, where rows are going to represent indices from the given array of `nums` plus one additional row. The same goes for columns, but instead of having only positive indices, we are going to add negative ones too.

In this solution, our rows represent how many numbers we are allowed to take starting from the beginning, and our columns represent the `current sum`. We are going to use these properties to store the `count`.

For example:
![alt image](images/494-2.png#center)

* When we are at `row = 0` and `column = 0`, our `count` is going to be `1` because we have only one way of getting `dp[0][0]`.

We are going to continue calculating the `count` for every row, where we will be using previous results to calculate the next ones:
![alt image](images/494-3.png#center)

In the picture, you can see that at `column = 3`, which represents our target sum, and `row = 5`, we found our result.

This solution uses O(n * m) memory, which we can optimize further.

#### Time/ Space complexity

* Time complexity: O(n * m)
* Space complexity: O(n * m)
* Where `n` is the length of the array `nums` and `m` is the sum of all the elements in the array

### Dynamic Programming (Space Optimized) Solution

```swift
func findTargetSumWays(_ nums: [Int], _ target: Int) -> Int {
    let n = nums.count
    var dp: [Int: Int] = [:]
    dp[0] = 1

    for i in 0 ..< n {
        var nextDP: [Int: Int] = [:]
        for (curSum, count) in dp {
            nextDP[curSum + nums[i], default: 0] += count
            nextDP[curSum - nums[i], default: 0] += count
        }
        dp = nextDP
    }

    return dp[target] ?? 0
}
```

#### Explanation

We can optimize the dynamic programming space complexity even further by storing only up to two rows in memory at a time.

#### Time/ Space complexity

* Time complexity: O(n * m)
* Space complexity: O(m)
* Where `n` is the length of the array `nums` and `m` is the sum of all the elements in the array

#### Thank you for reading! 😊
