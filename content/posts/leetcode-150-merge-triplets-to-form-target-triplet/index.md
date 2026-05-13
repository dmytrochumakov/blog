+++
title = "LeetCode - 150 - Merge Triplets to Form Target Triplet"
date = 2026-05-13T07:40:07+03:00
tags = ["LeetCode", "150", "Merge Triplets to Form Target Triplet", "Greedy", "Swift"]
draft = false
+++

### The problem

A **triplet** is an array of three integers. You are given a 2D integer array `triplets`, where `triplets[i] = [ai, bi, ci]` describes the `i-th` **triplet**. You are also given an integer array `target = [x, y, z]` that describes the **triplet** you want to obtain.

To obtain `target`, you may apply the following operation on `triplets` **any number** of times (possibly **zero**):

* Choose two indices (**0-indexed**) `i` and `j` (`i != j`) and **update** `triplets[j]` to become `[max(ai, aj), max(bi, bj), max(ci, cj)]`.

  * For example, if `triplets[i] = [2, 5, 3]` and `triplets[j] = [1, 7, 5]`, `triplets[j]` will be updated to `[max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5]`.

Return `true` *if it is possible to obtain the* `target` ***triplet*** `[x, y, z]` *as an **element** of* `triplets`*, or* `false` *otherwise*.

**Example 1:**

```txt
Input: triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]
Output: true
Explanation: Perform the following operations:
- Choose the first and last triplets [[2,5,3],[1,8,4],[1,7,5]]. Update the last triplet to be [max(2,1), max(5,7), max(3,5)] = [2,7,5]. triplets = [[2,5,3],[1,8,4],[2,7,5]]
The target triplet [2,7,5] is now an element of triplets.
```

**Example 2:**

```txt
Input: triplets = [[3,4,5],[4,5,6]], target = [3,2,5]
Output: false
Explanation: It is impossible to have [3,2,5] as an element because there is no 2 in any of the triplets.
```

**Example 3:**

```txt
Input: triplets = [[2,5,3],[2,3,4],[1,2,5],[5,2,3]], target = [5,5,5]
Output: true
Explanation: Perform the following operations:
- Choose the first and third triplets [[2,5,3],[2,3,4],[1,2,5],[5,2,3]]. Update the third triplet to be [max(2,1), max(5,2), max(3,5)] = [2,5,5]. triplets = [[2,5,3],[2,3,4],[2,5,5],[5,2,3]].
- Choose the third and fourth triplets [[2,5,3],[2,3,4],[2,5,5],[5,2,3]]. Update the fourth triplet to be [max(2,5), max(5,2), max(5,3)] = [5,5,5]. triplets = [[2,5,3],[2,3,4],[2,5,5],[5,5,5]].
The target triplet [5,5,5] is now an element of triplets.
```

**Constraints:**

* `1 <= triplets.length <= 10^5`
* `triplets[i].length == target.length == 3`
* `1 <= ai, bi, ci, x, y, z <= 1000`

#### Explanation

Let's visualize the first example to better understand the problem:
![alt image](images/1899.png#center)

The easiest way to find the maximum is to try every combination that is **less than or equal to the target**. It is not a very efficient solution, as it takes `O(n^2)` time. Instead, we can be greedy and solve it in `O(n)` time.

### Greedy Solution

#### Explanation

The greedy algorithm is more efficient because we are not looking for the **max** value. Instead, we are trying to find if a *triplet* value is **equal to the target value**, and if it is, we are going to **store the index** where that value was found.
![alt image](images/1899-1.png#center)

As for the algorithm, we are going to iterate over `triplets` and **skip** triplets where values are **greater** than the `target`. After the iteration is complete, we are going to check if the length of the indices that we stored is equal to `3`.

#### Code

```swift
func mergeTriplets(_ triplets: [[Int]], _ target: [Int]) -> Bool {
    var indecies: Set<Int> = []

    for triplet in triplets {
        if triplet[0] > target[0] || triplet[1] > target[1] || triplet[2] > target[2] {
            continue
        }

        for (i, v) in triplet.enumerated() {
            if v == target[i] {
                indecies.insert(i)
            }
        }
    }

    return indecies.count == 3
}
```

#### Time/Space complexity

* Time complexity: `O(n)`
* Space complexity: `O(1)` because the length of the `indecies` set is less than or equal to `3`

#### Thank you for reading! 😊
