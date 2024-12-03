+++
title = 'LeetCode - Blind 75 - Minimum Window Substring'
date = 2024-12-03T07:09:24+03:00
tags = ["LeetCode", "Blind 75", "Minimum Window Substring", "Swift"]
draft = false
+++

### The Problem
Given two strings `s` and `t` of lengths `m` and `n`, respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

> A substring is a contiguous, non-empty sequence of characters within a string.

The test cases will be generated such that the answer is unique.

#### Examples
``` 
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return an empty string.
```

#### Constraints
* m == s.length
* n == t.length
* 1 <= m, n <= 10⁵
* `s` and `t` consist of uppercase and lowercase English letters.

**Follow-up:** Could you find an algorithm that runs in `O(m + n)` time?

---

### Brute Force Solution
```swift
func minWindow(_ s: String, _ t: String) -> String {
    if t.isEmpty {
        return ""
    }

    var countT: [Character: Int] = [:]
    for c in t {
        countT[c, default: 0] += 1
    }

    var res = (-1, -1)
    var resLen = Int.max
    let sArray = Array(s)
    let lenS = sArray.count

    for i in 0 ..< lenS {
        var countS: [Character: Int] = [:]

        for j in i ..< lenS {
            countS[sArray[j], default: 0] += 1

            var flag = true
            for (key, value) in countT {
                if countS[key, default: 0] < value {
                    flag = false
                    break
                }
            }

            let windowSize = (j - i + 1)
            if flag && windowSize < resLen {
                resLen = windowSize
                res = (i, j)
            }
        }
    }

    let (l, r) = res
    if resLen == Int.max {
        return ""
    } else {
        return String(sArray[l...r])
    }
}
```

#### Explanation
When you look at the solution, you can see that it is a very complicated problem. Let's divide it into parts to help us understand it more deeply.  
Before we find the result, we need to calculate the count for each character in the `t` string. This will help us find a similar substring in `s`.  
Now, when we have our count of `t`, we can compare it with the count of `s` and update our result. This is not a very efficient solution, and we can minimize repetitive work—“iterating through the input every iteration”—by getting rid of the second loop and applying the sliding window technique.

##### Time/Space Complexity
* Time complexity: O(n²)  
* Space complexity: O(m), where `m` is the total number of unique characters in strings `s` and `t`.

---

### Solution 2 - Optimal
```swift
func minWindow(_ s: String, _ t: String) -> String {
    if t.isEmpty {
        return ""
    }

    var countT: [Character: Int] = [:]
    for c in t {
        countT[c, default: 0] += 1
    }

    var window: [Character: Int] = [:]
    var res = (-1, -1)
    var resLen = Int.max
    let sArray = Array(s)
    let lenS = sArray.count
    var have = 0
    let need = countT.count
    var l = 0

    for r in 0 ..< lenS {
        let c = sArray[r]
        window[c, default: 0] += 1

        if countT[c] != nil && window[c] == countT[c] {
            have += 1
        }

        while have == need {
            let windowSize = (r - l + 1)
            if windowSize < resLen {
                res = (l, r)
                resLen = windowSize
            }

            window[sArray[l], default: 0] -= 1
            if countT[sArray[l]] != nil && window[sArray[l], default: 0] < countT[sArray[l], default: 0] {
                have -= 1
            }
            l += 1
        }
    }

    l = res.0
    let r = res.1

    if resLen == Int.max {
        return ""
    } else {
        return String(sArray[l...r])
    }
}
```

#### Explanation
To solve this problem in an optimal way, we need to add two additional properties: `have` and `need`. These will help us determine when to move our pointers.  
The `need` variable is simply the count of all characters inside the `countT` dictionary.  
`have` is the number of characters in the current window that satisfy the condition.  
We update the `have` property only when the value of `window` is equal to the value of `countT`. For example, a substring of `s` with "ADOB" and a substring of `t` with "ABC" will have:  
`window` with `A: 1, B: 1`, and `countT` with `A: 1, B: 1, C: 1`.  
When the condition `have == need` is satisfied, we update the result and move the pointers.

##### Time/Space Complexity
* Time complexity: O(n)  
* Space complexity: O(m), where `m` is the total number of unique characters in strings `s` and `t`.

#### Thank you for reading! 😊
