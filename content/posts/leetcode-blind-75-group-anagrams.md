+++
title = 'LeetCode - Blind 75 - Group Anagrams'
date = 2024-11-01T07:20:55+03:00
tags = ["LeetCode", "Blind 75", "Group Anagrams", "Swift"]
draft = false
+++

### The problem 
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
> An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, using all the original letters exactly once.

#### Examples
``` 
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
Explanation:
* There is no string in strs that can be rearranged to form "bat."
* The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
* The strings "ate," "eat," and "tea" are anagrams as they can be rearranged to form each other.
```

```
Input: strs = [""]
Output: [[""]]
```

```
Input: strs = ["a"]
Output: [["a"]]
```

#### Constraints
* 1 <= strs.length <= 1000
* 0 <= strs[i].length <= 100
* strs[i] is made up of lowercase English letters.

### Brute force solution 
```swift 
func groupAnagrams(_ strs: [String]) -> [[String]] {
    var res: [String: [String]] = [:]
    for s in strs {
        let sortedS = String(s.sorted())
        res[sortedS, default: []].append(s)
    }
    return Array(res.values)
}
```

#### Explanation
The brute force solution:
- Uses additional space `res` to store the result
- Iterates through all input strings in `strs`
- Sorts each string
- Appends the current element to the `res` dictionary by the `sortedS` key
- Returns the result

##### Time/Space Complexity
* Time complexity: O(m * n log n), where `m` is the length of input `strs` and `n log n` is the time complexity of the sorting algorithm, where `n` is the length of the longest string.
* Space complexity: O(m ∗ n), as it uses additional space to store the sorted string.

### Solution 2
```swift 
func groupAnagrams(_ strs: [String]) -> [[String]] {
    var res: [[Int]: [String]] = [:]
    let aAsciiValue = Character("a").asciiValue!

    for s in strs {
        var count = Array(repeating: 0, count: 26)

        for c in s {
            count[Int(c.asciiValue! - aAsciiValue)] += 1
        }

        if res[count] == nil {
            res[count] = [s]
        } else {
            res[count]!.append(s)
        }
    }

    return Array(res.values)
}
``` 

#### Explanation
Solution 2:
- Iterates through each string in `strs`
- Iterates through every character in string `s` and counts its occurrences
- Updates the result property `res`
- Returns the result value 

##### Time/Space Complexity
* Time complexity: O(m * n), where `m` is the length of input `strs` and `n` is the length of a single string.
* Space complexity: O(m), where `m` is the number of strings.

#### Thank you for reading! 😊
