+++
title = "LeetCode - 150 - Palindrome Partitioning"
date = 2025-09-12T07:46:17+03:00
tags = ["LeetCode", "150", "Palindrome Partitioning", "Swift"]
draft = false
+++

### The problem

Given a string `s`, partition `s` such that every substring of the partition is a palindrome. Return all possible palindrome partitionings of `s`.

> A substring is a contiguous non-empty sequence of characters within a string.

> A palindrome is a string that reads the same forward and backward.

#### Examples

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

```
Input: s = "a"
Output: [["a"]]
```

#### Constraints

* 1 <= s.length <= 16
* s contains only lowercase English letters.

#### Explanation

From the description of the problem we learn that we are given string `s` that we need to partition in a way that all substrings are palindromes.

One of the rules that will help us create palindromes is that every single character is a palindrome, because when you reverse it, it stays the same. For example, for input `s = "aab"` we can create `three` different palindromes `["a"], ["a"], ["b"]`.
![alt image](images/131.png#center)

You can see that partitioning of the input `s` gives us the opportunity to find the first array with palindromes and add it to our result.

Another way to find palindromes is to partition input `s` by only using characters `"aa"`, and a single `"b"`.
![alt image](images/131-1.png#center)

We can’t add anything else to our result because input `s` with `"ab" -> "ba"` is not a palindrome, and input `s` with `"aab" -> "baa"` is also not a palindrome.

### Backtracking Solution

```swift
func partition(_ s: String) -> [[String]] {
    var res: [[String]] = []
    var part: [String] = []
    let n = s.count

    func dfs(_ i: Int) {
        if i >= n {
            res.append(part)
            return
        }
        for j in i ..< n {
            if isPalindrome(s, i, j) {
                part.append(String(s[i ..< j+1]))
                dfs(j + 1)
                part.removeLast()
            }
        }
    }

    dfs(0)

    return res
}

func isPalindrome(_ s: String, _ l: Int, _ r: Int) -> Bool {
    var l = l
    var r = r
    while l < r {
        if s[l] != s[r] {
            return false
        }
        l += 1
        r -= 1
    }
    return true
}

extension StringProtocol {
    subscript(offset: Int) -> Character { self[index(startIndex, offsetBy: offset)] }
    subscript(range: Range<Int>) -> SubSequence {
        let startIndex = index(self.startIndex, offsetBy: range.lowerBound)
        return self[startIndex..<index(startIndex, offsetBy: range.count)]
    }
}
```

#### Explanation

We can solve this problem by using a backtracking algorithm. To do that we need to create every single combination for our input that we can partition, and if those partitions can form palindromes we will add them to our result.

For example, for input `s` with characters `"aab"`
![alt image](images/131-2.png#center)
For the first partition we have `three` choices:

* We can add a single `a` by taking only the first character
* Next, we can add two `a`'s by taking the first two characters
* Lastly, we can add our entire string `s`. We also will be checking if our partition is a palindrome and you can see that `"aab"` is not a palindrome so we are not going to continue on this path.

For the second partition we also have `three` choices:

* We can try to partition string `"ab"`, and add another single `a`
* Another partition will be when we take both characters `ab`, but we can’t do this because it’s not a palindrome, so we will not be going that path
* At this point, for the choice with `aa`, we are left with only one `b` character, and we can’t add more at this path, so this means that we found one partition where all substrings are palindromes

Lastly, we are left with only one choice: to add `b` to the path with single `a`s. That also will be a palindrome.

#### Time/ Space complexity

* Time complexity: O(n\*2^n)
* Space complexity: O(n) extra space, O(n\*2^n) for the output list

#### Thank you for reading! 😊
