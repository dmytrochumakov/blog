+++
title = "LeetCode - Blind 75 - Palindromic Substrings"
date = 2025-03-21T07:39:02+03:00
tags = ["LeetCode", "Blind 75", "Palindromic Substrings", "Swift"]
draft = false
+++

### The Problem 
Given a string `s`, return the number of palindromic substrings in it.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

#### Examples

``` 
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

#### Constraints
* 1 <= s.length <= 1000
* `s` consists of lowercase English letters.

### Brute Force Solution 

```swift 
func countSubstrings(_ s: String) -> Int {
    let n = s.count
    let sArray = Array(s)
    var res = 0

    for i in 0 ..< n {
        for j in i ..< n {
            var l = i
            var r = j

            while l < r && sArray[l] == sArray[r] {
                l += 1
                r -= 1
            }

            res += (l >= r) ? 1 : 0
        }
    }

    return res
}
```

#### Explanation
We can solve this problem by understanding how a brute-force approach works and then optimizing it.  

Let's say we are given a string `s = "aaab"`. If we start brute-forcing it, we need to go through every single substring starting from the first position, and it will look like this:  
![alt image](images/p-647.png#center)  

In total, there will be `n^2` substrings, and for each substring, we need to determine if it is a palindrome. This check takes `O(n)`, because we use two pointers to compare characters.  
Thus, the overall time complexity will be **O(n³)**.

We can improve time complexity by using a **dynamic programming** algorithm.

#### Time/Space Complexity
* **Time complexity:** O(n³)
* **Space complexity:** O(1)

### Dynamic Programming Solution 

```swift 
func countSubstrings(_ s: String) -> Int {
    let n = s.count
    let sArray = Array(s)
    var res = 0
    var dp = Array(repeating: Array(repeating: false, count: n), count: n)

    for i in stride(from: n - 1, to: -1, by: -1) {
        for j in i ..< n {
            if sArray[i] == sArray[j] && (j - i <= 2 || dp[i + 1][j - 1]) {
                dp[i][j] = true
                res += 1
            }
        }
    }

    return res
}
``` 

#### Explanation
We can optimize the brute-force solution using a **dynamic programming** approach, but this requires additional memory.  

- We use a `dp` 2D array to keep track of palindromic substrings.
- `res` is a variable to keep count of the number of palindromic substrings.

We iterate through the string in reverse order and use an inner loop to iterate from `i` to the end.  
Next, we check if the characters are equal and either:  
* The substring is of length ≤ 2, or  
* The inner substring is already a palindrome (checked using `dp[i + 1][j - 1]`).  

When we find a palindrome, we update `res`. Finally, we return `res`.

We can further **reduce space complexity** by solving this problem using the **two-pointers** technique.

#### Time/Space Complexity
* **Time complexity:** O(n²)
* **Space complexity:** O(n²)

### Two Pointers Solution 

```swift 
func countSubstrings(_ s: String) -> Int {
    let n = s.count
    let sArray = Array(s)
    var res = 0

    for i in 0 ..< n {
        // odd-length palindromes
        var l = i
        var r = i
        while l >= 0 && r < n && sArray[l] == sArray[r] {
            res += 1
            l -= 1
            r += 1
        }

        // even-length palindromes
        l = i
        r = i + 1
        while l >= 0 && r < n && sArray[l] == sArray[r] {
            res += 1
            l -= 1
            r += 1
        }
    }

    return res
}
``` 

#### Explanation
We can optimize the brute-force solution by solving it in a different way.  
Instead of checking **every substring**, we **expand outward from each character**, treating it as the center of a palindrome.  

![alt image](images/p-647-1.png#center)  

- We initialize two pointers (`l` and `r`) at the **same** position and expand outward.  
- If characters match, we count that substring as a palindrome.  
- We repeat this for both **odd-length** (single center) and **even-length** (two centers) palindromes.  

At every step, we expand `l` to the left and `r` to the right, counting palindromic substrings until we go out of bounds.

By using this **two-pointers** approach, we **avoid repeated work**, reducing overall **time complexity** and **space complexity**.

#### Time/Space Complexity
* **Time complexity:** O(n²)
* **Space complexity:** O(1)

#### Thank you for reading! 😊
