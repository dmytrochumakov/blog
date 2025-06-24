+++
title = "LeetCode - 150 - Largest Rectangle in Histogram"
date = 2025-06-24T07:51:28+03:00
tags = ["LeetCode", "150", "Largest Rectangle in Histogram", "Swift"]
draft = false
+++

### The problem

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

#### Examples

![alt image](images/histogram.jpg#center)

```
Input: heights = [2,1,5,6,2,3]  
Output: 10  
Explanation: The above is a histogram where width of each bar is 1.  
The largest rectangle is shown in the red area, which has an area = 10 units.  
```

![alt image](images/histogram-1.jpg#center)

```
Input: heights = [2,4]  
Output: 4  
```

#### Constraints

* 1 <= heights.length <= 10^5
* 0 <= heights\[i] <= 10^4

#### Explanation

Before we jump to the solution, let's visualize the problem with `Input: heights = [2,1,5,6,2,3]`
![alt image](images/84.png#center)
After we calculated the area, you can see that the largest rectangle has an area `10`.
Now let's figure out why it works this way.
If we just draw the first two elements, we can see the pattern that with rectangle with height `2`, once we reached the `1`, we can't extend it any further, because `1 < 2` and we have a hole.
For rectangle with value `1`, we can extend it towards the left and use the left rectangle.
![alt image](images/84-1.png#center)
Now let's look at an example when the `1` comes first
![alt image](images/84-2.png#center)
In this case, we can keep extending it to the right because nothing is stopping us.
In case if two rectangles are even, we can extend it to the right too.
After analyzing all cases from above, we can conclude that current heights will be in increasing order.

### Stack Solution

```swift
func largestRectangleArea(_ heights: [Int]) -> Int {
    var maxArea = 0
    var stack: [(Int, Int)] = []

    for (i, h) in heights.enumerated() {
        var start = i
        while !stack.isEmpty && stack.last!.1 > h {
            let (index, height) = stack.removeLast()
            maxArea = max(maxArea, height * (i - index))
            start = index
        }
        stack.append((start, h))
    }

    for (i, h) in stack {
        maxArea = max(maxArea, h * (heights.count - i))
    }

    return maxArea
}
```

#### Explanation

To solve this problem, we will be using a stack. We will be looking for increasing heights and where increasing stopped.
If we found the rectangle that cannot be extended, we will compute the area and **pop** the element.
For example, input with `heights = [1, 2, 3, 4, 2]`
![alt image](images/84-3.png#center)
You can see that we can't extend rectangle with value `4` any further, so we will compute the area and pop `4`.
We also cannot extend rectangle with value `3`, so we need to compute the area and pop it as well.
As for rectangle with value `2`, we can extend it because the rightmost value to it is also `2`.
So in this example, we only need to remove values `3` and `4`.

Now let's look at the algorithm.

![alt image](images/84-4.png#center)

* We start at index `0`, and add height `2` to our stack
* Next, we get to index `1`, and add height `1` to our stack, but you can see that we have height `1` at the top of our stack, and we can't extend area with height `2` any further. The max area that we have so far is `two`, because our width is `one`, and our height is `two`, and now we can pop rectangle with value `2` from our stack. We can also extend rectangle at index `one` to the left, so index for this height will be `zero`
* Next, we are going to get to index `two` and height `5`, five is greater than one and it can be extended, so we are going to add it to our stack
* Next, we get to index `three`, and again, height `6` is greater than `5`, so we are going to add it to our stack
* Next, at index `four` we have height `2`, and we can't extend previous rectangle with height `6` any further, therefore we need to compute the area and pop it
* Now, the top value in our stack has height `5`, and it is also greater than `2`, so we need to compute the area, and pop it. As for start index for rectangle with height `2`, we can extend backwards to the index `two`, because we just popped two elements
* Lastly, we reach our index `five` with height `3`, so we put it into our stack, and we do not need to pop previous value `2`, because `3 > 2`, and we can continue extending. As for start index, we can't extend backwards anymore so it will be `5`

At this point, we have three values that are still in our stack, and we need to compute areas for these heights

* At index `five` and height `3`, it started and went all the way to the end of the histogram, that means that the length for it is just `1`, and we can compute the area of `3`. The area of `3` is not greater than our current maximum of `10`, so we don't update our max
* At the next index of `2`, we have height `2`, so this means that the area starts from index `2`, and went all the way to the end of histogram. This means that width was `4` and height was `2`, so the computed area equals `8`. The calculated area is not greater than `10`, so we don't update our max area
* At the last area of our stack with index `0`, and height `1`, tells us that it started at index `0` and went all the way to the end of the histogram, so the width will be `6`, the height was `1`, so the calculated area equals `6`, this area is not greater than our current value of `10`, so we do not update max area

![alt image](images/84-5.png#center)

#### Time/Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
