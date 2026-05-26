+++
title = "LeetCode - 150 - Valid Parenthesis String"
date = 2026-05-26T08:05:31+03:00
tags = ["LeetCode", "150", "Valid Parenthesis String", "Greedy", "Swift"]
draft = false
+++

### The problem

Given a string `s` containing only three types of characters: `'('`, `')'`, and `'*'`, return `true` *if* `s` *is **valid***.

The following rules define a **valid** string:

* Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
* Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
* A left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
* `'*'` could be treated as a single right parenthesis `')'`, a single left parenthesis `'('`, or an empty string `""`.

**Example 1:**

```
Input: s = "()"
Output: true
```

**Example 2:**

```
Input: s = "(*)"
Output: true
```

**Example 3:**

```
Input: s = "(*))"
Output: true
```

**Constraints:**

* `1 <= s.length <= 100`
* `s[i]` is `'('`, `')'`, or `'*'`.

#### Explanation

To better understand the problem, let's visualize the second example.

We can solve this problem in a brute force way by building a decision tree for a wildcard.

The decision tree is going to have **three** options that we can choose from: `(`, `)`, and `""`.

![alt image](images/678.png#center)

The brute force solution is not very efficient since it will take `O(3^n)` time, and even if we added memoization, it would take `O(n^3)` time.

### Greedy Solution

#### Explanation

To solve this problem in linear time, we are going to use a greedy approach. We can do this by creating an `open` variable and tracking the number of open parentheses. If we find a closed parenthesis, we decrement the `open` count by one.

The problem with one variable is that when we encounter a **wildcard** character, it creates an additional **three** possibilities, and we can't store all possibilities in one variable. For example:

![alt image](images/678-1.png#center)

* If we were at the **wildcard** character and we decided to choose an open parenthesis, we would have **two** open parentheses.
* If we chose a closed parenthesis, we would have **zero** open parentheses.
* Lastly, if we picked an empty string, the amount of open parentheses would stay unchanged.

This observation helps us recognize that we need to track two properties instead of one. We are going to create `openMin` and `openMax` properties that will represent a range of possibilities.

Let's look at a slightly different example to see how it works:

* If we were at the first **wildcard** character and we decided to include an open parenthesis, `openMin` would be `0` and `openMax` would be `1`.

![alt image](images/678-2.png#center)

* If we were at the second **wildcard** character and we decided to include an open parenthesis, `openMin` would be `0` and `openMax` would be `2`.

![alt image](images/678-3.png#center)

But, if we chose a closed parenthesis, we would need to decrement the `openMin` property, which in this case would become `-1`. The negative `openMin` suggests that one of the choices we made was invalid, and we need to reset `openMin` to `0`.

![alt image](images/678-4.png#center)

Lastly, we need to take care of the corner case where `openMax` could become negative.

* If we had invalid input that starts with a closed parenthesis, we would decrement `openMax` and return `false` because we have no choices left to make it valid.

![alt image](images/678-5.png#center)

In the end, we are going to check if `openMin == 0` and return the result.

#### Code

```swift
func checkValidString(_ s: String) -> Bool {
    var openMin = 0
    var openMax = 0

    for c in s {
        if c == "(" {
            openMin += 1
            openMax += 1
        } else if c == ")" {
            openMin -= 1
            openMax -= 1
        } else {
            openMin -= 1
            openMax += 1
        }

        if openMax < 0 {
            return false
        }

        if openMin < 0 {
            openMin = 0
        }
    }

    return openMin == 0
}
```

#### Time/Space Complexity

* Time complexity: `O(n)`
* Space complexity: `O(1)`

#### Thank you for reading! 😊
