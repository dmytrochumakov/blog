+++
title = 'LeetCode - Blind 75 - Longest Substring Without Repeating Characters'
date = 2024-11-26T07:12:59+03:00
tags = ["LeetCode", "Blind 75", "Longest Substring Without Repeating Characters", "Swift"]
draft = false
+++

#### The Problem  
Given a string `s`, find the length of the longest substring without repeating characters.  
> A substring is a contiguous non-empty sequence of characters within a string.

#### Examples  
``` 
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.  
Notice that the answer must be a substring. "pwke" is a subsequence and not a substring.
```

#### Constraints  
* 0 <= s.length <= 5 * 10⁴  
* `s` consists of English letters, digits, symbols, and spaces.

---

### Brute Force Solution  

```swift
func lengthOfLongestSubstring(_ s: String) -> Int {
    let n = s.count
    var res = 0

    for i in 0 ..< n {
        var chars: [Character] = []
        for j in i ..< n {
            if chars.contains(s[j]) {
                break
            }
            chars.append(s[j])
        }
        res = max(res, chars.count)
    }

    return res
}
```

#### Explanation  
Let’s start by visualizing the problem:  
![alt image](images/problem_3.png#center)  

We can compare every substring starting from the first element, iterating through all elements, and when we find a duplicate character, we update the result with the length of the substring.  
This is not an efficient solution and will take O(n * m) time. We can optimize it with the sliding window technique.

##### Time/Space Complexity  
* **Time complexity**: O(n * m) - where `n` is the length of the string and `m` is the number of unique characters in the substring.  
* **Space complexity**: O(m)  

---

### Solution 2 - Sliding Window  

```swift
func lengthOfLongestSubstring(_ s: String) -> Int {
    let n = s.count
    let s = Array(s)
    var res = 0
    var charSet: Set<Character> = []
    var l = 0

    for r in 0 ..< n {
        while charSet.contains(s[r]) {
            charSet.remove(s[l])
            l += 1
        }
        charSet.insert(s[r])
        let windowSize = (r - l + 1)
        res = max(res, windowSize)
    }

    return res
}
```

#### Explanation  
As mentioned above, we can optimize our brute force solution by using the sliding window technique.  
We control the window using a `set` data structure to eliminate duplicates.  
When we find a duplicate value during iteration, we remove that value from the set and move the left pointer to form a new window.  
![alt image](images/problem_3_1.png#center)  

By iterating through all the input, we can find the length of the longest substring.

##### Time/Space Complexity  
* **Time complexity**: O(n)  
* **Space complexity**: O(m) - where `m` is the number of unique characters in the substring.  

#### Thank you for reading! 😊
