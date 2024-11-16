+++
title = 'LeetCode - Blind 75 - 3Sum'
date = 2024-11-16T07:20:13+03:00
tags = ["LeetCode", "Blind 75", "3Sum", "Swift"]
draft = false
+++

### The Problem 
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.  
Notice that the solution set must not contain duplicate triplets.

#### Examples
``` 
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

#### Constraints
* 3 <= nums.length <= 3000
* -10⁵ <= nums[i] <= 10⁵

---

### Brute Force Solution 
```swift 
func threeSum(_ nums: [Int]) -> [[Int]] {
    var nums = nums
    let n = nums.count
    var res: Set<[Int]> = []

    nums.sort()

    for i in 0 ..< n {
        for j in i + 1 ..< n {
            for k in j + 1 ..< n {
                if nums[i] + nums[j] + nums[k] == 0 {
                    let val = [nums[i], nums[j], nums[k]]
                    res.insert(val)
                }
            }
        }
    }

    return Array(res)
}
```

#### Explanation
Let's start with the brute force solution.  
According to the problem, we need to find triplet elements that, when added together, equal `0`. By looking at examples, the first thing that comes to mind is to use three loops, add each of the elements, and compare the result with `0`. If the result equals `0`, we add the elements to the result.

##### Time/Space Complexity
* **Time complexity:** O(n³)  
* **Space complexity:** O(m), where `m` is the number of triplets.

---

### Solution 2 - Optimal
```swift 
func threeSum(_ nums: [Int]) -> [[Int]] {
    var nums = nums
    let n = nums.count
    var res: [[Int]] = []

    nums.sort()

    for (i, num) in nums.enumerated() {

        if i > 0 && num == nums[i - 1] {
            continue
        }

        var l = i + 1
        var r = n - 1

        while l < r {
            let tSum = num + nums[l] + nums[r]

            if tSum > 0 {
                r -= 1
            } else if tSum < 0 {
                l += 1
            } else {
                res.append([num, nums[l], nums[r]])
                l += 1
                while nums[l] == nums[l - 1] && l < r {
                    l += 1
                }
            }
        }
    }

    return res
}
``` 

#### Explanation
The time complexity of O(n³) is not exactly blazingly fast, so we need a better approach. An optimal solution is exactly what we are looking for.  

Using sorting, we can detect duplicate values. For example, before sorting: `[-1,0,1,2,-1,-4]`, and after sorting: `[-4, -1, -1, 0, 1, 2]`. We can iterate and check in the base case if the current value is equal to the previous one, and if it is, skip and move to the next iteration.  

We can find the second and third elements by using the two-pointer technique. We determine which pointer to move by calculating the total sum of the triplet and comparing it to `0`.

##### Time/Space Complexity
* **Time complexity:** O(n²)  
* **Space complexity:** O(1) or O(n), depending on the sorting algorithm.

#### Thank you for reading! 😊
