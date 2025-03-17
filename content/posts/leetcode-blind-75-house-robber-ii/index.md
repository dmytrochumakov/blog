+++
title = "LeetCode - Blind 75 - House Robber II"
date = 2025-03-17T07:28:50+03:00
tags = ["LeetCode", "Blind 75", "House Robber II", "Swift"]
draft = false
+++

### The problem  
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses in this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses are broken into on the same night.  

Given an integer array `nums` representing the amount of money in each house, return the maximum amount of money you can rob tonight without alerting the police.  

#### Examples  

``` 
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2) because they are adjacent houses.
```  

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```  

```
Input: nums = [1,2,3]
Output: 3
```  

#### Constraints  
* 1 <= nums.length <= 100  
* 0 <= nums[i] <= 1000  

### Depth-first search (memoization) solution  

```swift  
func rob(_ nums: [Int]) -> Int {
    let n = nums.count

    if n == 1 {
        return nums[0]
    }

    var memo: [Index: Int] = [:]

    func dfs(_ i: Int, _ flag: Bool) -> Int {
        if i >= n || (flag && i == n - 1) {
            return 0
        }
        if memo[Index(i, flag)] != nil {
            return memo[Index(i, flag)]!
        }
        memo[Index(i, flag)] = max(dfs(i + 1, flag),
                                   nums[i] + dfs(i + 2, flag || i == 0))
        return memo[Index(i, flag)]!
    }

    return max(dfs(0, true), dfs(1, false))
}

struct Index: Hashable {
    let i: Int
    let flag: Bool
    init(_ i: Int, _ flag: Bool) {
        self.i = i
        self.flag = flag
    }
}
```  

#### Explanation  
This problem is very similar to the first [House Robber](https://dmytros.blog/posts/leetcode-blind-75-house-robber/) problem. The main difference is that the last house is connected to the first.  
![alt image](images/p-123.png#center)  

We can reuse the solution from the first [House Robber](https://dmytros.blog/posts/leetcode-blind-75-house-robber/) problem.  

Let's take a closer look at both of these problems.  
![alt image](images/p-123-1.png#center)  

In the first House Robber problem:  
- We can find the max amount we can rob until the second house; the total will be `2`.  
- Next, we can find the max amount we can rob until the third house; the total will be `3`.  
- Lastly, when looking at the entire array, we can rob house three, skip house two, and rob house one, giving a total of `4`.  

When looking at the second House Robber problem, we see that we need to make some adjustments to reuse the solution from the first problem because the first and last houses are connected.  

We can reuse the solution from the first problem by robbing the entire array except for the last value. We can also run the reused solution on the entire array except for the first value.  
![alt image](images/p-123-2.png#center)  

One way to solve this problem is by using the DFS algorithm with memoization. We can reuse the algorithm from the first problem by adjusting it slightly. We will add an additional flag and a base case to detect a cycle between the last and first houses.  

This solution does not follow the dynamic programming category; it's a more brute-force way of solving this type of problem. We can rewrite it using true dynamic programming.  

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

---

### Dynamic programming solution  

```swift  
func rob(_ nums: [Int]) -> Int {
    let n = nums.count

    if n == 1 {
        return nums[0]
    }

    return max(helper(Array(nums[1...])),
               helper(Array(nums[0..<n - 1])))
}

func helper(_ nums: [Int]) -> Int {
    let n = nums.count

    if n == 0 {
        return 0
    }

    if n == 1 {
        return nums[0]
    }

    var dp = Array(repeating: 0, count: n)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])

    for i in 2 ..< n {
        dp[i] = max(dp[i - 1], nums[i] + dp[i - 2])
    }

    return dp.last!
}
```  

#### Explanation  
Another way to solve this problem is by using the dynamic programming algorithm.  

We will need a helper function that we can reuse from the first House Robber problem. We will call it on two subarrays:
- The first subarray excludes the first house.  
- The second subarray excludes the last house.  

In the end, we will find the `max` result.  

This solution takes O(n) space, but we can optimize memory by replacing the `dp` array with two properties.  

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

---

### Dynamic programming (space optimized) solution  

```swift  
func rob(_ nums: [Int]) -> Int {
    let n = nums.count
    return max(nums[0],
               helper(Array(nums[1...])),
               helper(Array(nums[0..<n - 1])))
}

func helper(_ nums: [Int]) -> Int {
    var rob1 = 0
    var rob2 = 0

    for n in nums {
        let newRob = max(rob1 + n, rob2)
        rob1 = rob2
        rob2 = newRob
    }

    return rob2
}
```  

#### Explanation  
Lastly, we can solve this problem using a space-optimized dynamic programming algorithm.  

Basically, we maintain two properties that store the max amount that can be robbed from the previous two houses. 
- We iterate over every house in the input array and compute the max that can be robbed up to that point by choosing between robbing house one and skipping house two, or skipping `rob1` and including `rob2`. 
- We then update our properties. As a result, `rob2` will contain the maximum amount that can be robbed.  

We also need to handle a few edge cases:  
- We need to skip the first house.  
- We also need to skip the last house.  
- The last edge case occurs when the input array contains only one element, so we need to include the first element to avoid a result of 0.  

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
