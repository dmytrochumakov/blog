+++
title = "LeetCode - 150 - Reverse Integer"
date = 2026-07-01T08:37:18+03:00
tags = ["LeetCode", "150", "Reverse Integer", "Bit_Manipulation", "Swift"]
draft = false
+++


### The problem
Given a signed 32-bit integer `x`, return `x` *with its digits reversed*. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-2^31, 2^31 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**

 

**Example 1:**

```
Input: x = 123
Output: 321

```

**Example 2:**

```
Input: x = -123
Output: -321

```

**Example 3:**

```
Input: x = 120
Output: 21

```

 

**Constraints:**

- `-2^31 <= x <= 2^31 - 1`

### Iteration Solution

#### Explanation
In order to solve this problem, we need to split it into two steps:
- First, we should reverse the integer.
- Second, we should check if the reversed integer fits into 32 bits.

Let's look at the first step:
![alt image](images/7.png#center)

To reverse the integer, we are going to use two operations: **division** and **modulo**.
- Modulo helps us extract the last digit from the integer.
- Division helps us find the remaining digits on the left side. Division removes the last digit, and the rest of the digits stay the same.
- Multiplying the result by `10` helps us shift all existing digits one place to the left, and adding the digit that we found using modulo helps append the reversed digit to the end of the result.

![alt image](images/7-2.png#center)


Now we move to the second step - verifying if the integer fits into 32 bits.

Let's visualize our constraints:
![alt image](images/7-1.png#center)

While reversing the integer, before appending the next digit, we are going to check if the new number would overflow.

We are going to compare the current result with `MAX / 10` (which is `214748364`).

If `result > MAX / 10`, then multiplying would make the result too large.

![alt image](images/7-3.png#center)

If `result == MAX / 10`, then we must check if the last digit is greater than `MAX % 10` (which is `7`). If it is, then the result would exceed `MAX`.

![alt image](images/7-4.png#center)

The same idea applies to negative numbers.

If the result goes below `MIN`, then it overflows.

![alt image](images/7-5.png#center)

#### Code
``` swift
func reverse(_ x: Int) -> Int {
    let MIN = -2147483648  // -2^31,
    let MAX = 2147483647  //  2^31 - 1

    var res = 0
    var x = x

    while x != 0 {
        let digit = x % 10
        x = x / 10

        if res > MAX / 10 || (res == MAX / 10 && digit > MAX % 10) {
            return 0
        }

        if res < MIN / 10 || (res == MIN / 10 && digit < MIN % 10) {
            return 0
        }

        res = (res * 10) + digit
    }

    return res
}
```

#### Time/ Space complexity

* Time complexity: O(1) - Because the maximum number of digits in a 32-bit integer is `10` (`2147483647`, `-2147483648`), the loop executes at most 10 times, which is constant.
* Space complexity: O(1)

#### Thank you for reading! 😊
