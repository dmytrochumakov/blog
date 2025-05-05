+++
title = "LeetCode - Blind 75 - Number of 1 Bits"
date = 2025-05-05T07:41:18+03:00
tags = ["LeetCode", "Blind 75", "Number of 1 Bits", "Swift"]
draft = false
+++

### The problem  
Given a positive integer `n`, write a function that returns the number of set bits in its binary representation (also known as the [Hamming weight](https://en.wikipedia.org/wiki/Hamming_weight)).

> A set bit refers to a bit in the binary representation of a number that has a value of 1.

#### Examples

``` 
Input: n = 11  
Output: 3  
Explanation:  
The input binary string 1011 has a total of three set bits.
```

```
Input: n = 128  
Output: 1  
Explanation:  
The input binary string 10000000 has a total of one set bit.
```

```
Input: n = 2147483645  
Output: 30  
Explanation:  
The input binary string 1111111111111111111111111111101 has a total of thirty set bits.
```

#### Constraints  
* 1 <= n <= 2^31 - 1

Follow up: If this function is called many times, how would you optimize it?

#### Explanation  
The brute-force way to solve this problem is to go through every single character in the input string and count all `1`s. This solution will take O(n) time, but we actually can solve it in a more efficient way.  

### Bit Mask Solution  
``` swift 
func hammingWeight(_ n: Int) -> Int {
    var n = n
    var res = 0

    for i in 0 ..< 32 {
        if (((1 << i) & n) != 0) {
            res += 1
        }
    }

    return res
}
``` 

#### Explanation  
Another way that we can solve this problem is by using the logic `&` operator.  
![alt image](images/191.png#center)

When we use the `&` operator on two values, we only get `1` in the case when both values have `1`, and other values will be converted to `0`.

At this moment we have a way to detect if the first bit is `1` or `0`, but we also need to look at the next bit, so we need to get the rest of the bits and shift them to the right by one.  
![alt image](images/191-1.png#center)

This way we can compare two numbers, and if we detect `1`, then we update our result.  
We are using the range `0 .. < 32` because from the constraints we know that `n` fits into a 32-bit integer.

We also have a small downside in our solution—the algorithm has to look at every bit even if it’s not `1`.

#### Time/ Space complexity  
* Time complexity: O(1)  
* Space complexity: O(1)  

### Bit Mask (Optimal) Solution  
``` swift 
func hammingWeight(_ n: Int) -> Int {
    var n = n
    var res = 0

    while n != 0 {
        n = n & (n - 1)
        res += 1
    }

    return res
}
```

#### Explanation  
We can solve this problem in an optimal way by running the algorithm as many times as there are `1`s in our input.  
![alt image](images/191-2.png#center)

We can do this by using the `&` operation, `n - 1` value, and incrementing our result by `1`.  
![alt image](images/191-3.png#center)

This solution works because we `&` the current `n` value with `n - 1`, and update our `n` value with the new one.  

- When we subtract `1` from `n`, we are getting rid of a bit.  
- We also use logic `&` to combine the two values, because we are removing one bit that contains `1`.
- Next, when we subtract `1`, we get rid of the next `1` bit.

Even when we introduce a bunch of other `1`s, this will not affect us because every new `1` bit that we introduced will be on the right side, and they do not matter because we `&` `n` with `n - 1`— they are all going to cancel out.

Basically, what we did is skip all `0`s in between, and we allowed ourselves to run the loop as many times as there are `1` bits in the input.

#### Time/ Space complexity  
* Time complexity: O(1)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
