+++
title = "LeetCode - 150 - Partition Labels"
date = 2026-05-18T07:51:15+03:00
tags = ["LeetCode", "150", "Partition Labels", "Greedy", "Swift"]
draft = false
+++

LeetCode - 150 - Partition Labels

### The problem

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part. For example, the string `"ababcc"` can be partitioned into `["abab", "cc"]`, but partitions such as `["aba", "bcc"]` or `["ab", "ab", "cc"]` are invalid.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return *a list of integers representing the sizes of these parts*.

**Example 1:**

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into fewer parts.
```

**Example 2:**

```
Input: s = "eccbbbbdec"
Output: [10]
```

**Constraints:**

* `1 <= s.length <= 500`
* `s` consists of lowercase English letters.

#### Explanation

To better understand how we can solve this problem let's visualize the second example.

If we tried to partition `s` starting after the first `e` character, we would discover that we can't do that because we have a second `e` at the **len(s) - 2** position and the partition has to be contiguous.
![alt image](images/763.png#center)

If we tried to partition `s` starting after the second `e`, we would find that we can't do that since we have character `c` at the **second** and **last** positions, which tells us we can only create **one** partition with **length = 10**.
![alt image](images/763-1.png#center)

### Greedy Solution

#### Explanation

The second example did not show how we can partition a more complex input, so let's look at the first example where we can see that.

In order to find where a single partition ends, we are going to use a hashmap and track the **lastIndex** of every character.
![alt image](images/763-2.png#center)

We are also going to create a `size` property to track the size of the partition and an `end` property to track where the current partition ends.
![alt image](images/763-3.png#center)

When the `lastIndex` from the current character is equal to the `end`, we are going to add `size` to the *output*, **reset** the `size` and start calculating a new partition. We are going to continue this process until we reach the end of `s`.
![alt image](images/763-4.png#center)

#### Code

```swift
func partitionLabels(_ s: String) -> [Int] {
    let sArr = Array(s)
    var lastIndex: [Character: Int] = [:]

    for (i, c) in s.enumerated() {
        lastIndex[c] = i
    }

    var res: [Int] = []
    var size = 0
    var end = 0

    for (i, c) in s.enumerated() {
        size += 1
        end = max(end, lastIndex[c]!)

        if i == end {
            res.append(size)
            size = 0
        }
    }

    return res
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(m)
* Where `n` is the length of string **s** and `m` is the number of unique characters in the string **s**

#### Thank you for reading! 😊
