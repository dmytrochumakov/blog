+++
title = "LeetCode - Blind 75 - Jump Game"
date = 2025-04-13T07:48:19+03:00
tags = ["LeetCode", "Blind 75", "Jump Game", "Swift"]
draft = false
+++

### The problem 
You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

#### Examples

``` 
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

#### Constraints
* 1 <= nums.length <= 10⁴
* 0 <= nums[i] <= 10⁵

#### Explanation
From the description of the problem, we learn that we start from the start index, and for **every element in the array**, the number at the position represents the **max jump length that we can perform at every single position**.  
Basically, this means for input with `nums = [2,3,1,1,4]`  
![alt image](images/55.png#center)

- From the first position, we can jump of length 2 or we can do a jump with length 1. We don’t have to do the max jump length; we can choose to do a 1 jump.  
- Now we are at the position of `3`. Now we can do a jump of length 1, or we can do a jump of length 2, or a jump of length 3, and with length 3 we reach the end, therefore we can return `true`.

### Recursion Solution 
``` swift 
func canJump(_ nums: [Int]) -> Bool {
    let n = nums.count

    func dfs(_ i: Int) -> Bool {
        if i == n - 1 {
            return true
        }
        let end = min(n - 1, i + nums[i])
        for j in i + 1 ..< end + 1 {
            if dfs(j) {
                return true
            }
        }
        return false
    }

    return dfs(0)
}
```

#### Explanation
Let’s look at another example with `nums=[3,2,1,0,4]`  
![alt image](images/55-1.png#center)

> Values on the edges represent jump length; values below represent indices.

If we visualize the example as a decision tree based on indices:  
- We can see that from position `0` we can make three jumps: jump of length 1, 2, and 3.  
- At index 1, we can take `2` jumps: one decision with a jump of length 1, another jump with length 2.  
- At index 2 with value `1`, we can take only 1 jump.  
- At index 3, we have a value of `0`, which basically means that we can’t take a jump and this path is a dead end for us.

This solution is using recursion, and the time complexity will be O(n!), which is very slow. But we don’t actually need to do it because, as you can see, we have a lot of repetitive work and we can optimize it by adding caching to it.

#### Time/ Space complexity
* Time complexity: O(n!)
* Space complexity: O(n)

### Dynamic Programming Top-Down Solution 
``` swift 
func canJump(_ nums: [Int]) -> Bool {
    let n = nums.count
    var memo: [Int: Bool] = [:]

    func dfs(_ i: Int) -> Bool {
        if memo[i] != nil {
            return memo[i]!
        }
        if i == n - 1 {
            return true
        }
        if nums[i] == 0 {
            return false
        }
        let end = min(n - 1, i + nums[i])
        for j in i + 1 ..< end + 1 {
            if dfs(j) {
                memo[i] = true
                return true
            }
        }
        memo[i] = false
        return false
    }

    return dfs(0)
}
``` 

#### Explanation
In the previous solution, we learned that we can use caching to optimize our time complexity. We can do it by adding an additional hash map and using it to store the index that we visited and calculate the value for it.  
![alt image](images/55-2.png#center)

In the image with the example of `nums=[3,2,1,0,4]`, we can see that we have paths that we already visited, such as the path with index `2`, and that tells us that with caching, we can, for index `2`, return `false` because we cannot reach the end.  
You may also notice that none of our paths can reach the end. Therefore, when we try to go down to paths like `dp[2]`, `dp[3]`, `dp[1]`, and `dp[0]`, we will return `false`.

As a result, caching helped us speed up the solution to O(n²) time complexity. But we can optimize it even further with a linear time O(n) solution.

#### Time/ Space complexity
* Time complexity: O(n²)
* Space complexity:  O(n)

### Greedy Solution 
``` swift 
func canJump(_ nums: [Int]) -> Bool {
    let n = nums.count
    var goal = n - 1

    for i in stride(from: n - 1, to: -1 , by: -1) {
        if i + nums[i] >= goal {
            goal = i
        }
    }

    return goal == 0
}
```

#### Explanation
From the previous solution, we learned that we can optimize time complexity from O(n²) to O(n). We can do it by using a Greedy algorithm.  
Previously, we were trying to solve this problem from start and work to the end. Now let’s do it in reverse order: start at the `n` position and work our way to the beginning.  
![alt image](images/55-3.png#center)

Let’s work our way backwards: our goal is to reach index `4` – the last element in the array.  
- If we look at index `3` before our goal, we can see that it has value 1, and we can reach our goal from this index, meaning we can take our goal post and shift it to index `3`.  
- Now we are not trying to reach index `4`; our goal post is at index `3`.  
- Next, we look at the next position in reverse order with index `2` that has value 1, meaning that we can make a jump of length 1 and reach position with index `3`, and we can shift our goal post. We will keep doing it as long as we can reach a goal post.  
- Next, we work our way backwards again. Now we are at the position with index `1` and value 3. We can make a jump of length 3 or less, and we can reach our goal post and shift it.  
- Lastly, we are at the position with index `0` with value of 2, so we can take 2 or 1 jumps and reach our goal post. Therefore, we update our goal post and return `true`, because from the starting position all the way to the end we were able to reach the last position.

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
