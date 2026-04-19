+++
title = "LeetCode - 150 - Regular Expression Matching"
date = 2026-04-19T09:15:38+03:00
tags = ["LeetCode", "150", "Regular Expression Matching", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'`, where:

* `'.'` matches any single character.
* `'*'` matches zero or more of the preceding element.

Return a boolean indicating whether the matching covers the entire input string (not partial).

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Constraints:**

* `1 <= s.length <= 20`
* `1 <= p.length <= 20`
* `s` contains only lowercase English letters.
* `p` contains only lowercase English letters, `'.'`, and `'*'`.
* It is guaranteed that for each appearance of the character `'*'`, there will be a previous valid character to match.

#### Explanation

Let's begin with a visualization of a few base cases:

* If we had an input `s = "aa"` and `p = "a"`, we would return `false` because the strings have different lengths.

For the case where the expression has a **dot**:

* If we had `s = "aa"` and `p = "a.."`, we would return `false` as the strings have different lengths.
* If we had `s = "aa"` and `p = "a."`, we would return `true` since the **dot** can match any character.
  ![alt image](images/10.png#center)

In the case where we have an expression with a **star**, where `s = "aa"` and `p = "a*"`,
the expression `"a*"` means that the element that comes before can be repeated an **infinite** number of times or **zero** times.
![alt image](images/10-1.png#center)

For the case where we only have symbols, where `s = "ab"` and `p = ".*"`, we would be able to generate an empty string or any number of **dots**, where one of the **dots** could match `s`.
![alt image](images/10-2.png#center)

### Brute Force Solution

#### Explanation

One way to solve this problem is to brute-force it by making a decision tree.
For example, with input `s = "aa"` and `p = "a*"`, we have a choice to use the **star** and repeat `"a"` or skip it.
![alt image](images/10-3.png#center)

You can see that by repeating the character `"a"` twice, we find a match and can return `true`.

This is not an efficient solution, as it takes `O(2^(m+n))` time.

### Dynamic Programming (Top-Down) Solution

#### Explanation

Let's look at a slightly different example where we have different input sizes and a more complex pattern. For example, `s = "aab"` and `p = "c*a*b"`:

We have a choice to use a **star** or not. For example:

* If we use the first **star**, we would get the `c` character or an **empty** string.
  ![alt image](images/10-4.png#center)

We are not going to continue the `c` path because it does not match `a`. Instead, we skip it and update the `j` pointer by **two**, while the `i` pointer stays the same.
![alt image](images/10-5.png#center)

Next, if we use the **star**, we would get `a` or an **empty** string.
If we follow the `a` path, we would find a match and increment the `i` pointer.
![alt image](images/10-6.png#center)

Since the `j` pointer is still at the `a` character and we have a **star** after it, we can repeat `a` or skip it.
If we decide to repeat `a`, we would get the string `aa` that matches two characters in `s`. After that, we shift the `i` pointer by one.
![alt image](images/10-7.png#center)

Next, we have two matching characters, and we have a choice to repeat `a` one more time or skip it.
If we try to repeat `a`, we would get `aaa`, which does not match the string `s`.
If we skip it, we would get `aa` and increment the `j` pointer by two.
![alt image](images/10-8.png#center)

Lastly, we don't have any symbols left in the string `p`, so now we only have the option to compare characters at positions `i` and `j`.
![alt image](images/10-9.png#center)

You can see that the characters match, and we find the result, so we increment both pointers `i` and `j`.
The base case for finding a match will look like this: `i >= len(s) && j >= len(p)`.

> One quick note: we are going to check one more edge case. If `j` is greater than `len(p)` and we still have characters in `s`, we return `false` because there are still characters that need a match. For example:
> ![alt image](images/10-10.png#center)

```swift
func isMatch(_ s: String, _ p: String) -> Bool {
    let sArr = Array(s)
    let pArr = Array(p)
    var cache: [[Int]: Bool] = [:]

    func dfs(_ i: Int, _ j: Int) -> Bool {
        if let cached = cache[[i, j]] {
            return cached
        }

        if i >= s.count && j >= p.count {
            return true
        }

        if j >= p.count {
            return false
        }

        let match = i < s.count && (sArr[i] == pArr[j] || pArr[j] == ".")

        if (j + 1) < p.count && pArr[j + 1] == "*" {
            cache[[i, j]] = dfs(i, j + 2) || // don't use "*"
                            (match && dfs(i + 1, j)) // use "*"
            return cache[[i, j]]!
        }

        if match {
            cache[[i, j]] = dfs(i + 1, j + 1)
            return cache[[i, j]]!
        }

        return false
    }

    return dfs(0, 0)
}
```

#### Time/ Space complexity

* Time complexity: `O(m * n)`
* Space complexity: `O(m * n)`
* Where `m` is the length of string `s` and `n` is the length of string `p`.

#### Thank you for reading! 😊
