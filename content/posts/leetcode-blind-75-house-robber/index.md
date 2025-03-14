+++
title = "LeetCode - Blind 75 - House Robber"
date = 2025-03-14T07:52:39+03:00
tags = ["LeetCode", "Blind 75", "House Robber", "Swift"]
draft = false
+++

### The Problem  
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. The only constraint stopping you from robbing each of them is that adjacent houses have security systems connected, and they will automatically contact the police if two adjacent houses are broken into on the same night.  

Given an integer array `nums` representing the amount of money in each house, return the maximum amount of money you can rob tonight without alerting the police.  

#### Examples  

``` 
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9), and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

#### Constraints  
* 1 <= nums.length <= 100  
* 0 <= nums[i] <= 400  

---

### Depth-First Search (Memoization) Solution  

```swift 
func rob(_ nums: [Int]) -> Int {
    let n = nums.count
    var memo: [Int: Int] = [:]

    func dfs(_ i: Int) -> Int {
        if i >= n {
            return 0
        }
        if let cached = memo[i] {
            return cached
        }
        memo[i] = max(dfs(i + 1), nums[i] + dfs(i + 2))
        return memo[i]!
    }

    return dfs(0)
}
```

#### Explanation  
From the problem description, we learn that we are given an array where each element represents a house, and they are adjacent to each other. We also learn that we can't rob every house because there is a restriction: we can only rob houses that are not adjacent to each other.  

This problem belongs to the dynamic programming category. Let's draw a decision tree using the example `nums = [1,2,3,1]`.  

![alt image](images/p-198.png#center)  

We start at `1`. We can't rob `2` because it's adjacent to `1`, so we need to decide whether to rob `3` or `1`. We choose to rob houses `1` and `3`, giving us a total of `4`.  

If we decide to rob house `2`, we can only rob house `4`, resulting in a total of `3`, which is not the optimal solution since we already found a greater total of `4`.  

![alt image](images/p-198-1.png#center)  

This is a brute-force approach. Imagine if we had more numbers—our decision tree would grow exponentially, making it very complex. We can improve this by identifying subproblems.  

Let's analyze the subproblem:  
We have two choices to maximize our total robbery amount:  
1. Rob the first house, then find the maximum from the remaining houses.  
2. Skip the first house and find the maximum in the subarray excluding the first value.  

![alt image](images/p-198-2.png#center)  

In our first decision, we rob the first value, so we skip index `1` and rob the remaining array from index `2` to `n`. In code, this looks like `rob = arr[0] + rob[2:n]`.  

If we do not rob the first house, we solve the subproblem by starting from house `1` and continuing until the end of the array.  

Each robbery decision can be broken into subproblems:  

![alt image](images/p-198-3.png#center)  

- If we rob the first house, we get a total of `1`.  
- Next, we decide whether to rob house `2`, which has a larger total of `2`.  
- Then, we decide whether to rob house `3` (which includes robbing house `1`) or just rob house `2`. The maximum choice is robbing house `3` and house `1`, giving a total of `4`.  
- Lastly, we decide between robbing house `4` and house `1`, or robbing house `4` and house `2`. The best choice is robbing house `4` and house `2`, totaling `3`.  

#### Time and Space Complexity  
* **Time Complexity**: O(n)  
* **Space Complexity**: O(n)  

---

### Dynamic Programming Solution  

```swift 
func rob(_ nums: [Int]) -> Int {
    let n = nums.count
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
We can also solve this problem using a dynamic programming approach. The main idea remains the same as explained above, but instead of recursion, we use an array and iteration.  

This approach takes **O(n) space**, but we can optimize it further to **O(1) space** by using two variables instead of an additional array.  

#### Time and Space Complexity  
* **Time Complexity**: O(n)  
* **Space Complexity**: O(n)  

---

### Dynamic Programming (Space Optimized) Solution  

```swift 
func rob(_ nums: [Int]) -> Int {
    var rob1 = 0
    var rob2 = 0

    for n in nums {
        let temp = max(n + rob1, rob2)
        rob1 = rob2
        rob2 = temp
    }

    return rob2
}
```  

#### Explanation  
We don't need an additional array to store values because we only need to track the last two computed results.  

![alt image](images/p-198-4.png#center)  

This works because we either decide to rob the fourth house and the first and second house (excluding `3`), totaling `2`,  
or we skip robbing the fourth house and rob the first, second, and third houses, totaling `4`.  

![alt image](images/p-198-5.png#center)  

We've learned that we only need to maintain the last two max values we can rob. Instead of using an array, we use two variables and iterate through `nums`, computing the maximum at each step:  
- `n + rob1` represents robbing the current house (with a gap from the previous one).  
- `rob2` represents skipping the current house and keeping the previous max.  
- We update the variables accordingly.  

#### Time and Space Complexity  
* **Time Complexity**: O(n)  
* **Space Complexity**: O(1)  

#### Thank you for reading! 😊
