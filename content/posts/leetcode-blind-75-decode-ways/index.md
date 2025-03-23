+++
title = "LeetCode - Blind 75 - Decode Ways"
date = 2025-03-23T08:03:03+03:00
ags = ["LeetCode", "Blind 75", "Decode Ways", "Swift"]
draft = false
+++

### The Problem  
You have intercepted a secret message encoded as a string of numbers. The message is decoded via the following mapping:  
- `"1"` → `'A'`  
- `"2"` → `'B'`  
- …  
- `"25"` → `'Y'`  
- `"26"` → `'Z'`  

However, while decoding the message, you realize that there are many different ways to decode it because some codes are contained within others (e.g., `"2"` and `"5"` vs. `"25"`).  

For example, `"11106"` can be decoded into:  
- `"AAJF"` with the grouping `(1, 1, 10, 6)`  
- `"KJF"` with the grouping `(11, 10, 6)`  
- The grouping `(1, 11, 06)` is invalid because `"06"` is not a valid code (only `"6"` is valid).  

> **Note**: There may be strings that are impossible to decode.  

Given a string `s` containing only digits, return the number of ways to decode it. If the entire string cannot be decoded in any valid way, return `0`.  

The test cases are generated so that the answer fits in a 32-bit integer.  

#### Examples  

```swift
Input: s = "12"
Output: 2
Explanation:
"12" could be decoded as "AB" (1 2) or "L" (12).
```

```swift
Input: s = "226"
Output: 3
Explanation:
"226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

```swift
Input: s = "06"
Output: 0
Explanation:
"06" cannot be mapped to "F" because of the leading zero ("6" is different from "06"). In this case, the string is not a valid encoding, so return 0.
```

#### Constraints  
- `1 <= s.length <= 100`  
- `s` contains only digits and may contain leading zeros.  

### Depth-First Search Solution  

```swift
func numDecodings(_ s: String) -> Int {
    let n = s.count
    let sArray = Array(s)
    var dp: [Int: Int] = [n: 1]

    func dfs(_ i: Int) -> Int {
        if let cachedResult = dp[i] {
            return cachedResult
        }

        if sArray[i] == "0" {
            return 0
        }

        var res = dfs(i + 1)

        if i + 1 < n && (sArray[i] == "1" || (sArray[i] == "2" && "0123456".contains(sArray[i + 1]))) {
            res += dfs(i + 2)
        }

        dp[i] = res
        return res
    }

    return dfs(0)
}
```  

#### Explanation  
From the problem description, we learn that each character can be decoded to a specific letter.  

From the first example, we see that `"12"` can be decoded as `"AB"` because `"1"` maps to `"A"` and `"2"` maps to `"B"`, but it can also be decoded as `"L"`. As a result, we return `2` as the output.  

Looking at the example above, we see that the complexity of this problem comes from double digits. Single digits from `1` to `9` can be easily mapped, but when we have double digits, we must make a decision between two possible interpretations. So, we will use a **decision tree** to determine all possible decodings from the input.  

The only issue arises with edge cases. For example, when the input is `"06"`, `"0"` does not map to any letter. If a string starts with `"0"`, it is invalid and should return `0` because it cannot be decoded in any way.  

Similarly, when we have an input like `"27"`, we cannot include it in the two-character mapping since we only allow numbers `<= 26` (as the alphabet has only 26 letters).  

Let’s analyze an example with input `"121"` and try to solve it using a brute-force approach:  
![alt image](images/p-91.png#center)  

Starting from the beginning:  
- We can take `"1"` by itself.  
- We can also take the first two characters `"12"`.  
- From position `"1"`, we can take `"2"` or `"21"`.  
- When we reach `"21"`, we can’t make further decisions since we've reached the end of the string.  
- From `"2"`, we can take `"1"`, and we reach the end.  
- If we start from `"12"`, we are left with one character `"1"`.  

Thus, there are **three** different ways to solve this case.  

We will use **Depth-First Search (DFS)** and store results in a hash map to reduce the overall time complexity.  

#### Time/Space Complexity  
- **Time complexity**: `O(n)`  
- **Space complexity**: `O(n)`  

### Dynamic Programming Solution  

```swift
func numDecodings(_ s: String) -> Int {
    let n = s.count
    let sArray = Array(s)
    var dp: [Int: Int] = [n: 1]

    for i in stride(from: n - 1, through: 0, by: -1) {
        if sArray[i] == "0" {
            dp[i] = 0
        } else {
            dp[i] = dp[i + 1]
        }

        if i + 1 < n && (sArray[i] == "1" || (sArray[i] == "2" && "0123456".contains(sArray[i + 1]))) {
            dp[i]! += dp[i + 2]!
        }
    }

    return dp[0]!
}
```  

#### Explanation  
In the previous solution, we used **recursion** and additional memory to solve the problem, but we can actually optimize it using a **true dynamic programming approach**.  

We split the problem into smaller subproblems and find a solution iteratively.  

For example, with input `"121"`:  
![alt image](images/p-91-1.png#center)  
- We take the first `"1"` and ask how many ways we can decode the remaining `"21"`.  
- If we take `"12"`, we then ask how many ways we can decode `"1"`.  

Each subproblem corresponds to some portion of the string.  

This solution takes `O(n)` space, but we can optimize further and solve it in `O(1)` space by **removing the hash map** and using **two variables** instead.  

#### Time/Space Complexity  
- **Time complexity**: `O(n)`  
- **Space complexity**: `O(n)`  

### Dynamic Programming (Space-Optimized) Solution  

```swift
func numDecodings(_ s: String) -> Int {
    let n = s.count
    let sArray = Array(s)
    var dp = 0
    var dp2 = 0
    var dp1 = 1

    for i in stride(from: n - 1, to: -1 , by: -1) {
        if sArray[i] == "0" {
            dp = 0
        } else {
            dp = dp1
        }

        if (i + 1 < n && (sArray[i] == "1" ||
                          sArray[i] == "2" && "0123456".contains(sArray[i + 1]) )
        ) {
            dp += dp2
        }
        let tmpDp = dp
        let tmpDp1 = dp1
        dp = 0
        dp1 = tmpDp
        dp2 = tmpDp1
    }

    return dp1
}
```  

#### Explanation  
When decoding an entire string, we:  
![alt image](images/p-91-2.png#center)  
- Consider `"1"` alone and shift the pointer to `"21"` to solve that subproblem.  
- Consider `"12"` together and solve the subproblem `"1"`.  

To compute `dp[i]`, we only need `dp[i + 1]` and `dp[i + 2]`, so instead of storing all values, we use just **two variables**.  

#### Time/Space Complexity  
- **Time complexity**: `O(n)`  
- **Space complexity**: `O(1)`  

#### Thank you for reading! 😊
