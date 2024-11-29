+++
title = 'LeetCode - Blind 75 - Longest Repeating Character Replacement'
date = 2024-11-29T07:27:59+03:00
tags = ["LeetCode", "Blind 75", "Longest Repeating Character Replacement", "Swift"]
draft = false
+++

### The problem  
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.  
Return the length of the longest substring containing the same letter you can get after performing the above operations.  

#### Examples  
``` 
Input: s = "ABAB", k = 2  
Output: 4  
Explanation: Replace the two 'A's with two 'B's or vice versa.  
```  

```
Input: s = "AABABBA", k = 1  
Output: 4  
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".  
The substring "BBBB" has the longest repeating letters, which is 4.  
There may exist other ways to achieve this answer too.  
```  

#### Constraints  
* 1 <= s.length <= 105  
* `s` consists of only uppercase English letters.  
* 0 <= k <= s.length  

### Brute force solution  
```swift  
func characterReplacement(_ s: String, _ k: Int) -> Int {  
    let n = s.count  
    let s = Array(s)  
    var res = 0  

    for i in 0 ..< n {  
        var count: [Character: Int] = [:]  
        var maxF = 0  

        for j in i ..< n {  
            count[s[j], default: 0] += 1  
            maxF = max(maxF, count[s[j]]!)  

            let windowSize = (j - i + 1)  
            let numOfCharsToReplace = (windowSize - maxF)  
            let canReplace = (numOfCharsToReplace <= k)  

            if canReplace {  
                res = max(res, windowSize)  
            }  
        }  
    }  

    return res  
}  
```  

#### Explanation  
First things first, we need to figure out which character we are going to replace. We can do this by using a dictionary. This way, we can calculate the most frequent character in the substring. After that, we will be able to calculate the number of characters to replace and check if we can replace the character.  
This algorithm is not very fast and takes O(n^2) time. We can improve it further and optimize it to achieve O(n) time complexity.

##### Time/Space Complexity  
* Time complexity: O(n^2)  
* Space complexity: O(m), where `m` is the total number of unique characters in the string.  

### Solution 2 - Optimal  
```swift  
func characterReplacement(_ s: String, _ k: Int) -> Int {  
    let n = s.count  
    let s = Array(s)  
    var res = 0  
    var count: [Character: Int] = [:]  
    var l = 0  
    var maxF = 0  

    for r in 0 ..< n {  
        count[s[r], default: 0] += 1  
        maxF = max(maxF, count[s[r]]!)  

        let numOfCharsToReplace = (windowSize(r: r, l: l) - maxF)  
        let cantReplace = (numOfCharsToReplace > k)  

        if cantReplace {  
            count[s[l]]! -= 1  
            l += 1  
        }  

        res = max(res, windowSize(r: r, l: l))  
    }  

    return res  
}  

func windowSize(r: Int, l: Int) -> Int {  
    return r - l + 1  
}  
```  

#### Explanation  
The optimal solution is very similar to the brute force solution, except we got rid of the second loop. The logic stays the same. What we did is store properties from the inner loop outside. In case `numOfCharsToReplace` is more than `k`, it means we exceeded the allowed number of replacements. We then need to shrink our window by decrementing the count of the current character and moving the `l` pointer.  

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(m), where `m` is the total number of unique characters in the string.  

#### Thank you for reading! 😊
