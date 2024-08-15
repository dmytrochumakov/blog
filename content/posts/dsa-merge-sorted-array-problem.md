+++
title = 'DSA - Merge Sorted Array Problem'
date = 2024-08-15T07:04:16+03:00
tags = ["Data Structures", "Algorithms", "DSA", "LeetCode", "Swift"]
draft = false
+++

### Introduction
In the [previous chapter](https://dmytros.blog/posts/data-structures-and-algorithms-arrays-swift/), we discussed Data Structures and Algorithms, delved into an overview of Dynamic Arrays, and solved the "Remove Element" problem.

In this article, I'm going to show one of the ways to solve the [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150) problem.

### Problem 
You are given two integer arrays, `nums1` and `nums2`, sorted in non-decreasing order, and two integers, `m` and `n`, representing the number of elements in `nums1` and `nums2`, respectively. Merge `nums1` and `nums2` into a single array sorted in non-decreasing order. The final sorted array should not be returned by the function but should instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

### Solution 
```swift 
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        if m == 0  {
            nums1 = nums2
        } else if n == 0  {   
            let tmp = nums1         
            nums1 = tmp
        } else {
            var num1result: [Int] = []
            for i in 0 ..< m {
                num1result.append(nums1[i])
            }
            var num2result: [Int] = []
            for i in 0 ..< n {
                num2result.append(nums2[i])
            }
            var result = num1result + num2result
            result.sort()
            nums1 = result
        }
    }
}
``` 

### Approach 
The brute-force way to solve this problem is to check the lengths of `m` and `n`, and if it's `0`, assign a copy of `nums2` or `nums1` appropriately. In case `m` and `n` are not equal to `0`, create additional arrays and loop through `nums1` and `nums2`, taking into account the number of elements in the `m` and `n` properties.

- **Time Complexity**: O(logn) because it uses the underlying sort method.  
- **Space Complexity**: O(m+n) because it uses additional arrays for `num1` and `num2` results.

### Optimized Approach
I found a more optimal solution that uses **Time Complexity**: O(m + n), **Space Complexity**: O(1), and a three-pointer technique. In this solution, the `i` pointer reads values from `nums1`, the `j` pointer reads values from `nums2`, and the `w` pointer writes values to `nums1`. Loop backward through the sum of `m + n` elements and update the `nums1` values. Decrement the `i` pointer if `i` is greater than or equal to `0` and the value of `nums1[i]` is greater than `nums2[j]`. If not, update the value in `nums1` and decrement the `j` pointer.

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        var i = m - 1
        var j = n - 1

        for w in stride(from:n+m-1, through:0, by:-1) {
            if j < 0 {
                break
            }
            if i >= 0 && nums1[i] > nums2[j] {
                nums1[w] = nums1[i]
                i -= 1
            } else {
                nums1[w] = nums2[j]
                j -= 1
            }
        }
    }
}
```

#### Thank you for reading! 😊
