+++
title = "LeetCode - Blind 75 - Longest Palindromic Substring"
date = 2025-03-19T07:54:06+03:00
tags = ["LeetCode", "Blind 75", "HLongest Palindromic Substring", "Swift"]
draft = false
+++

### The Problem  
Given a string `s`, return the longest palindromic substring in `s`.  
> A string is palindromic if it reads the same forward and backward.

#### Examples

``` 
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

```
Input: s = "cbbd"
Output: "bb"
```

#### Constraints  
* 1 <= s.length <= 1000  
* `s` consists of only digits and English letters.

### Brute Force Solution  
```swift
func longestPalindrome(_ s: String) -> String {
    let sArray = Array(s)
    var res = ""
    var resLen = 0
    let n = s.count

    for i in 0 ..< n {
        for j in i ..< n {
            var l = i
            var r = j

            while l < r && sArray[l] == sArray[r] {
                l += 1
                r -= 1
            }

            if l >= r && resLen < (j - i + 1) {
                res = String(sArray[i ..< j + 1])
                resLen = j - i + 1
            }
        }
    }

    return res
}
```

#### Explanation  
In the first example, we are given the input `"babad"`. Let’s visualize it and try to find the longest palindromic substring.  

![alt image](images/p-3.png#center)

As you can see, we have two different palindromic substrings: `"bab"` and `"aba"`, so we could return either of these.  

The brute force solution to this problem involves checking every single substring, verifying if it's a palindrome, and finding the longest one.  

![alt image](images/p-3-1.png#center)

This solution is not very efficient because we need to scan through the entire string and check every substring. As a result, the time complexity is `O(n * n^2) -> O(n^3)`.

#### Time/Space Complexity  
* **Time complexity**: O(n³)  
* **Space complexity**: O(1)  

### Dynamic Programming Solution  
```swift
func longestPalindrome(_ s: String) -> String {
    var resIdx = 0
    var resLen = 0
    let n = s.count
    let sArray = Array(s)

    var dp: [[Bool]] = Array(repeating: Array(repeating: false, count: n), count: n)

    for i in stride(from: n - 1, to: -1, by: -1) {
        for j in i ..< n  {
            if sArray[i] == sArray[j] && (j - i <= 2 || dp[i + 1][j - 1]) {
                dp[i][j] = true
                if resLen < (j - i + 1) {
                    resIdx = i
                    resLen = j - i + 1
                }
            }
        }
    }

    return String(sArray[resIdx ..< resIdx + resLen])
}
```  

#### Explanation  
Another way to solve this problem is by using a dynamic programming algorithm.  

- We create `resIdx` to store the starting index of the result.  
- `resLen` stores the length of the result.  
- `dp` is a 2D array to keep track of palindromic substrings.  

After that, we iterate through the string in reverse and use an inner loop to iterate from `i` to the end.  

Next, we check if the characters are equal and either:  
- The substring is shorter than or equal to length `3`, or  
- The inner substring is already a palindrome.  

When we find a palindrome, we check if it's longer than the current result. If so, we update our pointers.  

Finally, we return the result substring using the calculated pointers.  

This is not a very efficient solution in terms of space complexity. We can improve it using the two-pointer technique.  

#### Time/Space Complexity  
* **Time complexity**: O(n²)  
* **Space complexity**: O(n²)  

### Two-Pointer Solution  
```swift
func longestPalindrome(_ s: String) -> String {
    var resIdx = 0
    var resLen = 0
    let n = s.count
    let sArray = Array(s)

    for i in 0 ..< n {
        // Odd length
        var l = i
        var r = i
        while l >= 0 && r < n && sArray[l] == sArray[r] {
            if (r - l + 1) > resLen {
                resIdx = l
                resLen = r - l + 1
            }
            l -= 1
            r += 1
        }
        // Even length
        l = i
        r = i + 1
        while l >= 0 && r < n && sArray[l] == sArray[r] {
            if (r - l + 1) > resLen {
                resIdx = l
                resLen = r - l + 1
            }
            l -= 1
            r += 1
        }
    }

    return String(sArray[resIdx ..< resIdx + resLen])
}
```  

#### Explanation  
Finally, we can solve this problem in a space-optimized way using the two-pointer technique.  

Let's visualize how we can check if a substring is a palindrome using the example `"bab"`:  

![alt image](images/p-3-2.png#center)  

- We can check if it's a palindrome by starting from the outside and comparing the characters. As long as they are equal, we continue until we reach the middle, confirming it is a palindrome.  

Another way:  

![alt image](images/p-3-3.png#center)  

- We can start in the middle, expand outward, and check if it’s a palindrome that way.  

You can see that the second method is more efficient, so we will use this approach.  

![alt image](images/p-3-4.png#center)  

For each character, we consider it as the center and expand outward. Since we take each character (`n`) and expand outward (`n`), the overall time complexity is `O(n²)`.  

Finally, we need to handle one edge case: checking for palindromes of even length.  

#### Time/Space Complexity  
* **Time complexity**: O(n²)  
* **Space complexity**: O(1)  

#### Thank you for reading! 😊
