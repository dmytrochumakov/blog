+++
title = "LeetCode - 150 - Binary Search"
date = 2025-06-27T07:37:23+03:00
tags = ["LeetCode", "150", "Binary Search", "Swift"]
draft = false
+++

### The problem

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

#### Examples

```
Input: nums = [-1,0,3,5,9,12], target = 9  
Output: 4  
Explanation: 9 exists in nums and its index is 4
```

```
Input: nums = [-1,0,3,5,9,12], target = 2  
Output: -1  
Explanation: 2 does not exist in nums so return -1
```

#### Constraints

* 1 <= nums.length <= 10^4
* -10^4 < nums\[i], target < 10^4
* All the integers in nums are unique.
* nums is sorted in ascending order.

#### Explanation

Before we jump into the solution, let’s figure out what the requirements for a binary search algorithm are and how it is going to work.
![alt image](images/704.png#center)
The main requirement for binary search is that the input must be sorted. 

From the description of the problem, we learn that our input is already sorted, so we do not need to do that.

As for the algorithm, we are going to have two pointers `l` and `r`, that we initialize at the start and end of the input accordingly.

We can’t look only to the leftmost or the rightmost values because we are eliminating only one possibility, and we still need to look at the rest of them.
But if we look at the midway point, we can compare it with the `target` and check if it is smaller than the `target` or if it is larger than the `target`:

* If it is smaller than the `target`, then everything to the left is also going to be smaller than the `target`.
  ![alt image](images/704-1.png#center)
* Now we can shift our `left` pointer and update it to the value of `mid + 1`. After that, we are going to repeat the steps we just did.
* We are going to check if our value at the `mid` index equals the `target`, and it does, so we are going to return that index.
  ![alt image](images/704-2.png#center)

The algorithm takes O(log n) time because at each iteration we are dividing our input by 2.

### Recursive Binary Search Solution

```swift
func search(_ nums: [Int], _ target: Int) -> Int {
    return binarySearch(0, nums.count - 1, nums, target)
}

func binarySearch(_ l: Int, _ r: Int, _ nums: [Int], _ target: Int) -> Int {
    if l > r {
        return -1
    }
    let m = (r + l) / 2

    if nums[m] > target {
        return binarySearch(l, m - 1, nums, target)
    } else if nums[m] < target {
        return binarySearch(m + 1, r, nums, target)
    } else {
        return m
    }
}
```

#### Time/Space complexity

* Time complexity: O(log n)
* Space complexity: O(log n)

### Iterative Binary Search Solution

```swift
func search(_ nums: [Int], _ target: Int) -> Int {
    var l = 0
    var r = nums.count - 1

    while l <= r {
        let m = (r + l) / 2
        if nums[m] > target {
            r = m - 1
        } else if nums[m] < target {
            l = m + 1
        } else {
            return m
        }
    }

    return -1
}
```

#### Time/Space complexity

* Time complexity: O(log n)
* Space complexity: O(1)

#### Thank you for reading! 😊
