+++
title = "LeetCode - 150 - Jump Game II"
date = 2026-04-26T08:45:38+03:00
tags = ["LeetCode", "150", "Jump Game II", "Greedy", "Swift"]
draft = false
+++

### The problem

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at index 0.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at index `i`, you can jump to any index `(i + j)` where:

* `0 <= j <= nums[i]` and
* `i + j < n`

Return *the minimum number of jumps to reach index* `n - 1`. The test cases are generated such that you can reach index `n - 1`.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

**Constraints:**

* `1 <= nums.length <= 104`
* `0 <= nums[i] <= 1000`
* It's guaranteed that you can reach `nums[n - 1]`.

#### Explanation

To better understand the problem, let's look at the first example.

We start from the first position and we have two choices - we can jump with length **one** or jump with length **two**.

If we jumped with length **two**, we would reach the element at the second position with value `1`.
![alt image](images/45.png#center)
After that, we could make a **one** jump where we end up with another value `1`, and from there we could make **one** more jump and reach the last index.
![alt image](images/45-1.png#center)
In total, we made **three** jumps.

If we decided to jump with length **one**, we would end up at the second position with value `3`, and from there we could jump straight to the last index.
![alt image](images/45-2.png#center)
In total, we made **two** jumps.

The minimum between **three** and **two** is **two**, therefore in our output we are going to return **two**.

One way to solve this problem is to use dynamic programming. The DP solution is very efficient and takes `O(n^2)` time, but we are going to focus on the greedy approach that is more optimal and solves it in `O(n)` time.

### Greedy Solution

#### Explanation

At this point, we learn that we can jump with length **one** or with any length that is in our array.

We are going to use a close/modified version of the Breadth-First Search algorithm that is going to help us find the minimum number of jumps.

Since we know that we can make a decision to jump with length **one** or any length, we can use values in our array as levels in BFS. For example:
If we start from the first element and make a jump with length **one** or **two**, we can get to values `3` or `1` that are going to be the first layer of BFS.
![alt image](images/45-3.png#center)

Next, we are going to find values for the second layer.
From the value `3`, we can jump to its neighbor `1`, but we won't because it's already part of the first layer. Next, we are going to jump to our target element `4` and form the second layer.
![alt image](images/45-4.png#center)

The layers are going to represent the number of jumps that we need to make to reach the last element in the array.

As for the algorithm, we are going to use two pointers **(left and right)** to create a window that serves as input for BFS. For example:
The **left** pointer is going to represent the leftmost portion of the array and the **right** pointer the rightmost portion.
![alt image](images/45-5.png#center)

To determine boundaries for the next portion, we are going to find which element can jump the furthest and update our pointers.
![alt image](images/45-6.png#center)
In our case, the element at the **left** pointer has a value of `3` and can jump a length of **three**, and the element at the **right** pointer has a value of `1` and can jump a length of **one**.

Next, we are going to set the **left** pointer to **right + 1** and the **right** pointer to the furthest element `4`.
![alt image](images/45-7.png#center)

We are going to continue this algorithm until we reach the end of the array.

```swift
func jump(_ nums: [Int]) -> Int {
    var res = 0
    var l = 0
    var r = 0

    while r < nums.count - 1 {
        var furthest = 0

        for i in l ... r {
            furthest = max(furthest, i + nums[i])
        }

        l = r + 1
        r = furthest
        res += 1
    }

    return res
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
