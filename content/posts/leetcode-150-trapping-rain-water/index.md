+++
title = "LeetCode - 150 - Trapping Rain Water"
date = 2025-05-23T07:18:50+03:00
tags = ["LeetCode", "150", "Trapping Rain Water", "Swift"]
draft = false
+++

### The problem
Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

#### Examples
![alt image](images/rainwatertrap.png#center)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

```
Input: height = [4,2,0,3,2,5]
Output: 9
```

#### Constraints
* n == height.length  
* 1 <= n <= 2 * 10^4  
* 0 <= height[i] <= 10^5

#### Explanation
Let's look at our example and determine the algorithm of how much water each position could trap.  
![alt image](images/42.png#center)

- In the picture, you can see that no matter how much water you put at the position of **zero**, it will spill out because there are no boundaries on the left.  
- The same goes for position **one**, because no matter how much water you put on top of the block with height `1`, it's not going to trap water, since there is no boundary to its left and the water is going to spill out.  
- In the **third** position, we have **one** block of water. If we try to add a second block of water, we can see that we have a right boundary of `2` on the right, but on the left side we only have a boundary with height `1`, meaning that water will spill out.  
- So we see that the height on the right is `2`, and the height on the left is `1`, so we need to take the `minimum` of `1` and `2` - (`min(1, 2)`).

### Prefix & Suffix Arrays Solution

```swift
func trap(_ height: [Int]) -> Int {
    let n = height.count
    if n == 0 {
        return 0
    }

    var leftMax = Array(repeating: 0, count: n)
    var rightMax = Array(repeating: 0, count: n)

    leftMax[0] = height[0]
    for i in 1 ..< n {
        leftMax[i] = max(leftMax[i - 1], height[i])
    }

    rightMax[n - 1] = height[n - 1]
    for i in stride(from: n - 2, to: -1, by: -1) {
        rightMax[i] = max(rightMax[i + 1], height[i])
    }

    var res = 0
    for i in 0 ..< n {
        res += min(leftMax[i], rightMax[i]) - height[i]
    }

    return res
}
```

#### Explanation
Above, we discussed our bottleneck (how much water we were able to trap) for every single position.

- We need the `max` height on the left and `max` height on the right, and we need to take the minimum of those two.

![alt image](images/42-1.png#center)  
Let's take a look at the position of `3`.

- We can’t trap any water here, because when you look at the right section, the maximum height is `3`.  
- When you look at the left section, the maximum height there is `1`.

The calculation will look like this:  
- We are going to take the minimum of the left height and right height and subtract the current height (`min(L, R) - h[i]`). This will help us determine how much water we can trap at position `i`.

If, for example, we have `L = 1`, `R = 3`, and current height `h[i] = 2`, the result will be `min(L, R) - h[i] = -1`. The negative value tells us that we can trap **zero** water at position `h[i]`. We will continue to use this calculation to get the appropriate water count, and when we take the sum of all the calculated values, we will get our result.

Now we can dive into the implementation.  
![alt image](images/42-2.png#center)

Since for every single position, to know how much water we can trap at index `i`, we need to know what the max left and right heights are at every single position. We can make an array and store the calculation for us.  
To do that:
- We are going to scan through the entire array, calculating every single **maxLeft** position.  
- We are also going to calculate **maxRight** in reverse order.  
- Lastly, we are going to take the **minimum** of max **left** and max **right**, because the minimum is going to help us determine how much water is trapped at a specific position.

So now, for every single position, we are going to determine how much water we can trap. We can do it with our calculation: `min(L, R) - h[i]`.  
![alt image](images/42-3.png#center)

This solution will take O(n) time and space complexity. It's a very efficient solution, but we can optimize it even further by reducing memory.

#### Time/Space complexity
* Time complexity: O(n)  
* Space complexity: O(n)

### Solution

```swift
func trap(_ height: [Int]) -> Int {
    let n = height.count

    if n == 0 {
        return 0
    }

    var l = 0
    var r = n - 1
    var res = 0

    var leftMax = height[l]
    var rightMax = height[r]

    while l < r {
        if leftMax < rightMax {
            l += 1
            leftMax = max(leftMax, height[l])
            res += leftMax - height[l]
        } else {
            r -= 1
            rightMax = max(rightMax, height[r])
            res += rightMax - height[r]
        }
    }

    return res
}
```

#### Explanation
From the previous solution, we learn that we can get rid of extra memory by using the two-pointer technique.  
![alt image](images/42-4.png#center)

- We are going to have a **left** pointer at the beginning of the array, and a **right** pointer at the end of the array.  
- We are also going to have two variables: **maxLeft** and **maxRight**, which are going to keep track of the max of the left pointer and max of the right pointer.  
- Now we are going to update our pointers. We are going to shift the pointer that has the smaller max value.  
- When our maxLeft and maxRight are equal, it does not matter which pointer we shift, so we can choose one of them.  
- We continue calculating our result by shifting pointers.  
- At height `2`, we are going to update our right pointer because our **maxL** value is greater than **maxR**.  
- After that, our **maxL** and **maxR** are equal again, and we will be updating the left pointer.  
- We will continue doing this operation until the condition **left < right** is no longer satisfied.

#### Time/Space complexity
* Time complexity: O(n)  
* Space complexity: O(1)

#### Thank you for reading! 😊
