+++
title = 'LeetCode - Blind 75 - Valid Palindrome'
date = 2024-11-14T07:00:45+03:00
tags = ["LeetCode", "Blind 75", "Valid Palindrome", "Swift"]
draft = false
+++

### The Problem
Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

> A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

#### Examples
``` 
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

#### Constraints
* 1 <= s.length <= 2 * 10^5
* s consists only of printable ASCII characters.

### Brute Force Solution
```swift
func isPalindrome(_ s: String) -> Bool {
    var newStr = ""

    for c in s {
        if c.isLetter || c.isNumber {
            newStr += c.lowercased()
        }
    }

    return newStr == String(newStr.reversed())
}
```

#### Explanation
Let's start with the brute force solution. According to the description, we need to check if the string contains only lowercase, alphanumeric characters and reads the same forward and backward. In Swift, you can check this using the built-in `isLetter` and `isNumber` properties. Based on this information, we iterate through all characters in the `s` string, verify that each character is a letter or number, update `newStr` with the current `lowercased` character, and compare the result with its `reversed` version.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

### Solution - 2 - Optimal
```swift
func isPalindrome(_ s: String) -> Bool {
    let s = Array(s)
    var l = 0
    var r = s.count - 1

    while l < r {
        if !isAlnum(s[l]) {
            l += 1
            continue
        }
        if !isAlnum(s[r]) {
            r -= 1
            continue
        }
        if s[l].lowercased() != s[r].lowercased() {
            return false
        }
        l += 1
        r -= 1
    }

    return true
}

func isAlnum(_ c: Character) -> Bool {
    let cAscii = c.asciiValue!
    return (
        cAscii >= Character("A").asciiValue! && cAscii <= Character("Z").asciiValue!
        ||
        cAscii >= Character("a").asciiValue! && cAscii <= Character("z").asciiValue!
        ||
        cAscii >= Character("0").asciiValue! && cAscii <= Character("9").asciiValue!
    )
}
``` 

#### Explanation
To solve the problem optimally, based on the description, we need to determine if each character is alphanumeric and lowercase. We can accomplish this by introducing an `isAlnum` helper function that checks if a character is alphanumeric within the loop, using the two-pointer technique. For example, in the array `"car rac"`, the first left and right characters are both `"c"`, the second are both `"a"`, and the third are both `"r"`. Following this logic, we can confirm that the input string `s` is a palindrome.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
