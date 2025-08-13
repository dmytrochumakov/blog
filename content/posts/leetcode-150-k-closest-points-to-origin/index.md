+++
title = "LeetCode - 150 - K Closest Points to Origin"
date = 2025-08-13T07:45:42+03:00
tags = ["LeetCode", "150", "K Closest Points to Origin", "Swift"]
draft = false
+++

### The problem

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the X-Y plane is the Euclidean distance (i.e., `√(x1 - x2)^2 + (y1 - y2)^2`).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

#### Examples

![alt image](images/closestplane1.jpg#center)

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```

```
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```

#### Constraints

* 1 <= k <= points.length <= 10^4
* -10^4 <= xi, yi <= 10^4

#### Explanation

From the description of the problem we learn that we have an array of `points` that is represented as a pair of values and we need to return the closest points to the origin, where the origin is the center with coordinates `[0, 0]`. We are also given the `k` parameter that tells us the number of closest elements to the origin that we need to return.

Let’s look at our first example with input `points = [[1,3],[-2,2]], k = 1`
In this example we only need to return a single closest element because `k = 1`.

The first thing that we need to do is figure out how far the given `points` are from the origin. We can do that by using the formula `(x1 - x2)^2 + (y1 - y2)^2`. 

We can avoid using `sqrt` because we do not need to find the distance, we only need to find which one of the points is greater.
![alt image](images/973.png#center)

* When we calculate how far the coordinates `[1, 3]` are we get the result `(1 - 0)^2 + (3 - 0)^2 = 10`
* When we calculate how far the coordinates `[-2, 2]` are we get the result `(-2 - 0)^2 + (2 - 0)^2 = 8`

You can see that coordinates `[-2, 2]` are closer to the origin than `[1, 3]`. Now we need to figure out the way to find `k` closest elements.

### Sorting Solution

```swift
func kClosest(_ points: [[Int]], _ k: Int) -> [[Int]] {
    var points = points
    points.sort { (p1, p2) in
        let dist1 = p1[0] * p1[0] + p1[1] * p1[1]
        let dist2 = p2[0] * p2[0] + p2[1] * p2[1]
        return dist1 < dist2
    }
    return Array(points.prefix(k))
}
```

#### Explanation

The brute force way to find `k` closest elements is to sort our input, but it’s not very efficient in terms of time complexity because it will take `O(n*log(n))` time.

#### Time/ Space complexity

* Time complexity: O(n\*log(n))
* Space complexity: O(1) or O(n) depending on the sorting algorithm

### Min Heap Solution

```swift
func kClosest(_ points: [[Int]], _ k: Int) -> [[Int]] {
    var minHeap: Heap<Helper> = Heap()
    for point in points {
        let x = point[0]
        let y = point[1]
        let dist = x*x + y*y
        minHeap.insert(Helper(dist: dist, x: x, y: y))
    }

    var res: [[Int]] = []
    for _ in 0 ..< k {
        let helper = minHeap.removeMin()
        res.append([helper.x, helper.y])
    }

    return res
}

struct Helper: Comparable {
    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.dist < rhs.dist
    }
    
    let dist: Int
    let x: Int
    let y: Int
}
```

#### Explanation

Above we learned that sorting is not very efficient. We can optimize this problem by using a min heap data structure so our overall time complexity will be `O(k*log(n))`.
![alt image](images/973-1.png#center)

* The first step we are going to do is figure out the distance. We will use it as the first element by putting the coordinates into our min heap because it will be the element that we want our min heap to be ordered by.
* The next step is we will be popping from the min heap `k` times, which will take `O(logn)` time.
  As a result, we will be returning coordinates `[-2, 2]` because they have the smaller distance and we only need to pop once because our `k = 1`.

#### Time/ Space complexity

* Time complexity: O(k\*log(n))
* Space complexity: O(n)

#### Thank you for reading! 😊
