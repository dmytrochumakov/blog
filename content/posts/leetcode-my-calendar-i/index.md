+++
title = "LeetCode - My Calendar I"
date = 2026-11-09T17:28:04+03:00
tags = ["LeetCode", "My Calendar I", "Intervals", "Medium", "Swift"]
draft = false
+++

### The problem

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **double booking**.

A **double booking** happens when two events have some non-empty intersection (i.e., some moment is common to both events).

The event can be represented as a pair of integers `startTime` and `endTime` that represents a booking on the half-open interval `[startTime, endTime)`, the range of real numbers `x` such that `startTime <= x < endTime`.

Implement the `MyCalendar` class:

- `MyCalendar()` Initializes the calendar object.
- `boolean book(int startTime, int endTime)` Returns `true` if the event can be added to the calendar successfully without causing a **double booking**. Otherwise, return `false` and do not add the event to the calendar.

**Example 1:**

```
Input
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
Output
[null, true, false, true]

Explanation
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It cannot be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.
```

**Constraints:**

- `0 <= start < end <= 10^9`
- At most `1000` calls will be made to `book`.

#### Explanation

I'm going to lay down some maybe obvious reasoning here - you can skip it if you want.

From the given shape of the code, we can see that, our main task is to pick the right data structure to store events so that we can efficiently check if we have **double booking**.

![alt image](images/729.png#center)

The usual technique is to check if the **start** of the current event is **higher than or equal to the end of the previous event**, but we are also going to check if the **end is less than or equal to the start** of the event that comes before any existing one.

We can choose an array as our data structure for a brute-force approach and use a for-loop for the double-booking check.

This is a valid solution, but it is not as efficient as it could be.

#### Code

```swift
class MyCalendar {
    
    private var arr: [(Int, Int)]
    
    init() {
        self.arr = []
    }
    
    func book(_ startTime: Int, _ endTime: Int) -> Bool {
        if self.arr.isEmpty {
            self.arr.append((startTime, endTime))
            return true
        } else {
            for (start, end) in self.arr {
                if startTime >= end || endTime <= start {
                    continue
                } else {
                    return false
                }
            }
            self.arr.append((startTime, endTime))
            return true
        }
    }
    
}
```

#### Time/ Space complexity

* Time complexity: O(n) for each *book()* function call
* Space complexity: O(n)


### Binary Search Tree Solution

#### Explanation

The more optimal way to solve this problem is to use a Binary Search Tree.

BST allows us to check for double booking in O(logn) average time.

The underlying logic stays the same:
- We insert to the **right** when **start** is greater than or equal to **end**.
- We insert to the **left** when **end** is less than or equal to **start**.
- We return **false** in any other case.

![alt image](images/729-1.png#center)

#### Code

```swift
final class Node {
    
    let start: Int
    let end: Int
    var left: Node?
    var right: Node?
    
    init(start: Int, end: Int) {
        self.start = start
        self.end = end
    }
    
}

final class BST {
    
    private(set) var root: Node?
    
    func insert(_ startTime: Int, _ endTime: Int) -> Bool {
        if self.root == nil {
            self.root = Node(start: startTime, end: endTime)
            return true
        } else {
            return self.recursiveInsert(self.root, startTime, endTime)
        }
    }
    
    func recursiveInsert(_ node: Node?, _ startTime: Int, _ endTime: Int) -> Bool {
        guard let node = node else { return false }
        
        if startTime >= node.end {
            if node.right == nil {
                node.right = Node(start: startTime, end: endTime)
                return true
            } else {
                return self.recursiveInsert(node.right, startTime, endTime)
            }
        } else if endTime <= node.start {
            if node.left == nil {
                node.left = Node(start: startTime, end: endTime)
                return true
            } else {
                return self.recursiveInsert(node.left, startTime, endTime)
            }
        } else {
            return false
        }
    }
    
}

class MyCalendar {
    
    private var bst: BST
    
    init() {
        self.bst = BST()
    }
    
    func book(_ startTime: Int, _ endTime: Int) -> Bool {
        return self.bst.insert(startTime, endTime)
    }
    
}
```

#### Time/ Space complexity

* Time complexity: O(logn) in the average case, O(n) in the worst case for each *book()* function call
* Space complexity: O(n)

#### Thank you for reading! 😊
