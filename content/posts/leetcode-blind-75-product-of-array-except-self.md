+++
title = 'LeetCode - Blind 75 - Product of Array Except Self'
date = 2024-11-05T07:25:53+03:00
tags = ["LeetCode", "Blind 75", "Product of Array Except Self", "Swift"]
draft = false
+++

### The problem
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.  
The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.  
You must write an algorithm that runs in `O(n)` time and without using the division operation.

#### Examples
``` 
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

#### Constraints
* 2 <= nums.length <= 10^5
* -30 <= nums[i] <= 30
* The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

Follow up: Can you solve the problem in `O(1)` extra space complexity? (The output array does not count as extra space for space complexity analysis.)

### Brute force solution
```swift
func productExceptSelf(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var res = [Int](repeating: 0, count: n)

    for i in 0 ..< n {
        var val = 1
        for j in 0 ..< n {
            if i == j {
                continue
            }
            val *= nums[j]
            res[i] = val
        }
    }

    return res
}
```

#### Explanation
The brute force solution:
- Iterates through all indices `i` in the range from 0 to `n` (excluded).
- Creates a second loop and iterates through all indices `j` in the range from 0 to `n` (excluded).
- Checks if `i` and `j` indices are equal; if they are, skips the current iteration to the next one.
- Updates `val` property.
- Updates `res` property by index `i` and `val`.
- Returns result.

##### Time/Space Complexity
* Time complexity: O(n^2)
* Space complexity: O(1)

### Solution - 2 - Division
```swift
func productExceptSelf(_ nums: [Int]) -> [Int] {
    let n = nums.count

    var val = 1
    var zeroCnt = 0

    for num in nums {
        if num != 0 {
            val *= num
        } else {
            zeroCnt += 1
        }
    }

    if zeroCnt > 1 {
        return [Int](repeating: 0, count: n)
    }

    var res = [Int](repeating: 0, count: n)

    for (i, c) in nums.enumerated() {
        if zeroCnt != 0 {
            if c != 0 {
                res[i] = 0
            } else {
                res[i] = val
            }
        } else {
            res[i] = val / c
        }
    }

    return res
}
```

#### Explanation
Solution - 2:
- Iterates through all elements in input `nums` and multiplies `val` with `num` if the element is not equal to `0`; if it is, increments `zeroCnt`.
- Checks if `zeroCnt` is more than `1`; if it is, returns an array with `0` elements with length of `n`, otherwise moves to the next step.
- Iterates through `enumerated` `nums` and checks if `zeroCnt` is not equal to `0`; if so, it divides `val` by `c`. If `zeroCnt` is 0, checks if `c` does not equal `0` and sets `res[i]` to `val`; if it does, sets `0` to `res[i]`.
- Returns result.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(1)

### Solution - 3 - Prefix/Postfix
```swift
func productExceptSelf(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var res = [Int](repeating: 0, count: n)
    var prefix = [Int](repeating: 0, count: n)
    var suffix = [Int](repeating: 0, count: n)

    prefix[0] = 1
    suffix[n - 1] = 1

    for i in 1 ..< n {
        prefix[i] = nums[i - 1] * prefix[i - 1]
    }
    for i in stride(from: n - 2, to: -1, by: -1) {
        suffix[i] = nums[i + 1] * suffix[i + 1]
    }
    for i in 0 ..< n {
        res[i] = prefix[i] * suffix[i]
    }

    return res
}
```

#### Explanation
Solution - 3:
- Initializes `res`, `prefix`, and `suffix` arrays.
- Updates the first `prefix` element with `1`.
- Updates the last `suffix` element with `1`.
- Iterates from `0` to `n` (excluded); updates `prefix[i]` with the product of `num` and `prefix` elements at index `i - 1`.
- Iterates in reverse from `n - 2` to `-1`; updates `suffix[i]` with the product of `num` and `suffix` elements at index `i + 1`.
- Iterates from `0` to `n` (excluded); updates `res[i]` with the product of `prefix` and `suffix` elements at index `i`.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

### Solution - 4 - Prefix/Postfix (Optimized)
```swift
func productExceptSelf(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var res = [Int](repeating: 1, count: n)

    var prefix = 1
    for i in 0 ..< n {
        res[i] = prefix
        prefix *= nums[i]
    }

    var postfix = 1
    for i in stride(from: n - 1, to: -1, by: -1) {
        res[i] *= postfix
        postfix *= nums[i]
    }

    return res
}
```

#### Explanation
Solution - 4:
- Iterates through the range of indices from 0 to n (excluded).
- Sets `prefix` to `res[i]`.
- Multiplies `prefix` with the element of `nums[i]`.
- Iterates in reversed order.
- Sets `postfix` to `res[i]`.
- Multiplies `postfix` with the element of `nums[i]`.
- Returns result.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
