+++
title = "LeetCode - 150 - Gas Station"
date = 2026-05-02T10:02:45+03:00
tags = ["LeetCode", "150", "Gas Station", "Greedy", "Swift"]
draft = false
+++

### The problem

There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return *the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return* `-1`. If there exists a solution, it is **guaranteed** to be **unique**.

**Example 1:**

```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 units of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

**Example 2:**

```
Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 units of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 units of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

**Constraints:**

* `n == gas.length == cost.length`
* `1 <= n <= 105`
* `0 <= gas[i], cost[i] <= 104`
* The input is generated such that the answer is unique.

#### Explanation

Let's begin by visualizing the first example; it will help us understand the problem from a different angle.
We are going to find the start position by creating a **diff** array that stores the gas we need to travel to the next station. To calculate the difference, we are going to subtract `cost` from `gas`. If the result is negative, this would mean that we don't have enough gas to travel. For example:
![alt image](images/134.png#center)
The first **three** elements in **diff** are going to be negative and in total form a value of `-6`, therefore we are going to skip them.

The last two elements are going to be positive:

* if we started from the element at index *three*, we would find that it is our result because starting from that index we could move to index *four* and get a total amount of gas equal to `6` that helps us complete the circle and go back to where we started `6 - 6 = 0`.
  ![alt image](images/134-1.png#center)
* if we started from index *four*, we would get `3` gas and not be able to complete the circle because we have negative gas with a total of `-6`.
  ![alt image](images/134-2.png#center)

> In order for this solution to work, the total sum of **gas** must be *greater than or equal* to the total of **cost**. We are going to check it at the beginning of our solution.

The brute force way to solve this problem is to iterate over every position in the *gas* array and check if we have enough gas to complete the circle. This solution will work, but it will take `O(n^2)` time.

### Greedy Solution

#### Explanation

We can solve this problem in a more efficient way by using a greedy approach.

We are going to iterate over each element in the *gas* array, calculating the **diff** value and keeping track of two properties - `totalGas` and `startIndex`.

If `totalGas` becomes negative, this means that we ran out of gas and we need to reset `totalGas` to `0`. We are also going to update `startIndex` to *currentIndex + 1*.
![alt image](images/134-3.png#center)

If `totalGas` becomes positive, we are going to leave it as it is and will continue iterating.
![alt image](images/134-4.png#center)

When we reach the end of the array, we are not going to go back to the start and redo the calculation since we already verified that *sum(gas) >= sum(cost)* and guaranteed the existence of the solution.

As a result, we found that starting from the position `3`, we can reach the end of the array without using all of the gas. We also know that we can only have one unique solution; therefore, we are going to return index `3` in our output.
![alt image](images/134-5.png#center)

#### Code
```swift
func canCompleteCircuit(_ gas: [Int], _ cost: [Int]) -> Int {
    if gas.reduce(0, +) < cost.reduce(0, +) {
        return -1
    }

    var totalGas = 0
    var startIndex = 0

    for i in 0 ..< gas.count {
        let diff = gas[i] - cost[i]

        totalGas += diff

        if totalGas < 0 {
            totalGas = 0
            startIndex = i + 1
        }
    }

    return startIndex
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
