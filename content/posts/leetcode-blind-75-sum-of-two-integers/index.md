+++
title = "LeetCode - Blind 75 - Sum of Two Integers"
date = 2025-05-14T07:56:51+03:00
tags = ["LeetCode", "Blind 75", "Sum of Two Integers", "Swift"]
draft = false
+++

### The problem  
Given two integers `a` and `b`, return the sum of the two integers without using the operators `+` and `-`.

#### Examples

``` 
Input: a = 1, b = 2
Output: 3
```

```
Input: a = 2, b = 3
Output: 5
```

#### Constraints
* -1000 <= a, b <= 1000

#### Explanation  
From the description of the problem, we learn that we need to figure out the way of adding two numbers without using `+` or `-` operators.  
Let's visualize this problem and find the way we can do it.

We know that in binary representation of our number we could have `0` and `1`.  
![alt image](images/371.png#center)  
- Normally, when we use `+` operator, we have a value in binary, for example `a = 1`, `b = 0`, we would get `1` in our output.  
- If binary values in both `a = 1`, `b = 1`, are `1`s, we would get `0` as result.

![alt image](images/371-1.png#center)  
What we have discovered is logical operator `xor` - `^` (exclusive or). Basically, it means:  
- If one of `a` or `b` has `1`, then we will have `1` in the output.  
- If both of `a` and `b` are the same, then we will have `0` in the output.

So `xor` works. If we have `a = 1` and `b = 1`, and we do the `xor` operation, we're going to get a `0` in the output, but we also want a `1` carry.  
> We are going to have a carry only in the case when we have two **ones** `a = 1` and `b = 1`.

To determine if we have a carry, we are going to use the `&` operator, because by using the `&` operator on `a = 1` and `b = 1` we will get `1`.  
![alt image](images/371-2.png#center)

Now we need to move our carry to the left spot. We can do it by shifting `<<` to the left by `1`.  
Now our solution will look like this: `(a & b) << 1`

Summarizing all that we did above:  
- We are going to have a loop  
- Do `xor` operation  
- If we have a carry value, we are going to shift `<<` to the left  

### Solution  
```swift 
func getSum(_ a: Int, _ b: Int) -> Int {
    var a = a
    var b = b

    while (b != 0) {
        let tmp = (a & b) << 1
        a = a ^ b
        b = tmp
    }
    
    return a   
}
```

#### Explanation  
Now as we figured out the way how we are going to solve this problem, let's go through an example with `a = 9`, `b = 11`:  
![alt image](images/371-3.png#center)

- We are going to run `xor` operation `a ^ b` bit by bit, and in result we will get `0010`.  
- We are also going to run `&` operation `a & b`, and shift `<<` it to the left. That will get `10010`.  
- After that, we are going to take the result of the `xor` operation and the `&`, and add them together (by doing the same operations as above).  
- We will continue doing these operations until our carry is equal to `0`.

> We do not need to create additional logic for dealing with **negative** numbers because `xor` and `&` operations are equivalent to **addition**, as long as the language that we are using handles binary representation correctly.

#### Time/ Space complexity  
* Time complexity: O(1)  
* Space complexity: O(1)

#### Thank you for reading! 😊
