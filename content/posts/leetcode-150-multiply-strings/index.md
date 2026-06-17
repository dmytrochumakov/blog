+++
title = "LeetCode - 150 - Multiply Strings"
date = 2026-06-17T08:13:55+03:00
tags = ["LeetCode", "150", "Multiply Strings", "Math_&_Geometry", "Swift"]
draft = false
+++

### The problem

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Note:** You must not use any built-in BigInteger library or convert the inputs to integers directly.

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Constraints:**

* `1 <= num1.length, num2.length <= 200`
* `num1` and `num2` consist of digits only.
* Both `num1` and `num2` do not contain any leading zeros, except the number `0` itself.

### Multiplication Solution

#### Explanation

We can multiply two numbers using the stacked method. We are going to start iterating from right to left in reverse order, multiplying each value in `num1` with a value in `num2`. If the result is greater than or equal to `10`, we are going to carry an additional value. For example, with `num1 = 123` and `num2 = 45`:

![alt image](images/43.png#center)

* The first step is to multiply `5` and `3`
* The next step is to multiply `5` and `2`
* The next step is to multiply `5` and `1`

When we are done with `5`, we are going to find the result for the next value, `4`. The steps are the same: we are going to multiply `4` with each number in `num1`.

![alt image](images/43-1.png#center)

Lastly, we are going to add both results and find the answer to the problem.

![alt image](images/43-2.png#center)

To avoid any inconvenience with conversion from character to integer and integer to character, we are going to store the result in an array that we can later convert to a string. The array is going to be in reverse order and will look like this:

![alt image](images/43-3.png#center)

> The size of the array is going to be equal to the sum of the lengths of both `num1` and `num2` strings.

In the end, we are going to reverse the array. After reversing it, a few digits at the beginning of the array could be **zeros**, so we need to find the first non-zero value and return the output from that position onward while converting it to a string.

> Dividing by `10` helps us find the carry value, and taking the remainder (`% 10`) helps us find the digit that stays at the current position.

#### Code

```swift
func multiply(_ num1: String, _ num2: String) -> String {
    if num1 == "0" || num2 == "0" {
        return "0"
    }

    var res = Array(repeating: 0, count: num1.count + num2.count)
    let num1Arr = Array(Array(num1).reversed())
    let num2Arr = Array(Array(num2).reversed())

    for i1 in 0 ..< num1.count {
        for i2 in 0 ..< num2.count {
            let digit = num1Arr[i1].wholeNumberValue! * num2Arr[i2].wholeNumberValue!
            res[i1 + i2] += digit
            res[i1 + i2 + 1] += (res[i1 + i2] / 10)
            res[i1 + i2] = (res[i1 + i2] % 10)
        }
    }

    res = Array(res.reversed())
    var firstNonZeroValueIndex = 0
    while firstNonZeroValueIndex < res.count && res[firstNonZeroValueIndex] == 0 {
        firstNonZeroValueIndex += 1
    }

    let output = res[firstNonZeroValueIndex...].map { String($0) }
    return output.joined()
}
```

#### Time/Space complexity

* Time complexity: O(n*m)
* Space complexity: O(n+m)
* Where `n` is the length of `num1` and `m` is the length of `num2`

#### Thank you for reading! 😊
