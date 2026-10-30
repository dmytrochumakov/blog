+++
title = "LeetCode - Bitwise AND of Numbers Range"
date = 2026-10-30T17:01:02+03:00
tags = ["LeetCode", "Bitwise AND of Numbers Range", "Bit_Manipulation", "Medium", "Swift"]
draft = false
+++

### The problem

Given two integers `left` and `right` that represent the range `[left, right]`, return _the bitwise AND of all numbers in this range, inclusive_.

**Example 1:**

```
Input: left = 5, right = 7
Output: 4

```

**Example 2:**

```
Input: left = 0, right = 0
Output: 0

```

**Example 3:**

```
Input: left = 1, right = 2147483647
Output: 0

```

**Constraints:**

- `0 <= left <= right <= 2^31 - 1`

#### Explanation

This problem is a great one. I haven't had so much fun in a long time.

> The bitwise AND operation is represented by the `&` symbol. It compares two numbers bit by bit. If the first bit equals the second bit and both bits are `ones`, then the resulting bit is `one`.

The underlying issue with brute force approach is that iterating through the entire range from `left` to `right` would be inefficient, as it would take `O(n)` time, and we would get a time limit exceeded error on LeetCode.

You would probably think - We have the range of positive integers that is in sorted order, so can we use something like binary search? And you would be right. There is a way to cut our linear time to `log` time by shifting bits to the **right** `>> 1`. Shifting bits **removes lower bits** and **divides** the number in **half**, which gives us `logn` performance.

![alt image](images/201.png#center)

So the solution is simple - we need to shift bits to the `right` in both numbers until they are both equal - this gives us a **common prefix** of two numbers that we can use later.

- While we are looking for the common prefix, we count the number of shifts.
- Then we take `left` and shift it back by the number of shifts.

![alt image](images/201-1.png#center)

Shifting to the left puts the prefix back in its original position, filling the removed bits with zeros and giving us the answer.

Since we are dividing both numbers by two on each iteration, the time complexity is going to be O(logn), and we are not using additional space, so the space complexity is going to be O(1).

#### Code

```swift
func rangeBitwiseAnd(_ left: Int, _ right: Int) -> Int {
    var left = left
    var right = right
    var shifts = 0
    
    while left != right {
        left >>= 1
        right >>= 1
        shifts += 1
    }
    
    return left << shifts
}
```

### Solution 2

#### Explanation

There is also another way of solving this problem, and you can find it in Brian Kernighan's famous book, The C Programming Language.

It is even more efficient than the logn solution, as it only counts the number of set bits (1s) by clearing the lowest set bit.

![alt image](images/201-2.png#center)

With the same input, it only took two operations to find the result, whereas the above solution took five (I included the shift to the left).

#### Code

```swift
func rangeBitwiseAnd(_ left: Int, _ right: Int) -> Int {
    var right = right
    
    while right > left {
        right = right & (right - 1)
    }
    
    return right
}
```

The time complexity is O(k), where k is the number of 1-bits, and the space complexity is O(1).

#### Thank you for reading! 😊
