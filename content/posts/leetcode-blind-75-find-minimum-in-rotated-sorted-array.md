+++
title = 'LeetCode - Blind 75 - Find Minimum in Rotated Sorted Array'
date = 2024-12-09T07:05:38+03:00
tags = ["LeetCode", "Blind 75", "Find Minimum in Rotated Sorted Array", "Swift"]
draft = false
+++

### The Problem  
Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:  
* `[4,5,6,7,0,1,2]` if it was rotated `4` times.  
* `[0,1,2,4,5,6,7]` if it was rotated `7` times.  

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.  
Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.  
You must write an algorithm that runs in `O(log n)` time.

#### Examples  
``` 
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times.
```

#### Constraints  
* `n == nums.length`  
* `1 <= n <= 5000`  
* `-5000 <= nums[i] <= 5000`  
* All the integers of `nums` are unique.  
* `nums` is sorted and rotated between `1` and `n` times.  

---

### Brute Force Solution  
```swift
func findMin(_ nums: [Int]) -> Int {
    return nums.min()!
}
```

#### Explanation  
The easiest way to solve this problem is to use Swift's built-in `min` function, but it takes `O(n)` time. Underline, it just loops through the entire input and finds the minimum element.  
The more optimal way to solve this problem is to use binary search.

##### Time/Space Complexity  
* Time complexity: `O(n)`  
* Space complexity: `O(1)`  

---

### Solution - 2 - Binary Search  
```swift
func findMin(_ nums: [Int]) -> Int {
    var res = nums[0]
    var l = 0
    var r = nums.count - 1

    while l <= r {
        if nums[l] < nums[r] {
            res = min(res, nums[l])
            break
        }

        let m = (l + r) / 2
        res = min(res, nums[m])

        if nums[m] >= nums[l] {
            l = m + 1
        } else {
            r = m - 1
        }
    }

    return res
}
```  

#### Explanation  
This solution uses a slightly modified binary search; the prerequisite for binary search is that to achieve `O(log n)` time, you need sorted input.  
In this solution, we use a slightly different approach. Since we are using a rotated array as input, we use the condition  
`nums[m] >= nums[l]` to look into the left or right side of the input `nums` and update pointers. For example, if we have the rotated array `[3, 4, 5, 1, 2]`, our `middle` pointer will be `5`, and the `left` pointer will be `3`. Following the condition `nums[m] >= nums[l]`, we update the `left` pointer. When that condition is not satisfied, for example, when the `middle` pointer is at `5` and the `left` pointer is at `1`, we update the `right` pointer.

##### Time/Space Complexity  
* Time complexity: `O(log n)`  
* Space complexity: `O(1)`  

#### Thank you for reading! 😊
