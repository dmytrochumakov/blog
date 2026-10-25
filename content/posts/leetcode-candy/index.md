+++
title = "LeetCode - Candy"
date = 2026-10-25T18:51:38+03:00
tags = ["LeetCode", "Candy", "Greedy", "Hard", "Swift"]
draft = false
+++

### The problem

There are `n` children standing in a line.

Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have **at least** one candy.
- Children with a **higher** rating get more candies than their neighbors.

Return the **minimum** number of candies you need to have to distribute the candies to the children.

**Example 1:**

```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

```

**Example 2:**

```
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

```

**Constraints:**

- `1 <= n == ratings.length <= 5 * 10^4`
- `0 <= ratings[i] <= 5 * 10^4`

#### Explanation

I don't know why they made this problem hard. Maybe because it's frustrating, or the point was to push the person to figure out all possible edge cases.

There is nothing special or clever about this problem. All that you need to know is what increasing, decreasing, and mixed order in an array are, how to put it all together with an array of ones, and how to use two passes to assign the right amount of candies.

On the first pass, you increase the number of candies only if the ratings are in increasing order.

On the second pass, you do the same but in reverse order, keeping only the max value.

In the end, the sum of candies would be the result.

![alt image](images/135.png#center)

This solution is greedy and takes O(n) time and O(n) space.

There also exists a better solution that makes space complexity O(1). You can try it yourself if you want. I will stick with the current one.

#### Code
```swift
func candy(_ ratings: [Int]) -> Int {
    let n = ratings.count
    
    if n == 1 {
        return 1
    }
    
    var res = Array(repeating: 1, count: n)
    
    for i in 1 ..< n {
        if ratings[i] > ratings[i - 1] {
            res[i] = res[i - 1] + 1
        }
    }
    
    for i in stride(from: n - 2, to: -1, by: -1) {
        if ratings[i] > ratings[i + 1] {
            res[i] = max(res[i], res[i + 1] + 1)
        }
    }
    
    return res.reduce(0, +)
}
```

#### Time/ Space complexity
* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
