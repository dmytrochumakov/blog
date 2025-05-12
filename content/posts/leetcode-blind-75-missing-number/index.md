+++
title = "LeetCode - Blind 75 - Missing Number"
date = 2025-05-12T08:01:36+03:00
tags = ["LeetCode", "Blind 75", "Missing Number", "Swift"]
draft = false
+++

### The problem
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

#### Examples

```
Input: nums = [3,0,1]
Output: 2
Explanation:
n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

```
Input: nums = [0,1]
Output: 2
Explanation:
n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation:
n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```

#### Constraints
* n == nums.length
* 1 <= n <= 10^4
* 0 <= nums[i] <= n
* All the numbers of `nums` are unique.

Follow up: Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

### Solution
```swift
func missingNumber(_ nums: [Int]) -> Int {
    let n = nums.count
    var numsSet = Set(nums)
    for i in 0 ..< n + 1 {
        if !numsSet.contains(i) {
            return i
        }
    }
    return 0
}
```

#### Explanation
One way to solve this problem is to use additional memory by using a **hash set**.  
Let's look at an example with `nums = [3,0,1]`,  
![alt image](images/268.png#center)

We are given **three** distinct numbers, so the range that we are looking at is from `0` to `3`, and in this range we have **four** different numbers: `0, 1, 2, 3`.

We are going to iterate through numbers `0, 1, 2, 3` and check if we have this number in our **hash set**, and if it's not, we will return it as our result.

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(n)

### Bitwise XOR Solution
```swift
func missingNumber(_ nums: [Int]) -> Int {
    let n = nums.count
    var res = n
    for i in 0 ..< n {
        res ^= i ^ nums[i]
    }
    return res
}
```

#### Explanation
Another way that we can solve this problem with O(1) memory is by using the `xor` - `^` binary operator.

Suppose we had two numbers, `2` and `3`, that we want to `^` together  
![alt image](images/268-1.png#center)

You can do a `^` operation by looking at each bit:  
- If both of them are different, `0` and `1`, the output is going to be `1`.
- If both bits are the same, the output is going to be `0`.

When we have both numbers that are the same, for example `5` and `5`, we will get `0` in the output, because the binary representation of these numbers is the same  
![alt image](images/268-2.png#center)

> The order in which we do the `^` operation does not matter, because values that are the same cancel each other out.

Now we are at the point where we can apply the `^` operator and solve this problem.  
We will be using the example with `nums = [3,0,1]`,  
![alt image](images/268-3.png#center)

We are going to take the range `[0, 1, 2, 3]`, and use the `^` operator on the input array `[3, 0, 1]`. We can see that values:
- `0`s are going to cancel out
- We are going to get rid of the `1`s
- `3`s are going to cancel out as well

At the end, we have value `2` that did not show up twice, because that was the missing number, which will be the answer.

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(1)

### Math Solution
```swift
func missingNumber(_ nums: [Int]) -> Int {
    let n = nums.count
    var res = n
    for i in 0 ..< n {
        res += i - nums[i]
    }
    return res
}
```

#### Explanation
The easiest way to solve this problem with O(1) space is by using a math operation.  
We will be using the example with `nums = [3,0,1]`,  
![alt image](images/268-4.png#center)

The idea behind it looks like this:  
- We are going to take the `sum` of range `[0, 1, 2, 3]` and subtract the `sum` of the input array `[3, 0, 1]`, after that we would be left with the result of `2`.

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
