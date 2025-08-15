+++
title = "LeetCode - 150 - Kth Largest Element in an Array"
date = 2025-08-15T07:29:52+03:00
tags = ["LeetCode", "150", "Kth Largest Element in an Array", "Swift"]
draft = false
+++

### The problem

Given an integer array `nums` and an integer `k`, return the `kth` largest element in the array.

> Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

#### Examples

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

#### Constraints

* 1 <= k <= nums.length <= 10^5
* -10^4 <= nums\[i] <= 10^4

#### Explanation

From the description of the problem we learn that we need to find the `kth` largest element from the given array. The `kth` element means if, for example, we had `k = 1` then we would return the `first` largest element, or if we had `k = 3` then we would return the `third` largest element.

### Sorting Solution

```swift
func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
    var nums = nums
    nums.sort()
    return nums[nums.count - k]
}
```

#### Explanation

The brute force way to solve this problem is to sort our input array and find the `kth` largest element by taking the length of the input array and subtracting it with the `k` - (`nums.count - k`), this way we can find the index of the `kth` largest element.

From a performance standpoint it’s not a very efficient solution because it will take `O(n*log(n))` time, we can find a more optimal solution.

#### Time/ Space complexity

* Time complexity: O(n\*log(n))
* Space complexity: O(1) or O(n) depending on the sorting algorithm

### Max Heap Solution  

```swift
func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
    var maxHeap: Heap<Int> = Heap(nums)
    
    var res = 0
    for _ in 0 ..< k {
        res = maxHeap.removeMax()
    }
    
    return res
}
```

#### Explanation

Another way to solve this problem is by using the max heap data structure.

* At the first step we are going to put our input inside the max heap, that will take `O(n)` time
* Next, we are going to pop from the max heap `k` times, where the pop operation will take `O(logn)` time
* Lastly, we will return the result
  
This solution is slightly better than sorting, because we only need to pop `k` times, which can be less than the length of the entire array of `nums`.

#### Time/ Space complexity

* Time complexity: O(k\*log(n))
* Space complexity: O(n)

#### Thank you for reading! 😊
