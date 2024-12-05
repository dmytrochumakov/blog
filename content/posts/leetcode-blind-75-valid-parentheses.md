+++
title = 'LeetCode - Blind 75 - Valid Parentheses'
date = 2024-12-05T07:25:46+03:00
tags = ["LeetCode", "Blind 75", "Valid Parentheses", "Swift"]
draft = false
+++

### The Problem  
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, and `']'`, determine if the input string is valid.  
An input string is valid if:  
1. Open brackets must be closed by the same type of brackets.  
2. Open brackets must be closed in the correct order.  
3. Every closing bracket has a corresponding open bracket of the same type.  

#### Examples  
```  
Input: s = "()"  
Output: true  
```  

```  
Input: s = "()[]{}"  
Output: true  
```  

```  
Input: s = "(]"  
Output: false  
```  

```  
Input: s = "([])"  
Output: true  
```  

#### Constraints  
* 1 <= s.length <= 10⁴  
* `s` consists of parentheses only `'()[]{}'`.  

### Brute Force Solution  
```swift  
func isValid(_ s: String) -> Bool {  
    var s = s  
    while s.contains("()") || s.contains("{}") || s.contains("[]") {  
        s = s.replacingOccurrences(of: "()", with: "")  
        s = s.replacingOccurrences(of: "{}", with: "")  
        s = s.replacingOccurrences(of: "[]", with: "")  
    }  
    return s.isEmpty  
}  
```  

#### Explanation  
The brute force way to solve this problem is to check if the string contains `"()"`, `"{}"`, `"[]"` and replace them with an empty string. It's not a very efficient algorithm and takes O(n²) time because of the `while` loop and the replacing operation on the entire string. We can optimize it by using a stack data structure.  

##### Time/Space Complexity  
* Time complexity: O(n²)  
* Space complexity: O(n)  

---

### Solution - 2: Stack  
```swift  
func isValid(_ s: String) -> Bool {  
    var stack: [Character] = []  
    let closeToOpen: [Character: Character] = [ ")" : "(", "]" : "[", "}" : "{" ]  
    for c in s {  
        if closeToOpen[c] != nil {  
            if !stack.isEmpty && stack.last! == closeToOpen[c] {  
                stack.removeLast()  
            } else {  
                return false  
            }  
        } else {  
            stack.append(c)  
        }  
    }  
    return stack.isEmpty  
}  
```  

#### Explanation  
By using a stack, we can compare the last parentheses with the current parentheses in the loop. For example `(` and `)`. If they match, remove them; if they don’t match, return `false`.  
To avoid repetitive work and writing code for every open and close parenthesis, we can use a dictionary.  

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

#### Thank you for reading! 😊
