+++
title = "LeetCode - Blind 75 - Counting Bits"
date = 2025-05-08T07:47:53+03:00
tags = ["LeetCode", "Blind 75", "Counting Bits", "Swift"]
draft = false
+++

### The problem  
Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (`0 <= i <= n`), `ans[i]` is the number of `1`s in the binary representation of `i`.

#### Examples

``` 
Input: n = 2  
Output: [0,1,1]  
Explanation:  
0 --> 0  
1 --> 1  
2 --> 10  
```

```
Input: n = 5  
Output: [0,1,1,2,1,2]  
Explanation:  
0 --> 0  
1 --> 1  
2 --> 10  
3 --> 11  
4 --> 100  
5 --> 101  
```

#### Constraints  
* 0 <= n <= 10^5

Follow up:  
* It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?  
* Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

#### Explanation  
Before jumping into the solution, let’s take a look at an example with input `n = 2` and how we can count bits.  
![alt image](images/338.png#center)

When we start counting, we can see that we have three values: `0, 1, 2`.  
- Value `0` has no leading `1`s in binary representation  
- Value `1` has only one `1` bit  
- Value `2` also has only one `1` bit  

As a result, we return an array with `[0, 1, 1]`.

### Bit Manipulation - I Solution  
```swift
func countBits(_ n: Int) -> [Int] {
    var res: [Int] = []

    for num in 0 ..< n + 1 {
        var one = 0
        for i in 0 ..< 32 {
            if ((num & (1 << i)) != 0) {
                one += 1
            }
        }
        res.append(one)
    }

    return res
}
```

#### Explanation  
Now let’s change our input to `3` -> `011`, and find the way we can count multiple `1`s.  
![alt image](images/338-1.png#center)

We are going to use `&` operator, which will help us determine if the current bit is equal to `1`.  
![alt image](images/338-2.png#center)

We will also be using the `<<` shift operator to the right. This way, we can cross out the current bit and find the rest of the bits.

#### Time/Space complexity  
* Time complexity: O(n*logn)  
* Space complexity: O(1) extra space, O(n) space for the output array

### Bit Manipulation (DP) Solution  
```swift
func countBits(_ n: Int) -> [Int] {
    var dp = Array(repeating: 0, count: n + 1)
    var offset = 1

    for i in 1 ..< n + 1 {
        if offset * 2 == i {
            offset = i
        }
        dp[i] = 1 + dp[i - offset]
    }

    return dp
}
```

#### Explanation  
Let’s look at the binary representation of `8`:  
![alt image](images/338-3.png#center)

We know that `0` has zero `1`s in binary representation.  
- When we get to `1`, we have one occurrence of `1`  
- For binary representation of `2`, we have one  
- For binary representation of `3`, we have two different `1`s  

When you get to value `4`, you start to notice how we are doing repetitive work.  
You can see that for the values from `4` to `7`, the first two bits repeat the previous four values that we calculated.

So when we want to get the value of `4`, we can take the binary representation of `0` and add `1` to its third bit.  
If you want to get the value of `5`, you can take the binary representation of `1` and add a `1` bit to the third bit.  

You can see that when we calculate how many `1`s are in the binary representation of `4`, we just say `1 + dp[0]` or `1 + dp[n - 4]`. This is a dynamic problem because we are using previous results that we calculated to compute the new results.

When we get to the binary representation of `8`, our offset is no longer **four**, because now the binary representation of `8` represents integer `0`. So we want to take `8`, offset it to **eight**, which is going to get us to **zero**.

For each value, we are going to have the equation `1 + dp[n - offset]`, where `offset` is the most significant bit we have reached so far.

The most significant bit is going to be `[1, 2, 4, 8, 16, …]`. Basically, it doubles in size. We also know that each bit is a **power of two**—that’s what binary represents.  
Let’s look at how we can calculate our offset:  
![alt image](images/338-4.png#center)

We know that `0` is our base case and it has *zero ones*.  
- We know that in the next position of *one*, we reached the most significant bit of `1`, therefore our value will be `1 + dp[n - 1]`.  
- At the next position of *two*, we reached the significant bit of `2`, so our value will be `1 + dp[n - 2]`.  
- At the next position of *three*, we again have the most significant bit of `2`, so it’s going to use the previous value `1 + dp[n - 2]`.  
- When we get to the next element of *four*, we can see that we reached a new **power of two**, therefore we will be modifying our `offset` to `1 + dp[n - 4]`. Similarly, when we get to *eight*, we reach a new **power of two**, so we are going to do `1 + dp[n - 8]`.

We can detect that we reached a new **power of two** by multiplying the previous **power of two** by **2**, and comparing it to the current value we are at.

#### Time/Space complexity  
* Time complexity: O(n)  
* Space complexity: O(1) extra space, O(n) space for the output array

#### Thank you for reading! 😊
