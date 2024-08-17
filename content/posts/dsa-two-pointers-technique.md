+++
title = 'DSA - Two Pointers Technique'
date = 2024-08-17T07:00:23+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Two Pointers", "Swift"]
draft = false
+++

### What is the Two Pointers Technique?
The two pointers technique helps track indices in a collection of elements to access objects in memory by index with O(1) space. This technique is very handy when you need to optimize the time and space of a solution.

### What Problems Does It Solve?
The two pointers technique solves problems involving collections. For example, it is useful when you need to compare each element to other elements in that collection.

### What Are the Ways to Use It?
The first way to use the two pointers technique is to set the `left` pointer at the beginning of the array and the `right` pointer at the end, then `increment` the `left` and `decrement` the `right` pointer until they meet.

```swift 
while l < r {
    l += 1
    r -= 1
}
```

The second way is to use `slow` and `fast` pointers for cycle detection in a LinkedList. It is called `fast` and `slow` because the `fast` pointer moves `twice` as fast as the `slow` pointer.
```swift 
class Node {
    var val: Int 
    var next: Node?

    init(val: Int, next: Node? = nil) {
        self.val = val
        self.next = next
    }
}

func hasCycle(_ head: Node?) -> Bool {
    var fast = head
    var slow = head
    while fast != nil && fast?.next != nil {
        fast = fast?.next?.next
        slow = slow?.next
        if fast == slow {
            return true
        }
    }
    return false
}
```

### Problem
As an example, let's look at the [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/) problem.

Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.
Return the indices of the two numbers, `index1` and `index2`, added by one, as an integer array `[index1, index2]` of length 2. The tests are generated such that there is exactly one solution. You may not use the same element twice.
Your solution must use only constant extra space.

### Solution
Let's look at the solution to the [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/) problem that uses the two pointers technique, where the `left` pointer is initialized with the first index in the array and the `right` pointer is initialized with the last index.
```swift 
class Solution {
    func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
        var l: Int = 0
        var r: Int = numbers.count - 1
        while l < r {
            if numbers[l] + numbers[r] < target {
                l += 1
            } else if numbers[l] + numbers[r] > target {
                r -= 1
            } else {
                return [l+1,r+1]
            }
        }
        return []
    }
}
```

#### Thank you for reading! 😊
