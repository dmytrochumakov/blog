+++
title = 'LeetCode - Blind 75 - Longest Consecutive Sequence'
date = 2024-11-10T07:10:01+03:00
tags = ["LeetCode", "Blind 75", "Longest Consecutive Sequence", "Swift"]
draft = false
+++

### The Problem
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
You must write an algorithm that runs in `O(n)` time.

> A **consecutive sequence** is a sequence of elements in which each element is exactly 1 greater than the previous element.

#### Examples
```swift
Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

```swift
Input: nums = [0, 3, 7, 2, 5, 8, 4, 6, 0, 1]
Output: 9
```

#### Constraints
* 0 <= nums.length <= 10^5
* -10^9 <= nums[i] <= 10^9

### Brute Force Solution
```swift
func longestConsecutive(_ nums: [Int]) -> Int {
    var res = 0
    let setOfNums: Set<Int> = Set(nums)

    for num in nums {
        var streak = 0
        var curr = num

        while setOfNums.contains(curr) {
            streak += 1
            curr += 1
        }

        res = max(res, streak)
    }

    return res
}
```

#### Explanation
Let's start with the brute force solution. We need to increment each element by `1` to check if the incremented value exists in our input array. For example, if we have an array `nums` with `[3, 2, 1, 100, 5]`, the longest consecutive sequence will be `3`.  
If you look closely at the array `[3, 2, 1, 100, 5]`, it has a sequence of `2, 1`, which when incremented forms elements that already exist in the array `(1 -> 2, 2 -> 3)`. By counting the number of increments, you can determine the longest sequence. The solution above does exactly that:
- Iterates through the array of `nums`
- Checks if the element exists in the array and updates `streak` by `1`
- Calculates the max of `res` and `streak`
- Returns the result

This solution uses a `set` to avoid duplicate values, which could lead to incorrect results.

##### Time/Space Complexity
* Time complexity: O(n^2)
* Space complexity: O(n)

### Solution - 2 - Sorting
```swift
func longestConsecutive(_ nums: [Int]) -> Int {
    if nums.isEmpty {
        return 0
    }

    let n = nums.count
    var nums = nums

    nums.sort()

    var res = 0
    var curr = nums[0]
    var streak = 0
    var i = 0

    while i < n {
        if curr != nums[i] {
            curr = nums[i]
            streak = 0
        }

        while i < n && nums[i] == curr {
            i += 1
        }

        streak += 1
        curr += 1
        res = max(res, streak)
    }

    return res
}
```

#### Explanation
The next way to solve this problem is by sorting. Sorting will transform your array, for example `[3, 2, 1, 100, 5]` to `[1, 2, 3, 5, 100]`. In this case, the longest sequence will be on the left side of the array, so you can iterate from left to right to find the result.

The condition `if curr != nums[i]` checks if `curr` value is not the same as the element `nums[i]` and resets the `streak`. The following steps are similar to the brute force solution.

##### Time/Space Complexity
* Time complexity: O(n log n)
* Space complexity: O(1)

### Solution - 3 - Optimal
```swift
func longestConsecutive(_ nums: [Int]) -> Int {
    var longestSCount = 0
    let setOfNums: Set<Int> = Set(nums)

    for num in setOfNums {
        if !setOfNums.contains(num - 1) {
            var length = 0
            while setOfNums.contains(num + length) {
                length += 1
            }
            longestSCount = max(length, longestSCount)
        }
    }

    return longestSCount
}
```

#### Explanation
Let's look at the optimal solution using the example input `[1, 2, 3, 5, 100]`. When you look at it, you can recognise patterns such as elements `2, 3` having a neighbor on the left side. If you decrement each of them by `1`, you will see that values (`1 <- 2`, `2 <- 3`) have a neighbor. After that, all you need is to calculate the length of the longest sequence.

This solution also uses a `set` to avoid duplicate values, which would cause incorrect results.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
