+++
title = "LeetCode - Combinations"
date = 2026-10-01T18:20:58+03:00
tags = ["LeetCode", "Combinations", "Backtracking", "Medium", "Swift"]
draft = false
+++

### The problem

Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers chosen from the range_ `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.

```

**Example 2:**

```
Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.

```

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`

#### Explanation

Before diving into the explanation, one quick but very important note:

> Combinations ignore order, so `[1, 2]` and `[2, 1]` are considered the same and should appear once.

My first thought was to use two loops, but I quickly realized that it won't be enough, as it is impossible to produce every possible combination of size `k`.

When we are talking about combinations, permutations, and subsets, usually it means backtracking.

Backtracking means recursion, and with recursion, the rule of thumb is to always start from a base case.

> If you want to better understand backtracking, you can print out values. This way, you will be able to see a clear picture.

So far, we learned that we need to handle base cases. There are two of them:
- When `arr.count` reaches `k` - meaning that we no longer need to add new elements to `arr`, and we can store those values in the `result`.
- The second one is when `num` is higher than `n` - meaning we exceeded our constraints `[1..n]` and we cannot continue.

![alt image](images/77.png#center)

Everything else is just usual backtracking, where we `add` a new value and increment `num` and go as far as we can, then we `remove` to make room for a new incremented value and continue the same process again.

Eventually, we will get to the point where we have iterated through the entire range from `1` to `n`, and we will return the result.

### Backtracking Solution

#### Code
```swift
func combine(_ n: Int, _ k: Int) -> [[Int]] {
    var res: [[Int]] = []
    
    func backtrack(_ num: Int, _ arr: [Int]) {
        if arr.count == k {
            res.append(arr)
            return
        }
        
        if num > n {
            return
        }
        
        var arr = arr
        
        arr.append(num)
        backtrack(num + 1, arr)
        
        arr.removeLast()
        backtrack(num + 1, arr)
    }
    
    backtrack(1, [])
    
    return res
}
```

#### Time/ Space complexity
* Time complexity: O(k * (n! / (k! * (n-k)!)))
* Space complexity: O(k * (n! / (k! * (n-k)!)))

#### Thank you for reading! 😊
