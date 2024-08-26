+++
title = 'DSA - Binary Search'
date = 2024-08-26T07:09:48+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search", "Swift"]
draft = false
+++

### What is binary search?
[Binary search](https://en.wikipedia.org/wiki/Binary_search) is an algorithm that helps find an element in a sorted array in O(log n) time.

### Why should the input be sorted before performing binary search?
The input array for binary search needs to be sorted because the algorithm eliminates half of the choices at each step. If the guessed value is greater than the target value, it knows that the right part can’t contain the target value.

### How does binary search work?
It uses the [two pointers technique](https://dmytrosblog.substack.com/p/dsa-two-pointers-technique?r=2fepxg), which helps divide the input into two halves with each iteration, compares the middle array element with the target value, and shifts the pointers based on which half contains the target value.

### Code Example
```swift
func binarySearch(_ arr: [Int], _ target: Int) -> Int? {
    var l: Int = 0
    var r: Int = arr.count - 1

    while l <= r {
        let m = (l + r) / 2
        if arr[m] < target {
            l = m + 1
        } else if arr[m] > target {
            r = m - 1
        } else {
            return m
        }
    }
    return nil
}
```

#### Thank you for reading! 😊
