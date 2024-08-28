+++
title = 'DSA - Sliding Window'
date = 2024-08-28T07:20:26+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Sliding Window", "Swift"]
draft = false
+++

### What is the sliding window technique?
The sliding window technique is a common algorithmic approach used to create a fixed-sized window that moves through the data one step at a time, typically from left to right, to perform specific operations or computations on the elements within the window.

### What is the sliding window algorithm?
The sliding window algorithm is a method for finding a subset of elements that satisfy certain conditions in a given problem.

### How does the sliding window algorithm work?
Let’s look at the "maximum sum of a subarray" problem to better understand how it works:

> **Problem:** Given an array of integers, find the maximum sum of a subarray with a fixed window size.

In this case, the sliding window algorithm uses a fixed size window that the user can pass to a function as a parameter. It iterates through all the elements inside that window by accessing the `current value`(nums[i]) and `previous value`(nums[i - k]) and calculates the `window sum`, which is needed to determine the `max sum` result.

### Code Example
```swift
func maxSubArraySum(_ nums: [Int], _ k: Int) -> Int {
    var maxSum = Int.min
    var windowSum = nums[0 ..< k].reduce(0, +)

    for i in k ..< nums.count {
        windowSum += nums[i] - nums[i - k]
        maxSum = max(maxSum, windowSum)
    }

    return maxSum
}
```

#### Thank you for reading! 😊
