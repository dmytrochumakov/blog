+++
title = "LeetCode - Blind 75 - Longest Common Subsequence"
date = 2025-04-08T07:57:29+03:00
tags = ["LeetCode", "Blind 75", "Longest Common Subsequence", "Swift"]
draft = false
+++

### The problem  
Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return `0`.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.  
* For example, `”ace”` is a subsequence of `”abcde”`.

A common subsequence of two strings is a subsequence that is common to both strings.

#### Examples

```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

#### Constraints  
* 1 <= text1.length, text2.length <= 1000  
* text1 and text2 consist of only lowercase English characters.

#### Explanation  
Before we jump to coding, let's look at visual representation with some examples to better understand the problem.  
An example with input `text1 = "abcde", text2 = "ace"`  
![alt image](images/1143.png#center)

We can see that we have two equal characters `a` at the beginning of both strings. This means that if both characters match each other, then we can break it into subproblems. This also means that now we are looking into a subproblem of the remainder of both strings plus `1` because we know that the longest subsequence at least is going to be 1 since we found matching pairs of characters.

Let's look at an example if the first characters were different: input `text1 = “bbcde", text2 = "ace"`  
![alt image](images/1143-1.png#center)

We can't add 1 and find the longest subsequence because the first characters are different, but it's possible that the longest subsequence can be between `bbcde` and `ce` or it could be between `bcde` and `ace`.

Basically, what we find out based on comparing the first characters — whether they are equal or not — is that we can break the problem into subproblems and solve them.

### Recursion Solution  
``` swift 
func longestCommonSubsequence(_ text1: String, _ text2: String) -> Int {
    let text1Array = Array(text1)
    let text2Array = Array(text2)
    let text1Count = text1.count
    let text2Count = text2.count

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == text1Count || j == text2Count {
            return 0
        }
        if text1Array[i] == text2Array[j] {
            return 1 + dfs(i + 1, j + 1)
        }
        return max(dfs(i + 1, j), dfs(i, j + 1))
    }

    return dfs(0, 0)
}
```

#### Explanation  
One way to solve this problem is by using the depth-first search algorithm.

We will need to take care of a few base cases:  
- when we reach the end of one of the texts  
- when we find matching characters  

In the result, we will be returning the max of two values.

It is not a very efficient solution because it will take O(2^(m+n)) time complexity, but we can optimize it.

#### Time/ Space complexity  
* Time complexity: O(2^(m+n))  
* Space complexity: O(m+n)  
* where `m` is the length of string `text1` and `n` is the length of string `text2`

### Dynamic Programming Top-Down Solution  
``` swift 
func longestCommonSubsequence(_ text1: String, _ text2: String) -> Int {
    let text1Array = Array(text1)
    let text2Array = Array(text2)
    let text1Count = text1.count
    let text2Count = text2.count
    var memo: [IndexHelper: Int] = [:]

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == text1Count || j == text2Count {
            return 0
        }
        if memo[IndexHelper(i: i, j: j)] != nil {
            return memo[IndexHelper(i: i, j: j)]!
        }
        if text1Array[i] == text2Array[j] {
            memo[IndexHelper(i: i, j: j)] = 1 + dfs(i + 1, j + 1)
        } else {
            memo[IndexHelper(i: i, j: j)] = max(dfs(i + 1, j), dfs(i, j + 1))
        }
        return memo[IndexHelper(i: i, j: j)]!
    }

    return dfs(0, 0)
}

struct IndexHelper: Hashable {
    let i: Int
    let j: Int
}
```

#### Explanation  
We learned from the previous solution that we can optimize time complexity. We can do it by caching. Caching will help us reduce time complexity from O(2^(m+n)) to O(m*n).  
Base cases and algorithm stay the same; all that we needed to do is to add a hash map where we can store and retrieve stored results.

#### Time/ Space complexity  
* Time complexity: O(m*n)  
* Space complexity: O(m*n)  
* where `m` is the length of string `text1` and `n` is the length of string `text2`

### Dynamic Programming Bottom-Up Solution  
``` swift 
func longestCommonSubsequence(_ text1: String, _ text2: String) -> Int {
    let text1Array = Array(text1)
    let text2Array = Array(text2)
    let text1Count = text1.count
    let text2Count = text2.count

    var dp: [[Int]] = Array(repeating: Array(repeating: 0, count: text2Count + 1), count: text1Count + 1)

    for i in stride(from: text1Count - 1, to: -1, by: -1) {
        for j in stride(from: text2Count - 1, to: -1, by: -1) {
            if text1Array[i] == text2Array[j] {
                dp[i][j] = 1 + dp[i + 1][j + 1]
            } else {
                dp[i][j] = max(dp[i][j + 1], dp[i + 1][j])
            }
        }
    }

    return dp[0][0]
}
```

#### Explanation  
Another way to solve this problem is by using the dynamic programming bottom-up approach.

Let's visualize input with `text1 = "abcde", text2 = "ace"`  
![alt image](images/1143-2.png#center)

We have a 2D matrix that we are going to use as a representation of our `text1` and `text2`.  
- We start by comparing values at indices `0, 0`, and we can see that we have a match `a == a`, and we do not need to check it anymore  
- After that we are going diagonally and compare `c == b`, they are not equal, and we cannot go diagonally. Since these two characters are not equal, we need to check two different subproblems, and we have two decisions — go right or go down  
- If we go down, we can see that characters `c == c` and we can go diagonally because we want to look at the last two substrings  
- When we compare `d == e` we see that they are not equal, then we can’t go diagonally and we need to go to the right and down to find whatever max is and put it in the cell  
- We see that if we go to the right, we are going to be out of bounds, therefore the longest subsequence between a string and out-of-bounds value is going to be `0`  
- The right out-of-bounds column would be filled with `0`s. Similarly, the bottom rows also will be filled with `0`s  
- When we continue looking from `d` and `e` position, we see that we can’t go to the right because we will get `0`, and this is not our max value, let’s go down  
- When we go down, we find that `e == e` and we can look diagonally and put `1` to our row

Since we looked through all possible combinations and reached the end of the strings, we can now backtrack along the path that we came from and find the solution.  
![alt image](images/1143-3.png#center)

- We had `1` at position `e` and `e`  
- At position `d` and `e`, we went down because `d` and `e` characters did not match, so we are going to get value `1` from `e` and `e` position and put it into the position of `d` and `e`  
- Now we are going back to `c` and `c` diagonally, we are doing it because characters `c` and `c` match, so we are going to get value of `1` from position `d` and `e`, and put it to position `c` and `c` and add additional `1` to it, so now the value at position `c` and `c` equals `2`  
- From position `c` and `c` we are going up and put `2` to `b` and `c` row because characters did not match  
- Lastly, we are going back to position `a` and `a` diagonally because characters are matching each other, we pass value `2` to it from position `b` and `c`, and add additional value of `1`, so result at position `a` and `a` will be `3`, meaning that the longest common subsequence is equal to `3`.

You can see that we started from the bottom and worked our way up, that’s why this solution is called bottom-up.

#### Time/ Space complexity  
* Time complexity: O(m*n)  
* Space complexity: O(m*n)  
* where `m` is the length of string `text1` and `n` is the length of string `text2`

#### Thank you for reading! 😊
