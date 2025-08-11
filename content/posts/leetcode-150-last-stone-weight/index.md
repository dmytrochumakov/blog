+++
title = "LeetCode - 150 - Last Stone Weight"
date = 2025-08-11T07:25:28+03:00
tags = ["LeetCode", "150", "Last Stone Weight", "Swift"]
draft = false
+++

### The problem

You are given an array of integers `stones` where `stones[i]` is the weight of the `ith` stone.

We are playing a game with the stones. On each turn, we choose the heaviest two stones and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:

* If `x == y`, both stones are destroyed, and
* If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is at most one stone left.

Return the weight of the last remaining stone. If there are no stones left, return `0`.

#### Examples

```
Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation: 
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of the last stone.
```

```
Input: stones = [1]
Output: 1
```

#### Constraints

* 1 <= stones.length <= 30
* 1 <= stones\[i] <= 1000

#### Explanation

From the description of the problem, we learn that we have a collection of stones that we can smash together at each turn.
In case the stones have the same weight, both of the stones are going to be destroyed. If they are not the same weight, then the smaller stone will be destroyed, and the largest stone will be the difference between the weights.

For example, if we had stones with weights `7` and `6`, the stone with value `6` will be destroyed, and the larger stone’s weight will be reduced to `7 - 6 = 1`.

### Sorting Solution

```swift
func lastStoneWeight(_ stones: [Int]) -> Int {
    var stones = stones

    while stones.count > 1 {
        stones.sort()
        let y = stones.removeLast()
        let x = stones.removeLast()
        let curr = y - x
        if curr != 0 {
            stones.append(curr)
        }
    }

    if !stones.isEmpty {
        return stones[0]
    } else {
        return 0
    }

}
```

#### Explanation

One way to solve this problem is to sort our input each time before smashing stones.

It’s a valid solution but not very efficient because it will take `O(n^2*log(n))` time.

We can solve this problem in a more efficient way by using a max heap data structure.

#### Time/ Space complexity

* Time complexity: O(n^2\*log(n))
* Space complexity: O(1) or O(n) depending on the sorting algorithm

### Max Heap Solution

```swift
func lastStoneWeight(_ stones: [Int]) -> Int {
    var heap = Heap(stones)

    while heap.count > 1 {
        let y = heap.removeMax()
        let x = heap.removeMax()
        if x < y {
            heap.insert(y - x)
        }
    }

    if !heap.isEmpty {
        return heap.max!
    } else {
        return 0
    }

}
```

#### Explanation

Another way to solve this problem is to use a max heap data structure.

The first step that we will need to do is to create a max heap from our input, which will take `O(n)` time.

Next, we will need to find the `max` stones and smash them together.

Let’s look at our first example and imagine that we have already created a max heap.
![alt image](images/1046.png#center)

* The first largest stone is going to be `8`, and the second will be equal to `7`. We smash them together and get the result of `8 - 7 = 1`, then we add `1` to our array of stones.
* The next biggest stones are `4` and `2`. We smash them together and get `4 - 2 = 2`, and add `2` to our array of stones.
* Now, we are left with values `2, 1, 1, 1`, so we smash stones with values `2` and `1` and get a new stone with value `1` that we add to our array.
* Lastly, we are left with three `1`'s, where two of them will be smashed together, and we will be left with a single stone with value `1` that we will return as our result.

#### Time/ Space complexity

* Time complexity: O(n\*log(n))
* Space complexity: O(n)

#### Thank you for reading! 😊
