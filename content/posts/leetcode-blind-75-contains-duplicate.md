+++
title = 'LeetCode - Blind 75 - Contains Duplicate'
date = 2024-10-26T07:27:13+03:00
tags = ["LeetCode", "Blind 75", "Contains Duplicate", "Swift"]
draft = false
+++

### The Problem
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

#### Example
``` 
Input: nums = [1,2,3,1]
Output: true
Explanation:
The element 1 occurs at indices 0 and 3.
```

Follow-up: Can you come up with an algorithm that has less than O(NlogN) time complexity?

### Brute Force Solution
``` swift 
func containsDuplicate(_ nums: [Int]) -> Bool {
    var nums = nums
    let N = nums.count
    if N == 0 {
        return false
    }
    nums.sort()
    var prev = nums[0]

    for i in 1 ..< N {
        let num = nums[i]
        if num == prev {
            return true
        } else {
            prev = num
        }
    }

    return false
}
```

#### Explanation
The brute force solution for this problem uses sorting to allow us to quickly detect duplicates since identical elements will be adjacent. After sorting, it iterates through all elements, checking if the current element is equal to the previous one; if it is, it returns `true` immediately. If not, it updates `prev`. If no duplicates are detected, it returns `false`.

##### Time/Space Complexity
- Time complexity: O(NlogN) or O(N^2) depending on the sorting algorithm.
- Space complexity: O(1) or O(N) depending on the sorting algorithm; some sorting algorithms may use additional space.

### Solution - 2
``` swift 
func containsDuplicate(_ nums: [Int]) -> Bool {
    var seen: Set<Int> = []

    for num in nums {
        if seen.contains(num) {
            return true
        } else {
            seen.insert(num)
        }
    }

    return false
}
``` 

#### Explanation
Solution - 2 is more optimized than brute force, trading memory for faster execution time.
- It uses a `seen` set and iterates over `nums`.
- It checks if `num` has already been seen; if it has, it returns `true`; if not, it inserts `num` into the `seen` set.
- When iteration is completed, it returns `false` because no duplicates were found.

##### Time/Space Complexity
- Time complexity: O(N) - in the worst case, the algorithm iterates over all `nums`.
- Space complexity: O(N) - extra space is needed to store `seen` values in a set.

#### Thank you for reading! 😊
