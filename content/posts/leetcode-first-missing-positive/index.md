+++
title = "LeetCode - First Missing Positive"
date = 2026-08-29T17:57:45+03:00
tags = ["LeetCode", "First Missing Positive", "Arrays_&_Hashing", "Hard", "Swift"]
draft = false
+++

### The problem

Given an unsorted integer array `nums`. Return the *smallest positive integer* that is *not present* in `nums`.

You must implement an algorithm that runs in `O(n)` time and uses `O(1)` auxiliary space.

**Example 1:**

```
Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.

```

**Example 2:**

```
Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.

```

**Example 3:**

```
Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.

```

**Constraints:**

* `1 <= nums.length <= 10^5`
* `-2^31 <= nums[i] <= 2^31 - 1`

#### Explanation
Let's begin from the brute force solution; maybe we can work from there.

When I first read the problem and looked into the examples, I was confused.

I could not understand when `3` came from in the first example and `1` in the third.

> I also missed that zero and negatives don't count.

The first thought that I came up with is to `sort` the input, but I was on the wrong path because it was very inefficient.

I saw that we only need to find a `max` value to create a `range` from `1 to max` and find the missing number.

The problem became how we can find that `range`?

After that, I thought maybe I need some preprocessing or filtering, like filtering out every value that is less than `1`.

This is what I came up with.
Since we can only work with positive numbers, I decided to `filter all negative and zero values`. Then I decided to use a `set` to check values in O(1) time.
Lastly, I decided to find the `max` value so that I can create a possible `range` with `missing` numbers. The `range` included the `max value + 1` so that if we had a number that is bigger than the current one, we could include it.

```swift
func firstMissingPositive(_ nums: [Int]) -> Int {
    var filteredNums: [Int] = []
    for num in nums {
        if num > 0 {
            filteredNums.append(num)
        }
    }
    
    if filteredNums.isEmpty {
        return 1
    }
    
    let filteredSet = Set(filteredNums)
    let maxNum = filteredNums.max()!
    
    for num in 1 ... maxNum + 1 {
        if !filteredSet.contains(num) {
            return num
        }
    }
    
    return -1
}
```

This solution will pass on LeetCode, but it does not satisfy our requirement of constant space, as it takes O(n) space.

### Negative Marking Solution

#### Explanation

With a little bit of help, I was able to find how we can solve this problem in linear time and constant space. The solution is called negative marking.

In order to get constant memory, we are going to create three loops and do some in-memory preprocessing.

The idea is to use the negative sign as a flag that tells us `i + 1` is in the array. The problem is that *we could have negative numbers in the input*.

To make it work, in the first loop *we are going to replace all negative values with zero*.

The second loop *marks numbers that are present in the array with a negative sign*.

Lastly, the third loop *checks for the first non-negative index that is indicating that the value is not present in the array*.

The return `n + 1` means that all numbers are negative and present in nums, and the result is outside of the current range.

#### Code

```swift
func firstMissingPositive(_ nums: [Int]) -> Int {
    var nums = nums
    let n = nums.count
    
    for i in 0 ..< n {
        if nums[i] < 0 {
            nums[i] = 0
        }
    }
    
    for i in 0 ..< n {
        let val = abs(nums[i])
        if 1 <= val && val <= n {
            if nums[val - 1] > 0 {
                nums[val - 1] *= -1
            } else if nums[val - 1] == 0 {
                nums[val - 1] = -1 * (n + 1)
            }
        }
    }
    
    for i in 1 ... n {
        if nums[i - 1] >= 0 {
            return i
        }
    }
    
    return n + 1
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
