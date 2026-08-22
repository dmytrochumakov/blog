+++
title = "LeetCode - Longest Common Prefix"
date = 2026-08-22T19:10:53+03:00
tags = ["LeetCode", "Longest Common Prefix", "Arrays_&_Hashing", "Easy", "Swift"]
draft = false
+++

### The problem

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: strs = ["flower","flow","flight"]
Output: "fl"

```

**Example 2:**

```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

```

**Constraints:**

* `1 <= strs.length <= 200`
* `0 <= strs[i].length <= 200`
* `strs[i]` consists of only lowercase English letters if it is non-empty.

#### Explanation

> I'm trying to add a little bit of life to my posts, so I decided to lay down some thought process behind the solution. Please go easy on me.

My first attempt to solve this problem was to iterate over the first word's characters and inside that for loop iterate over the rest of the words and find the prefix. It did work out. 😊

I started over and I learned that I can just use a pointer to check if characters are equal.

Next, I used to have a variable `result` that was storing the prefix. It was just a string. When I ran the tests, I kept seeing "flow" in the output. After a few attempts, I realised that it was behaving correctly because "flower" and "flow" have the same prefix. I just needed to find the smallest one. So I decided to collect all prefixes by adding them into an array and find the minimum one. And voila, it worked.

The time complexity of this algorithm is O(n*m), where *n* and *m* are the length of the words that I was comparing with each other. And I also used additional O(n) space for the collection of prefixes.

It passed on LeetCode, but I thought that it had room for improvements.

#### Initial Solution

```swift
func longestCommonPrefix(_ strs: [String]) -> String {
   let firstStr = strs[0]
   if firstStr.isEmpty {
       return ""
   }
   
   if strs.count == 1 {
       return strs[0]
   }
   
   let firstStrArr = Array(firstStr)
   var res: [String] = []
   
   for str in strs.dropFirst() {
       var i = 0
       let strArr = Array(str)
       
       while i < str.count && i < firstStrArr.count {
           if firstStrArr[i] == strArr[i] {
               i += 1
           } else {
               break
           }
       }
       
       res.append( String(strArr[0 ..< i]))
   }
   
   return res.min() ?? ""
}
```

### Space Optimized  Solution

#### Explanation

The additional memory and finding the minimum prefix seemed overcomplicated. So I decided to look for ways to optimise it.

I found one. Instead of starting by iterating through the input strings, I could just iterate over the characters from the first string and compare them with other strings. If they are not equal, I would return the prefix that we have seen so far.

This solution only optimises space complexity. Now it is constant.

#### Code

```swift
func longestCommonPrefix(_ strs: [String]) -> String {
    var res = ""
    
    for i in strs[0].indices {
        
        for str in strs {
            if i == str.endIndex || strs[0][i] != str[i] {
                return res
            }
        }
        
        res.append(strs[0][i])
    }
    
    return res
}
```

#### Time/ Space complexity

* Time complexity: O(n*m)
* Space complexity: O(1)

- Where `n` is the length of the first string, and `m` is the number of `strs`.

#### Thank you for reading! 😊
