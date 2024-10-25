+++
title = 'LeetCode - Blind 75 - Two Sum'
date = 2024-10-25T07:16:07+03:00
tags = ["LeetCode", "Blind 75", "Two Sum", "Swift"]
draft = false
+++

### The Problem
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to the `target`.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

#### Example
``` 
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

Follow-up: Can you come up with an algorithm that has less than O(N^2) time complexity?

### Brute Force Solution
```swift
func twoSum(nums: [Int], target: Int) -> [Int] {
    for i in 0 ..< nums.count {
        for j in i + 1 ..< nums.count {
            if nums[i] + nums[j] == target {
                return [i, j]
            }
        }
    }

    return []
}
```

#### Explanation
The brute force solution for this problem is to use two loops, iterating through all of the current and next elements, finding the sum between them, and comparing the result to the `target`.  
This solution is not very efficient and has an O(N^2) time complexity.

##### Time/Space Complexity
- Time complexity: O(N^2)
- Space complexity: O(1)

### Solution - 2
```swift
func twoSum(nums: [Int], target: Int) -> [Int] {
    var hashmap: [Int: Int] = [:]

    for i in 0 ..< nums.count {
        let num = nums[i]
        let diff = target - num
        if hashmap[diff] != nil {
            return [hashmap[diff]!, i]
        }
        hashmap[num] = i
    }

    return []
}
```

#### Explanation
Solution 2 has an O(N) time complexity and an O(N) space complexity by taking advantage of a `hashmap`. It stores `i` to its `num` value at the end of each iteration. Before storing, it calculates the `diff` and checks if the `diff` value exists in `hashmap`; if it does, it returns an array with the index from `hashmap[diff]` and the current index `i`.

##### Time/Space Complexity
- Time complexity: O(N) – because in the worst case, the algorithm needs to iterate through all elements in `hashmap`.
- Space complexity: O(N) – because the algorithm needs additional space to store `i` and `num`.

#### Thank you for reading! 😊
