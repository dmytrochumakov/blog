+++
title = 'LeetCode - Blind 75 - Valid Anagram'
date = 2024-10-30T07:26:31+03:00
tags = ["LeetCode", "Blind 75", "Valid Anagram", "Swift"]
draft = false
+++

### The Problem
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.
> An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once.

#### Examples
``` 
Input: s = "anagram", t = "nagaram"
Output: true
```

```
Input: s = "rat", t = "car"
Output: false 
```

Follow-up: What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

### Brute Force Solution 
``` swift 
func isAnagram(s: String, t: String) -> Bool {
    if s.count != t.count {
        return false
    }
    return s.sorted() == t.sorted()
}
```

#### Explanation
The brute force solution is to check the lengths of `s` and `t`, and if they are not equal, `return false`. Otherwise, compare the sorted `s` and `t` strings and return the result.

##### Time/Space Complexity
- Time complexity: Depends on the sorting algorithm, on average O(N log N).
- Space complexity: O(1) or O(N) depending on the sorting algorithm.

### Solution - 2
``` swift 
func isAnagram(s: String, t: String) -> Bool {
    if s.count != t.count {
        return false
    }

    var sDict: [Character: Int] = [:]
    var tDict: [Character: Int] = [:]

    for i in 0 ..< s.count {
        sDict[s[i], default: 0] += 1
        tDict[t[i], default: 0] += 1
    }

    return sDict == tDict
}

extension StringProtocol {
    subscript(offset: Int) -> Character { self[index(startIndex, offsetBy: offset)] }
}
``` 
#### Explanation
Solution 2 has a base case that checks the lengths of `s` and `t`, and if they are not equal, it `returns` `false`. Otherwise, it iterates through all characters in `s` and `t`, storing the count of each character's occurrence. In the final step, it `returns` the result of comparing the two dictionaries.

##### Time/Space Complexity
- Time complexity: O(N) - iterates through the entire string.
- Space complexity: O(N) - uses additional space to store characters and their counts.

### Solution - 3
``` swift 
func isAnagram(s: String, t: String) -> Bool {
    if s.count != t.count {
        return false
    }

    var count: [Int] = Array(repeating: 0, count: 26)
    let aAsciiValue = Character("a").asciiValue!

    for char in s.utf8 {
        count[Int(char - aAsciiValue)] += 1
    }

    for char in t.utf8 {
        count[Int(char - aAsciiValue)] -= 1
    }

    for val in count {
        if val != 0 {
            return false
        }
    }

    return true
}
``` 
#### Explanation
Solution 3 has a base case that checks the lengths of `s` and `t`, and if they are not equal, it `return`s `false`. 
- After that, it iterates through the array of encoded `utf8` `s` elements, calculates the character index, and increments the count of that specific character in `s`. 
- In the next step, it iterates through the array of encoded `utf8` `t` elements, calculates the character index, and decrements the count of that specific character. 
- The last step is to iterate through the `count` array and check if any `val` is not equal to `0`; if so, it `return`s `false` (indicating an extra character was found, and it is no longer an anagram).

##### Time/Space Complexity
- Time complexity: O(N)  
- Space complexity: O(1)

#### Thank you for reading! 😊
