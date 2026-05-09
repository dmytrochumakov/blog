+++
title = "LeetCode - 150 - Hand of Straights"
date = 2026-05-09T09:16:39+03:00
tags = ["LeetCode", "150", "Hand of Straights", "Greedy", "Swift"]
draft = false
+++

### The problem

Alice has some number of cards, and she wants to rearrange the cards into groups so that each group is of size `groupSize` and consists of `groupSize` consecutive cards.

Given an integer array `hand`, where `hand[i]` is the value written on the `ith` card, and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

**Example 1:**

```
Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3], [2,3,4], [6,7,8]
```

**Example 2:**

```
Input: hand = [1,2,3,4,5], groupSize = 4
Output: false
Explanation: Alice's hand cannot be rearranged into groups of 4.
```

**Constraints:**

* `1 <= hand.length <= 10^4`

* `0 <= hand[i] <= 10^9`

* `1 <= groupSize <= hand.length`

**Note:** This question is the same as 1296: [https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

#### Explanation

Let's begin by visualizing the first example:
![alt image](images/846.png#center)

We can split the input into *three* different groups: `[1, 2, 3]`, `[2, 3, 4]`, `[6, 7, 8]`.
Groups are always going to have a **minimum** value that they start with, so we are going to track it.

It is possible that we can have duplicate values, so we are going to store them in a hashmap. It helps us track values that are available for creating the next group.

> Before we start execution of the algorithm, we need to take care of an edge case. If the size of `hand` divided by `groupSize` has a remainder, we would return `false` because it would be impossible to form groups with the given `groupSize` (one of the groups would have a size less than `groupSize`).

### Greedy Solution

#### Explanation

At this point, we know that we will be using a hashmap to count elements and form the next groups.
We also know that we must find the *minimum* value that each group starts with. One way to do this is to iterate over the entire input every time we need to form a new group. It is not very efficient, as it will take `O(n)` time. Instead, we are going to use a Min Heap and find it in `O(logn)` time.

The way this algorithm is going to work:
- While iterating, we are going to find the *minimum* value in the Min Heap that will be used as a start value in a new group. 
- We are also going to update the count in a hashmap for elements that we used in the new group. 
- Lastly, we are going to **pop** a value from the Min Heap only if the count in the hashmap becomes *zero*.

If we cannot form a new group, we would return `false` early.

For example, let's form the first group:
We are going to start by finding the **minimum** value in the Min Heap.

Next, we are going to take the *minimum*, decrement its count by one in the hashmap, and **pop** it if it becomes *zero*.

After that, we are going to move to the next value in the group while decrementing its count in the hashmap. We are going to continue this process until we form a group with the given `groupSize`.
![alt image](images/846-1.png#center)

So far, we have only discussed examples where we could successfully form a group with the given `groupSize`. Now, let's look at examples where it would be impossible to do so.

If we had a slightly modified input where the last value differed from the previous value by **two**, we would not be able to form the last group. This happens because we were expecting the value `8`, but instead we got `9`.
![alt image](images/846-2.png#center)

Lastly, if we had a missing value at the beginning or in the middle of our group, we would return `false`. For example, if we had *two* `1`s and *one* `2`. After forming the first group, the count of value `2` becomes **zero**, preventing us from forming the second group.
![alt image](images/846-3.png#center)

#### Code
```swift
func isNStraightHand(_ hand: [Int], _ groupSize: Int) -> Bool {
    if hand.count % groupSize == 1 {
        return false
    }

    var count: [Int: Int] = [:]

    for num in hand {
        count[num, default: 0] += 1
    }

    var minHeap: Heap<Int> = Heap(count.keys)

    while !minHeap.isEmpty {
        let first = minHeap.min!

        for num in first ..< first + groupSize {
            if count[num] == nil {
                return false
            }

            count[num]! -= 1

            if count[num] == 0 {
                if num != minHeap.min {
                    return false
                }
                minHeap.removeMin()
            }
        }
    }

    return true
}
```

#### Time/ Space complexity

* Time complexity: `O(n * logn)`
* Space complexity: `O(n)`

#### Thank you for reading! 😊
