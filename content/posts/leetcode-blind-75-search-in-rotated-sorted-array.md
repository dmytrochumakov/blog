+++
title = 'LeetCode - Blind 75 - Search in Rotated Sorted Array'
date = 2024-12-11T07:11:25+03:00
tags = ["LeetCode", "Blind 75", "Search in Rotated Sorted Array", "Swift"]
draft = false
+++

### The problem  
There is an integer array `nums` sorted in ascending order (with distinct values).  
Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k (1 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.  
Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.  
You must write an algorithm with `O(log n)` runtime complexity.

#### Examples
``` 
Input: nums = [4,5,6,7,0,1,2], target = 0  
Output: 4  
```

``` 
Input: nums = [4,5,6,7,0,1,2], target = 3  
Output: -1  
```

``` 
Input: nums = [1], target = 0  
Output: -1  
```

#### Constraints
* 1 <= nums.length <= 5000  
* -10⁴ <= nums[i] <= 10⁴  
* All values of `nums` are unique.  
* `nums` is an ascending array that is possibly rotated.  
* -10⁴ <= target <= 10⁴  

---

### Brute force solution  
```swift
func search(_ nums: [Int], _ target: Int) -> Int {  
    for i in 0 ..< nums.count {  
        if nums[i] == target {  
            return i  
        }  
    }  
    return -1  
}
```

#### Explanation  
The straightforward way to solve this problem is by iterating over all elements in the input and comparing each element with the target so that we can find the possible result. It is not a very efficient solution as it will take `O(n)` time, but we can achieve `O(log n)` time by using binary search.

#### Time/space complexity  
* Time complexity: O(n)  
* Space complexity: O(1)  

---

### Solution - 2 - Binary Search  
```swift
func search(_ nums: [Int], _ target: Int) -> Int {  
    var l = 0  
    var r = nums.count - 1  

    // Find the pivot  
    while l < r {  
        let m = (l + r) / 2  
        if nums[m] > nums[r] {  
            l = m + 1  
        } else {  
            r = m  
        }  
    }  

    let pivot = l  

    // Search in the left part  
    let res = binarySearch(0, pivot - 1, nums, target)  
    if res != -1 {  
        return res  
    }  

    // Search in the right part  
    return binarySearch(pivot, nums.count - 1, nums, target)  
}  

func binarySearch(  
    _ left: Int,  
    _ right: Int,  
    _ nums: [Int],  
    _ target: Int  
) -> Int {  
    var left = left  
    var right = right  
    while left <= right {  
        let mid = (left + right) / 2  
        if nums[mid] == target {  
            return mid  
        } else if nums[mid] < target {  
            left = mid + 1  
        } else {  
            right = mid - 1  
        }  
    }  
    return -1  
}
```

#### Explanation  
In the example above, we are searching for the pivot that we need to determine the portion of the array (left or right) that we are going to search. This solution is much better than brute force and takes `O(log n)` time.

#### Time/space complexity  
* Time complexity: O(log n)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
