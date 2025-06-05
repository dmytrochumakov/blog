+++
title = "LeetCode - 150 - Evaluate Reverse Polish Notation"
date = 2025-06-05T07:51:16+03:00
tags = ["LeetCode", "150", "Evaluate Reverse Polish Notation", "Swift"]
draft = false
+++

### The problem

You are given an array of strings `tokens` that represents an arithmetic expression in [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

* The valid operators are `+`, `-`, `*` and `/`.
* Each operand may be an integer or another expression.
* The division between two integers always truncates toward zero.
* There will not be any division by zero.
* The input represents a valid arithmetic expression in reverse Polish notation.
* The answer and all the intermediate calculations can be represented in a 32-bit integer.

#### Examples

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

#### Constraints

* 1 <= tokens.length <= 10^4
* tokens\[i] is either an operator: `+`, `-`, `*`, or `/`, or an integer in the range \[-200, 200].

#### Explanation

Let’s take a look at the example with input `tokens = ["2","1","+","3","*"]`, and try to figure out the way we can solve it.
![alt image](images/150.png#center)

* We will be reading inputs from left to right.
* Any operator will be applied to the previous two values (**we are guaranteed that there are going to be two values**).
* Next, we replace the previous two values with the new calculated value. `2 + 1 = 3`
  ![alt image](images/150-1.png#center)
* Next, we are going to apply the multiply operator on the previous two values and get our result `3 * 3 = 9`

### Stack Solution

```swift
func evalRPN(_ tokens: [String]) -> Int {
    var stack: [Int] = []

    for token in tokens {
        switch token {
        case "+":
            stack.append(stack.removeLast() + stack.removeLast())
        case "-":
            let a = stack.removeLast()
            let b = stack.removeLast()
            stack.append(b - a)
        case "*":
            stack.append(stack.removeLast() * stack.removeLast())
        case "/":
            let a = stack.removeLast()
            let b = stack.removeLast()
            stack.append(b / a)
        default:
            if let num = Int(token) {
                stack.append(num)
            }
        }
    }

    return stack[0]
}
```

#### Explanation

From the explanation above, we can see that we can use a stack data structure.

* As we read through the input, each value will be added to the stack.
  ![alt image](images/150-2.png#center)
* Any time we reach an operator, the previous values are removed from the stack, we perform the operation, and push the result back to the stack.
* Since we are guaranteed that the result of our operations will be valid, we will be left with a single value in the stack, which is the value we return.

#### Time/Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
