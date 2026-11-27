+++
title = "LeetCode - Minimum Size Subarray Sum"
date = 2026-11-27T22:03:01+03:00
tags = ["LeetCode", "Minimum Size Subarray Sum", "Sliding_Window", "Medium", "Swift"]
draft = false
+++

### The problem

Given an array of positive integers `nums` and a positive integer `target`, return _the **minimal length** of a_ _subarray_ _whose sum is greater than or equal to_ `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1

```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0

```

**Constraints:**

- `1 <= target <= 10^9`
- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^4`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

#### Explanation
The brute-force way to solve this problem is to use two for loops and try every possible combination. It is very slow and will take `O(n^2)` time.

We should also take care of the edge case where the sum of `nums` is less than `target`, because we cannot find any subarray whose sum is greater than or equal to `target`.

If you look closely, the brute-force solution points to the sliding window technique (`target`, `total`, `r - l + 1`).

```swift
func minSubArrayLen(_ target: Int, _ nums: [Int]) -> Int {
    if nums.reduce(0, +) < target {
        return 0
    }
    
    let n = nums.count
    var res = Int.max
    
    for l in 0 ..< n {
        var total = nums[l]
        if total >= target {
            return 1
        }
        for r in l + 1 ..< n {
            total += nums[r]
            if total >= target {
                res = min(res, r - l + 1)
                break
            }
        }
    }
    
    if res == Int.max {
        return 0
    } else {
        return res
    }
}
```

### Sliding-Window Solution

#### Explanation
The sliding window technique works in linear time and constant space. It removes inner for loop, giving us a perfomance boost.

It works because you increment the right pointer, while the left pointer stays in place until you reach or exceed the `target`.

Then you can start incrementing the left pointer until the total sum is less than `target`.

![alt image](images/209.png#center)

It is still linear time because you never visit the same number with the left pointer twice.

Eventually, you will get to the end of the array, and when you do, you will have exhausted all possible subarrays that could sum up to `target`.

![alt image](images/209-1.png#center)

#### Code
```swift
func minSubArrayLen(_ target: Int, _ nums: [Int]) -> Int {
    if nums.reduce(0, +) < target {
        return 0
    }
    
    let n = nums.count
    var total = 0
    var res = Int.max
    var l = 0
    
    for r in 0 ..< n {
        total += nums[r]
        while total >= target {
            res = min(res, r - l + 1)
            total -= nums[l]
            l += 1
        }
    }
    
    if res == Int.max {
        return 0
    } else {
        return res
    }
}
```

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(1)

In the follow-up section, they asked: if you have found an O(n) solution, try to explain an O(n*logn) solution. But I did not come up with one. I thought about sorting because it takes `O(n*logn)`, but it would not work, as we must preserve the order of `nums`, and sorting breaks it. If you could come up with one, please let me know.

#### Thank you for reading! 😊
