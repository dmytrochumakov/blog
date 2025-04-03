+++
title = "LeetCode - Blind 75 - Longest Increasing Subsequence"
date = 2025-04-03T07:51:08+03:00
tags = ["LeetCode", "Blind 75", "Longest Increasing Subsequence", "Swift"]
draft = false
+++

### The problem  
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

> A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

#### Examples

``` 
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

#### Constraints  
* 1 <= nums.length <= 2500  
* -10⁴ <= nums[i] <= 10⁴  

Follow-up: Can you come up with an algorithm that runs in O(n log(n)) time complexity?  

### Recursion solution  
```swift
func lengthOfLIS(_ nums: [Int]) -> Int {
    let n = nums.count

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == n {
            return 0
        }

        var LIS = dfs(i + 1, j) // not include

        if j == -1 || nums[j] < nums[i] {
            LIS = max(LIS, 1 + dfs(i + 1, i)) // include
        }

        return LIS
    }

    return dfs(0, -1)
}
```

#### Explanation  
The brute-force way to solve this problem is by using recursion.  
We need to determine whether to include a value or not.  
![alt image](images/300.png#center)  
For each value, we have two choices: we can include it in our subsequence, or we don’t include it. We do this for every single value, and the time complexity for this solution will be O(2ⁿ). This is not a very efficient algorithm, but we can take the brute-force solution and modify it to get a better solution.

#### Time/Space Complexity  
* Time complexity: O(2ⁿ)  
* Space complexity: O(n)  

### Dynamic programming top-down solution  
```swift
func lengthOfLIS(_ nums: [Int]) -> Int {
    let n = nums.count
    var memo: [[Int]] = Array(repeating: Array(repeating: -1, count: n + 1), count: n)

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == n {
            return 0
        }
        if memo[i][j + 1] != -1 {
            return memo[i][j + 1]
        }

        var LIS = dfs(i + 1, j) // not include

        if j == -1 || nums[j] < nums[i] {
            LIS = max(LIS, 1 + dfs(i + 1, i)) // include
        }
        memo[i][j + 1] = LIS
        return LIS
    }

    return dfs(0, -1)
}
```

#### Explanation  
We can optimize our brute-force solution by using caching, which will help improve our time complexity.  
Let's look at an example with input `[1, 2, 4, 3]`  
![alt image](images/300-1.png#center)  

We have four decisions: we can start at index `0`, `1`, `2`, or `3`. We also know the values at these indices and can find initial subsequences.  
So we can go down from value `[1]` and continue subsequences.  

- Now we can say we used index `0`, and we can choose from indices `1`, `2`, and `3`. All values at these indices satisfy the increasing order, so we can continue our decision tree.  
- For index `1`, we can choose values from indices `2` and `3`.  
- If we try to go down from index `2`, we see that we can't add a value to our subsequence because it is no longer increasing. We mark this as reaching the limit.  
- We also cannot continue the path from index `3` because no values come after it, so we reach the limit again and cannot increase it anymore.  

Now let's see how it looks after adding caching:  
![alt image](images/300-2.png#center)  

- If we try to continue creating a subsequence from index `3`, we see that we cannot because it is the last index and no more elements come after it. So `LIS[3] = 1`, and we do not need to repeat this work.  
- The same applies to index `2`, where `LIS[2] = 1`, because we cannot add a next value (`3 < 4`).  
- When we move to index `1`, we do not need to calculate it again because we already did when calculating index `0`. So `LIS[1] = 2`, and we do not need to go down this path again as it would be repeated work.  

When our depth-first search reaches the root, we see that the longest subsequence will be `LIS[0] = 3`.  
![alt image](images/300-3.png#center)  

This solution improves the time complexity from O(2ⁿ) to O(n²), but it increases space complexity to O(n²). We can further optimize space complexity.  

#### Time/Space Complexity  
* Time complexity: O(n²)  
* Space complexity: O(n²)  

### Dynamic programming bottom-up solution  
```swift
func lengthOfLIS(_ nums: [Int]) -> Int {
    let n = nums.count
    var lis = Array(repeating: 1, count: n)

    for i in stride(from: n - 1, to: -1, by: -1) {
        for j in i + 1 ..< n {
            if nums[i] < nums[j] {
                lis[i] = max(lis[i], 1 + lis[j])
            }
        }
    }

    return lis.max()!
}
```

#### Explanation  
From the previous solution, we learned that we can optimize space complexity using a bottom-up dynamic programming approach.  

In this approach, using the example `[1, 2, 4, 3]`, we start at index `3` and work backwards to index `0`.  
![alt image](images/300-4.png#center)  

- We start at index `3` and find the longest increasing subsequence for this index. Since no values come after index `3`, this is our base case, so `LIS[3] = 1`.  
- The longest increasing subsequence for index `2` could be just `1`, or it could be `1 + LIS[3]`. However, we can only do `1 + LIS[3]` if `nums[2] < nums[3]`. In our case, this is false because `4 > 3`, so `LIS[2] = 1`.  
- When we move to index `1`, we have a choice of value `1`. We also have a choice of `1 + LIS[2]` because `nums[1] < nums[2]`, and `1 + LIS[3]` because `nums[1] < nums[3]`.  
- Now, to find the longest increasing subsequence starting from index `0`, we use all calculated values from previous steps and add them together. Since all values greater than the value at index `0` are in increasing order, `LIS[0] = 3`.  

This is a much better solution in terms of space complexity.  

#### Time/Space Complexity  
* Time complexity: O(n²)  
* Space complexity: O(n)  

#### Thank you for reading! 😊
