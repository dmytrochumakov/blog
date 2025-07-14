+++
title = "LeetCode - 150 - Add Two Numbers"
date = 2025-07-14T08:00:00+03:00
tags = ["LeetCode", "150", "Add Two Numbers", "Swift"]
draft = false
+++

### The problem

You are given two non‑empty linked lists representing two non‑negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

#### Examples

![alt image](images/addtwonumber1.jpg#center)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

#### Constraints

* The number of nodes in each linked list is in the range \[1, 100].
* 0 ≤ Node.val ≤ 9
* It is guaranteed that the list represents a number that does not have leading zeros.

#### Explanation

When we add two numbers, we need to remember the carry value when it's necessary. The main caveat in this problem is the edge cases; we will discuss them later.
From the description of the problem, we know that we are given two non‑empty linked lists without negative integers, and the digits are stored in reverse order; we will see later that this reverse order will help us solve the problem.

Let's look at a slightly different example with input `3342` and `465`.

* We start adding numbers at the ones place, and since our linked list is in reverse order, it makes this easier.
  ![alt image](images/2.png#center)
* Looking at the input values, you can see that they have different lengths, so when we reach the last position, we will assume that the smaller input has a `zero` at that position.

We have just considered one edge case where the two linked lists have different sizes.
Now let's look at another example where we have two linked lists of the same size but with a carry value.
![alt image](images/2-1.png#center)
In this case, we might forget to add the node containing the carry and just return the result of the addition, so we need to remember that in our solution.

### Solution

```swift
func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
    let dummyNode = ListNode()
    var curr: ListNode? = dummyNode
    var carry = 0
    var l1 = l1
    var l2 = l2

    while l1 != nil || l2 != nil || carry != 0 {
        var v1 = 0
        if l1 == nil {
            v1 = 0
        } else {
            v1 = l1!.val
        }

        var v2 = 0
        if l2 == nil {
            v2 = 0
        } else {
            v2 = l2!.val
        }

        var val = v1 + v2 + carry
        carry = val / 10
        val = val % 10

        curr?.next = ListNode(val)

        curr = curr?.next
        if l1 != nil {
            l1 = l1!.next
        } else {
            l1 = nil
        }

        if l2 != nil {
            l2 = l2!.next
        } else {
            l2 = nil
        }
    }

    return dummyNode.next
}
```

#### Time/ Space complexity

* Time complexity: O(m + n)
* Space complexity: O(1) extra space, O(max(m, n)) for the output list
* Where `m` is the length of `l1` and `n` is the length of `l2`

#### Thank you for reading! 😊
