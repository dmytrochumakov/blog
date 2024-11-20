+++
title = 'LeetCode - Blind 75 - Container With Most Water'
date = 2024-11-20T06:57:55+03:00
tags = ["LeetCode", "Blind 75", "Container With Most Water", "Swift"]
draft = false
+++

### The Problem 
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.  
Find two lines that, together with the x-axis, form a container such that the container contains the most water.  
Return the maximum amount of water a container can store.  
Notice that you may not slant the container.

#### Examples

![alt image](images/question_11.jpg#center)

``` 
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by the array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

```
Input: height = [1,1]
Output: 1
```

#### Constraints
* n == height.length  
* 2 <= n <= 105  
* 0 <= height[i] <= 104  

### Brute Force Solution 
```swift
func maxArea(_ height: [Int]) -> Int {
    let n = height.count
    var res = 0

    for i in 0 ..< n {
        for j in i + 1 ..< n  {
            let areaWidth = (j - i)
            let minHeight = min(height[i], height[j])
            let area = minHeight * areaWidth
            res = max(res, area)
        }
    }

    return res
}
```

#### Explanation
Let's start with the brute force solution.  
By looking at the picture of Example 1, we can see that the area with height `7` and width `7` holds the most water. To get to this step, we compare the current and the next elements to find a solution. For example, in `[1,8,6,2,5,4,8,3,7]`, when we compare `1` and `8`, we look for the maximum area of water. We can see that the area will be `1 by 1` because the water spills to the left.  

![alt image](images/question_11_brute_force.png#center)

By following this example and continuing the iterations, we calculate the area width, minimum height, the area filled with water, and find the result.

##### Time/Space Complexity
* Time complexity: O(n^2)  
* Space complexity: O(1)  

---

### Solution 2 - Two Pointers
```swift
func maxArea(_ height: [Int]) -> Int {
    let n = height.count
    var res = 0

    var l = 0
    var r = n - 1

    while l < r {
        let areaWidth = (r - l)
        let minHeight = min(height[l], height[r])
        let area = minHeight * areaWidth
        res = max(res, area)

        if height[l] < height[r] {
            l += 1
        } else {
            r -= 1
        }
    }

    return res
}
``` 

#### Explanation
When we solved this problem using the brute force method, we saw that it could be done with two loops. Now, we can take it further and optimize it to O(n) time complexity using the two-pointer technique.  
The computation of the max area stays the same: we calculate the area width, minimum height, area, and move pointers by comparing the left and right height elements.

##### Time/Space Complexity
* Time complexity: O(n)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
