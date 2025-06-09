+++
title = "LeetCode - 150 - Generate Parentheses"
date = 2025-06-09T07:54:24+03:00
tags = ["LeetCode", "150", "Generate Parentheses", "Swift"]
draft = false
+++

### The problem

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

#### Examples

```
Input: n = 3  
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

```
Input: n = 1  
Output: ["()"]
```

#### Constraints

* 1 <= n <= 8

#### Explanation

Before we jump to the solution, let's figure out what **well-formed parentheses** mean.
In this problem, this means when you're writing the code using nested parentheses, you want them to be nested in a valid way, like `(()())`.

> We can't have a right `)` parenthesis come before a left `(` parenthesis.
> In the example section, you can see that for each left parenthesis we have a matching right parenthesis that comes after it at some point.
> The `n = 3` means that we have **three pairs**. In total, we have **six** parentheses (**three** are going to be open `(`, and **three** are going to be closed `)`).

### Backtracking Solution

```swift
func generateParenthesis(_ n: Int) -> [String] {
    var stack: [String] = []
    var res: [String] = []

    func backtrack(_ openN: Int, _ closedN: Int) {
        if openN == closedN && closedN == n {
            res.append(stack.joined(separator: ""))
            return
        }

        if openN < n {
            stack.append("(")
            backtrack(openN + 1, closedN)
            stack.removeLast()
        }

        if closedN < openN {
            stack.append(")")
            backtrack(openN, closedN + 1)
            stack.removeLast()
        }
    }

    backtrack(0, 0)

    return res
}
```

#### Explanation

Let's say we start with an empty array:

* We know that we can't start with a closing `)` parenthesis; we can only start with an open `(` parenthesis.
  ![alt image](images/22.png#center)
* We can have another open `(` parenthesis because our limit is `3` open parentheses, and so far we have **two**.
  ![alt image](images/22-1.png#center)
* What if we wanted to add a closing `)` parenthesis instead of an open `(`? We can do it because at this point we have only **one** open parenthesis, and **zero** closed, but when we add a closed parenthesis, the close count also changes to **one**.
  ![alt image](images/22-2.png#center)

You can see that we keep count of our open and closed parentheses, which helps us add an open parenthesis only when the condition `close < open` is satisfied.
When we add a parenthesis, we update our counters.

We can solve this problem by using a backtracking algorithm.

* We will be keeping track of how many open and closed parentheses are left
* And check if the condition `close < open` is satisfied
  ![alt image](images/22-3.png#center)

In the picture, you can see how the algorithm will generate the result, with three of each parenthesis and valid ordering.

#### Time/ Space complexity

* Time complexity: O(4^n/sqrt(n))
* Space complexity: O(n)

#### Thank you for reading! 😊
