+++
title = "LeetCode - 150 - Pow(x, n)"
date = 2026-06-13T10:51:19+03:00
tags = ["LeetCode", "150", "Pow(x, n)", "Math_&_Geometry", "Swift"]
draft = false
+++

### The Problem

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xⁿ`).

**Example 1:**

```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**

```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**

```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2^-2 = 1/2^2 = 1/4 = 0.25
```

**Constraints:**

* `-100.0 < x < 100.0`
* `-2^31 <= n <= 2^31 - 1`
* `n` is an integer.
* Either `x` is not zero or `n > 0`.
* `-10^4 <= x^n <= 10^4`

### Brute Force Solution

#### Explanation

The brute force way to solve this problem without using built-in functions is to use a while loop and multiply the `x` value `n` times. The problem with this solution is that it will not pass on LeetCode because it takes `O(n)` time.

#### Code

```swift
func myPow(_ x: Double, _ n: Int) -> Double {
    var nCopy = abs(n)
    var res: Double = 1

    while nCopy != 0 {
        res *= x
        nCopy -= 1
    }

    if n < 0 {
        return 1 / res
    } else {
        return res
    }
}
```

#### Time/Space Complexity

* Time complexity: O(n)
* Space complexity: O(1)

### Divide & Conquer Solution

#### Explanation

We can solve this problem in a more optimal way by applying the divide and conquer approach.

The divide and conquer approach helps us break the problem into smaller subproblems and eliminate half of the work. For example, with `Input: x = 2.00000, n = 10`, we are going to solve the `2^5` subproblem first.

* Next, we are going to solve `2^2`.
* Next, `2^1`.
* Lastly, `2^0`.

![alt image](images/50.png#center)

Since the divide and conquer approach uses recursion, we must take care of the base cases to avoid infinite recursive calls.

* The first base case is when `n == 0`. Any number that is not **zero** raised to the power of `0` is equal to `1`.
  ![alt image](images/50-1.png#center)
* The second base case is when `x == 0`. We are always going to return `0`.
  ![alt image](images/50-2.png#center)

> To handle negative `n`, we are going to use its absolute value. This way, we avoid any inconvenience in the calculation. Before returning the output, we are going to divide `1` by the result.

#### Code

```swift
func myPow(_ x: Double, _ n: Int) -> Double {
    func helper(_ x: Double, _ n: Int) -> Double {
        if x == 0 {
            return 0
        }

        if n == 0 {
            return 1
        }

        var res = helper(x, n / 2)
        res = res * res

        if n % 2 == 1 {
            return x * res
        } else {
            return res
        }
    }

    let res = helper(x, abs(n))
    if n >= 0 {
        return res
    } else {
        return 1 / res
    }

}
```

#### Time/Space Complexity

* Time complexity: O(log n)
* Space complexity: O(log n) for the recursive stack.

#### Thank you for reading! 😊
