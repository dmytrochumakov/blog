+++
title = "LeetCode - 150 - Daily Temperatures"
date = 2025-06-12T07:47:38+03:00
tags = ["LeetCode", "150", "Daily Temperatures", "Swift"]
draft = false
+++

### The problem

Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

#### Examples

```
Input: temperatures = [73,74,75,71,69,72,76,73]  
Output: [1,1,4,2,1,1,0,0]  
```

```
Input: temperatures = [30,40,50,60]  
Output: [1,1,1,0]  
```

```
Input: temperatures = [30,60,90]  
Output: [1,1,0]  
```

#### Constraints

* 1 <= temperatures.length <= 10^5
* 30 <= temperatures\[i] <= 100

#### Explanation

Before we jump to the solution, let's figure out the way we can solve this problem.
![alt image](images/739.png#center)
In the first example, we can see that we can calculate how many days in the input array it takes us to find a temperature that is greater than `73`.

* We can see that on the next day we have temperature `74` that is greater than `73`, so it took us **one** day
* We will need **one** day to find a temperature that is greater than `74`, because the next day contains temperature `75`
* We will need **four** days to get the next value that is greater than `75`
* For the last two positions, we will return `0` because nothing to the right is greater than `76` and there is no temperature to the right after `73`

### Brute force Solution

```swift
func dailyTemperatures(_ temperatures: [Int]) -> [Int] {
    let n = temperatures.count
    var res: [Int] = []

    for i in 0 ..< n {
        var count = 1
        var j = i + 1

        while j < n {
            if temperatures[j] > temperatures[i] {
                break
            }
            j += 1
            count += 1
        }

        if j == n {
            count = 0
        }

        res.append(count)
    }

    return res
}
```

#### Explanation

![alt image](images/739-1.png#center)
The brute force way to solve this problem is to look at every temperature and scan through the entire temperature array

* find out how many days it will take us to find a temperature that is greater than the current one.
* We will need to do the same thing for every temperature until we have looked through all elements in the array.

This solution will take O(n^2) time complexity, but there is a more optimal way to solve it if we use extra memory.

#### Time/ Space complexity

* Time complexity: O(n^2)
* Space complexity: O(1) extra space, O(n) space for the output array

### Stack Solution

```swift
func dailyTemperatures(_ temperatures: [Int]) -> [Int] {
    let n = temperatures.count
    var res: [Int] = Array(repeating: 0, count: n)
    var stack: [[Int]] = []

    for (i, t) in temperatures.enumerated() {
        while !stack.isEmpty && t > stack.last![0] {
            let val = stack.removeLast()
            let stackT = val[0]
            let stackIdx = val[1]
            res[stackIdx] = (i - stackIdx)
        }
        stack.append([t, i])
    }

    return res
}
```

#### Explanation

From the solution above, we learn that we can solve this problem in a more optimal way using a stack.
To do that, we need to know the previous temperatures that we looked at.

* If we find a temperature that is greater than the current temperature in our stack, we will find the difference, add it to our output, and remove the smallest temperature from our stack.
  ![alt image](images/739-2.png#center)
* In case we have a temperature that is less than the top temperature in our stack, we can't remove the value from our stack; instead, we will add the next value. The values in our stack will be in monotonic decreasing order
  ![alt image](images/739-3.png#center)
* In case we have temperatures in our stack in decreasing order and we encounter a new value that is greater than our top value in the stack, we will pop all values that are less than the new temperature
  ![alt image](images/739-4.png#center)

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
