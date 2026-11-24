+++
title = "LeetCode - Rotate Array"
date = 2026-11-24T19:28:28+03:00
tags = ["LeetCode", "Rotate Array", "Two_Pointers", "Medium", "Swift"]
draft = false
+++

### The problem

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

```

**Example 2:**

```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation:
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

```

**Constraints:**

- `1 <= nums.length <= 10^5`
- `-2^31 <= nums[i] <= 2^31 - 1`
- `0 <= k <= 10^5`

**Follow up:**

- Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
- Could you do it in-place with `O(1)` extra space?

#### Explanation

The easiest way to solve this problem is to remove the last element and insert it as the first element `k` times.

Removing from the end is an `O(1)` operation, but inserting at the beginning is `O(n)` because of the underlying array shifting. The overall time complexity is `O(k*n)`. It is a very slow solution, and you would hit a time limit error on LeetCode.

```swift
func rotate(_ nums: inout [Int], _ k: Int) {
    for _ in 0 ..< k {
        let last = nums.removeLast()
        nums.insert(last, at: 0)
    }
}
```

A slightly more optimal solution with `O(n)` time involves additional `O(n)` space.

We are going to create a **copy** of `nums` and move the value at index `i` to a "destination" index `(i + k) % n`, which, when iteration is complete, will give us the right result.

```swift
func rotate(_ nums: inout [Int], _ k: Int) {
    let n = nums.count
    let copy = nums
    
    for i in 0 ..< n {
        let dst = (i + k) % n
        nums[dst] = copy[i]
    }
}
``` 

### Two-Pointers Solution

#### Explanation
Lastly, to get constant space, we are going to use the two-pointers technique, where we are going to reverse the array three times:

- The first time, we just reverse the **entire** array.
![alt image](images/189.png#center)
- The second time, we reverse the **left** part from `0` up to `k - 1`.
![alt image](images/189-1.png#center)
- And the third time, we reverse the **right** part from `k` to `n - 1`.
![alt image](images/189-2.png#center)

To avoid going out of bounds, we are going to take `k` modulo `n`.

For those who are curious, this algorithm is part of [Block swap algorithms](https://en.wikipedia.org/wiki/Block_swap_algorithms), and you can find the math behind it that proves it.

#### Code
```swift
func rotate(_ nums: inout [Int], _ k: Int) {
    let n = nums.count
    let k = k % n
    
    func reverse(_ l: Int, _ r: Int) {
        var l = l
        var r = r
        while l <= r {
            nums.swapAt(l, r)
            l += 1
            r -= 1
        }
    }
    
    reverse(0, n - 1)
    reverse(0, k - 1)
    reverse(k, n - 1)
}
```

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
