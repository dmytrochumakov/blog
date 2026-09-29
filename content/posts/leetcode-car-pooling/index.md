+++
title = "LeetCode - Car Pooling"
date = 2026-09-29T17:41:19+03:00
tags = ["LeetCode", "Car Pooling", "Heap", "Priority_Queue", "Medium", "Swift"]
draft = false
+++

### The problem

There is a car with `capacity` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer `capacity` and an array `trips` where `trips[i] = [numPassengersi, fromi, toi]` indicates that the `ith` trip has `numPassengersi` passengers and the locations to pick them up and drop them off are `fromi` and `toi`, respectively. The locations are given as the number of kilometers due east from the car's initial location.

Passengers are dropped off before new passengers are picked up at the same location. At every point along the route, the total number of passengers in the car must not exceed `capacity`.

Return `true` if it is possible to pick up and drop off all passengers for all the given trips, or `false` otherwise.

**Example 1:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false
Explanation:
At kilometer 1, 2 passengers are picked up, so the car holds 2.
At kilometer 3, 3 more are picked up, so the car holds 5.
Since 5 > capacity = 4, the trips cannot all be completed.

```

**Example 2:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
Explanation:
At kilometer 1, the car holds 2 passengers.
At kilometer 3, the car holds 5 passengers.
At kilometer 5, the first 2 are dropped off, so the car holds 3.
At kilometer 7, the last 3 are dropped off, so the car holds 0.
The maximum occupancy is 5, which never exceeds capacity = 5.

```

**Constraints:**

- `1 <= trips.length <= 1000`
- `trips[i].length == 3`
- `1 <= numPassengersi <= 100`
- `0 <= fromi < toi <= 1000`
- `1 <= capacity <= 10^5`

#### Explanation

Since we can only move in one direction, we need to sort our input so that `from` is always going to be in increasing order.
![alt image](images/1094.png#center)

This is a straightforward min-heap problem. You only need to know two things: when to `remove` an element and how to store it the right way so the min-heap works as you would expect.

All that we need is to maintain the `total` number of people so that, in the end, we can compare it with `capacity`, or we can return `false` early when it is exceeded.

We are going to remove a location only when we drop off passengers: `minHeap.min!.to <= trip[1]`.
![alt image](images/1094-1.png#center)
We also need a custom comparable helper that helps us always remove the smallest `to` variable.

### Min-Heap Solution

#### Code
```swift
struct Helper: Comparable {
   let to: Int
   let numOfPeople: Int
   
   static func < (lhs: Helper, rhs: Helper) -> Bool {
       return lhs.to < rhs.to
   }
}

func carPooling(_ trips: [[Int]], _ capacity: Int) -> Bool {
   let trips = trips.sorted(by: { $0[1] < $1[1] })
   
   var minHeap: Heap<Helper> = []
   var total = 0
   
   for trip in trips {
       while !minHeap.isEmpty && minHeap.min!.to <= trip[1] {
           let helper = minHeap.removeMin()
           total -= helper.numOfPeople
       }
       
       total += trip[0]
       
       if total > capacity {
           return false
       }
       
       minHeap.insert(Helper(to: trip[2], numOfPeople: trip[0]))
   }
   
   return total <= capacity
}
```

#### Time/ Space complexity
* Time complexity: O(n*logn)
* Space complexity: O(n)

#### Thank you for reading! 😊
