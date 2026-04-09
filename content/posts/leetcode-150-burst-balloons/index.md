+++
title = "LeetCode - 150 - Burst Balloons"
date = 2026-04-09T08:10:49+03:00
tags = ["LeetCode", "150", "Burst Balloons", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it, represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `i`th balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return *the maximum coins you can collect by bursting the balloons wisely*.

**Example 1:**

```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

**Example 2:**

```
Input: nums = [1,5]
Output: 10
```

**Constraints:**

* `n == nums.length`
* `1 <= n <= 300`
* `0 <= nums[i] <= 100`

### Recursive Solution

#### Explanation

Let's begin with visualization of our base case when `i - 1` and `i + 1` are out of bounds. For example, if we had `nums = [5]`, we would pop the value and multiply it by **one** because both indices are out of bounds.
![alt image](images/312.png#center)

The brute-force way to solve this problem is to use recursion that helps us build a decision tree where we can try every possible combination of bursting balloons.

In order to take care of the base case, we are going to add **one**s to the beginning and end of the array. Then, in the implementation, we are going to skip them because they are not part of the computation; they only exist to avoid the out-of-bounds scenario. For the same reason, we are not including the **first** and **last** indices when iterating over **nums**.

The rest of the algorithm is straightforward: we compute `nums[i - 1] * nums[i] * nums[i + 1]` and find the maximum value.

This is not an efficient solution because it will take `O(n!)` time.

```swift
func maxCoins(_ nums: [Int]) -> Int {
    var nums: [Int] = [1] + nums + [1]

    func dfs(_ nums: [Int]) -> Int {
        if nums.count == 2 {
            return 0
        }

        var maxCoins = 0
        for i in 1 ..< nums.count - 1 {
            var coins = nums[i - 1] * nums[i] * nums[i + 1]
            coins += dfs(Array(nums[0 ..< i]) + Array(nums[i + 1 ..< nums.count]))
            maxCoins = max(maxCoins, coins)
        }

        return maxCoins
    }

    return dfs(nums)
}
```

#### Time/ Space complexity

* Time complexity: O(n!)
* Space complexity: O(n^2)

### Dynamic Programming (Top-Down) Solution

#### Explanation

In order to solve this problem in a more efficient way, we need to figure out how we can split the problem into smaller subproblems.

One way to do this is to burst a balloon at the current index and split the rest of the array into two subarrays. For example, if we had `nums = [3, 1, 5, 8]` and the current index is at value `5`:
![alt image](images/312-1.png#center)

We would get two subarrays `[3, 1]` and `[8]`:

* the first subarray would result in value `6`
* the second would result in value `8`
* and the total would be `14`
  ![alt image](images/312-2.png#center)

But we can't break the array this way because, in reality, both subarrays are going to be connected to each other. For example, two subarrays `[3, 1]` and `[8]` would form one `[3, 1, 8]` and result in `3 * 1 * 8 = 24`.
![alt image](images/312-3.png#center)

Before, we assumed that we could start from the value at the current index and then compute the remaining array, and we learned that it did not work.

In order for this to work, we are going to compute subarrays first and the value at the current index last. This way, we guarantee that our subarrays are never going to be connected, and we can use them independently. This helps us split the algorithm into smaller subproblems.
![alt image](images/312-4.png#center)

As for the implementation:
To handle the case when our indices are out of bounds, we are going to add additional **one**s to the beginning and the end of the array. We are also going to have left and right boundaries that will point us to the actual values in the array, helping us avoid the out-of-bounds **one**s.
![alt image](images/312-5.png#center)

For example:

* If we were bursting the first value `3`, we would compute the `i`th balloon and add to it the result from the right subarray. The formula would look like this: `nums[l - 1] * 3 * nums[r + 1] + dp[l + 1][r]`:
  ![alt image](images/312-6.png#center)
* If we were bursting the middle value `1`, we would compute the `i`th balloon and add to it the result of the left and right subarrays. The formula would look like this: `nums[l - 1] * 1 * nums[r + 1] + dp[i + 1][r] + dp[l][i - 1]`.
  ![alt image](images/312-7.png#center)

We continue to compute the result using these formulas until we reach the end of the array.

```swift
func maxCoins(_ nums: [Int]) -> Int {
    var nums = [1] + nums + [1]
    var cache: [[Int]: Int] = [:]

    func dfs(_ l: Int, _ r: Int) -> Int {

        if l > r {
            return 0
        }

        if let cached = cache[[l, r]] {
            return cached
        }

        cache[[l, r]] = 0
        for i in l ..< r + 1 {
            var coins = nums[l - 1] * nums[i] * nums[r + 1]
            coins += dfs(l, i - 1) + dfs(i + 1, r)
            cache[[l, r]]! = max(cache[[l, r]]!, coins)
        }

        return cache[[l, r]]!
    }

    return dfs(1, nums.count - 2)
}
```

#### Time/ Space complexity

* Time complexity: O(n^3)
* Space complexity: O(n^2)

#### Thank you for reading! 😊
