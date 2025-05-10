+++
title = "LeetCode - Blind 75 - Reverse Bits"
date = 2025-05-10T07:59:05+03:00
tags = ["LeetCode", "Blind 75", "Reverse Bits", "Swift"]
draft = false
+++

### The problem  
Reverse bits of a given 32-bit unsigned integer.

Note:
* Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
* In Java, the compiler represents the signed integers using [2’s complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in Example 2 above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

#### Examples

```
Input: n = 00000010100101000001111010011100  
Output:    964176192 (00111001011110000010100101000000)  
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192, whose binary representation is 00111001011110000010100101000000.
```

```
Input: n = 11111111111111111111111111111101  
Output:   3221225471 (10111111111111111111111111111111)  
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471, whose binary representation is 10111111111111111111111111111111.
```

#### Constraints
* The input must be a binary string of length `32`

Follow up: If this function is called many times, how would you optimize it?

### Brute Force Solution  
```swift
func reverseBits(_ n: Int) -> Int {
    var binary = ""

    for i in 0 ..< 32 {
        if ((n & (1 << i)) != 0) {
            binary += "1"
        } else {
            binary += "0"
        }
    }

    var res = 0
    for (i, bit) in binary.reversed().enumerated() {
        if bit == "1" {
            res |= (1 << i)
        }
    }

    return res
}
```

#### Explanation  
Before we jump to the code, let's look at an example with input `n = 4` and its binary representation.  
![alt image](images/190.png#center)

Suppose we had an output variable for the result where we had 32 bits in the output.  
We want to go bit by bit, reversing our input like we would do when reversing a string.

Now the question is how can we go bit by bit, getting the first bit, then the next bit, all the way to the end.

One way to do it is by using the `&` operator:
- If we take input with `0` and `1`, and `&` them, we get output `0` (`0 & 1 = 0`)
- If we have input with `1` and we `&` it with `1`, we get output `1` (`1 & 1 = 1`)

When we want to look at the next place, we are going to use the `<<` shift operator to the left by `1` each time we move to a different spot in the input.  
- For example, if we had a value like `01`, and then we do a shift operation to the left, `01 << 1`, all that does is shift bits to the left by **one** and replace the `1` spot with `0`. Our output will look like this: `01 << 1 = 10`.

Lastly, we are going to convert our binary representation to an **int** value by using logical `|` operation:
- If we have value `0` and we logic `|` it with `1`, we get `1` (`0 | 1 = 1`)
- If we take `0` and we logic `|` it with `0`, we get `0` (`0 | 0 = 0`)

#### Time/Space complexity  
* Time complexity: O(1)  
* Space complexity: O(1)

### Bit Manipulation Solution  
```swift
func reverseBits(_ n: Int) -> Int {
    var res = 0

    for i in 0 ..< 32 {
        let bit = (n >> i) & 1
        res = res | (bit << (31 - i))
    }

    return res
}
```

#### Explanation  
In the previous solution, we used a string that we reversed to get our result, but we can do better by getting rid of the additional string and unnecessary `for` loop, dealing only with bit manipulations.

We are going to use a `for` loop with range from `0` to `32` exclusive.
- After that, we are going to take `n`, shift it to the right `>>` by `i`. This will help us find the target bit, which we will `&` with `1`, so our `bit` will be `1` or `0`.
- Then we want to logic `|` our bit with our output to put this bit into the output.

We are using logic `|` with the bit because we are only going to be updating one spot of our result.
- But we want to update it in reverse order.
- We want to start from our largest bit and work our way down.
- We are going to shift our bit to the left `<<` by `31 - i`.

At the first iteration of the loop, we will be getting the first bit from `n`, and we will be putting it into the `31`st spot of the result.

The time complexity will be O(1) because we are guaranteed that it’s only going to be 32 bits, so the solution does not scale regardless of whatever input `n`.  
And for memory, we are only using a single variable, so it also will be O(1).

#### Time/Space complexity  
* Time complexity: O(1)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
