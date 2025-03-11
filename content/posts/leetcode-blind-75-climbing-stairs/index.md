+++
title = "LeetCode - Blind 75 - Climbing Stairs"
date = 2025-03-11T07:43:01+03:00
tags = ["LeetCode", "Blind 75", "Climbing Stairs", "Swift"]
draft = false
+++

### The problem  
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

#### Examples  

``` 
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

#### Constraints  
* 1 <= n <= 45  

### Brute Force Solution  

```swift  
func climbStairs(_ n: Int) -> Int {
    func dfs(_ i: Int) -> Int {
        if i >= n {
            return i == n ? 1 : 0
        }
        return dfs(i + 1) + dfs(i + 2)
    }

    return dfs(0)
}
```

#### Explanation  
Let's visualize and look at the first example `Input: n = 2`,  
![alt image](images/p-70.png#center)  
You can see that we can take two single steps or we can take one double step at once and find out that we have two different ways to solve this problem.

Let's look at another example `Input: n = 3`  
![alt image](images/p-70-1.png#center)  
You can see that we can find the solution by taking `1` step and `2` steps, `2` steps and `1` step, or taking `1` single step three times.

From the visualization above, you can see that it’s hard to visualize examples like a bar graph. A better way to do it is by visualizing it as a decision tree.  
For example, `Input: n = 5`  
![alt image](images/p-70-2.png#center)  

One way to solve this problem is by using a depth-first search algorithm and recursively finding all possible solutions. However, it could be problematic because it uses O(2^n) time complexity, which could be very slow and even exceed all available memory due to recursion stack calls.

##### Time/Space Complexity  
* **Time complexity:** O(2^n)  
* **Space complexity:** O(n)  

---

### Dynamic Programming (Bottom-Up) Solution  

```swift  
func climbStairs(_ n: Int) -> Int {
    if n <= 2 {
        return n
    }

    var dp = Array(repeating: 0, count: n + 1)
    dp[1] = 1
    dp[2] = 2
    for i in 3 ..< n + 1 {
        dp[i] = dp[i - 1] + dp[i - 2]
    }
    return dp[n]
}
```  

#### Explanation  
Another way we can solve this problem is by using true dynamic programming.  
![alt image](images/p-70-3.png#center)  

As you can see, starting from `0`, the result depends on subproblem `1`, subproblem `1` depends on subproblem `2`, and so on, until it finally depends on our base case `5`. Why don’t we start from the bottom, solve our base case, and work our way up to the original problem at `0`? (This is called the bottom-up dynamic programming approach.)

We will be storing our result in an array called `dp`. We are going to have positions in `dp` from position `0` all the way up to `n`.  
![alt image](images/p-70-4.png#center)  
- Initially, at the base case of `5`, we can take only one step.  
- Now, when we solve `5`, we can solve `4` because subproblem `4` depends on subproblem `5`. From `4`, we can take one step, which leads to our result, or we can take two steps, which leads us out of bounds. As a result, we have only one way to solve subproblem `4`.  
- Then we want to know how many ways from `3` we can reach `5`. The subproblem `3` depends on two subproblems that come after it: `4` and `5`. So, we can take one step or two steps. As a result, to solve subproblem `3`, we take the next two values from `dp` and add them together.  
- We continue the same process for positions `2`, `1`, and `0`.

The complexity of the dynamic programming solution is much better than the recursive solution, taking O(n) time and O(n) space. However, we can optimize it even further to O(n) time and O(1) space.

##### Time/Space Complexity  
* **Time complexity:** O(n)  
* **Space complexity:** O(n)  

---

### Dynamic Programming (Space Optimized) Solution  

```swift  
func climbStairs(_ n: Int) -> Int {
    var one = 1
    var two = 1

    for i in 0 ..< n - 1 {
        let tmp = one
        one = one + two
        two = tmp
    }

    return one
}
```  

#### Explanation  
From the picture above, you can notice that each value in the `dp` array depends on the two values that come after it. For example, `8` depends on `5` and `3`, etc.  
As a result, we don’t need to store the entire array. Instead, we can use two different variables.  

Let’s initialize `one` and `two` properties to represent steps and help us compute the next value. Once we compute the next value, we shift `one` and `two` variables and keep doing it until we get to the result at `0`.  
![alt image](images/p-70-5.png#center)  

If we initialize `one` and `two` properties with the value `1`, we need to compute `n-1` values and return the result stored in `one`.

##### Time/Space Complexity  
* **Time complexity:** O(n)  
* **Space complexity:** O(1)  

#### Thank you for reading! 😊
