+++
title = "LeetCode - Blind 75 - Remove Nth Node From End of List"
date = 2024-12-25T09:22:40+03:00
tags = ["LeetCode", "Blind 75", "Remove Nth Node From End of List", "Swift"]
draft = false
+++

### The Problem 
Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

#### Examples

![alt image](images/remove_ex1.jpg#center)

``` 
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

```
Input: head = [1], n = 1
Output: []
```

```
Input: head = [1,2], n = 1
Output: [1]
```

#### Constraints
* The number of nodes in the list is `sz`.
* 1 <= sz <= 30
* 0 <= Node.val <= 100
* 1 <= n <= sz

---

### Brute Force Solution 
```swift
func removeNthFromEnd(_ head: ListNode?, _ n: Int) -> ListNode? {
    var nodes: [ListNode] = []
    var curr = head

    while curr != nil {
        nodes.append(curr!)
        curr = curr!.next
    }

    let removeIndex = nodes.count - n
    if removeIndex == 0 {
        return head?.next
    }

    nodes[removeIndex - 1].next = nodes[removeIndex].next

    return head
}
```

#### Explanation
One way to solve this problem is to use extra memory by iterating over all elements in `head` and storing them in an array. The brute force solution allows us to know the index of each element and delete the node.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

---

### Recursive Solution 
```swift
func rec(_ head: ListNode?, _ n: inout Int) -> ListNode? {
    guard let head = head else {
        return nil
    }

    head.next = rec(head.next, &n)
    n -= 1

    if n == 0 {
        return head.next
    }

    return head
}

func removeNthFromEnd(_ head: ListNode?, _ n: Int) -> ListNode? {
    var n = n
    return rec(head, &n)
}
```

#### Explanation
If you take a closer look, the recursive solution is no different from the brute force solution and uses the same time and space complexity.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

---

### Two Pointers Solution 
```swift
func removeNthFromEnd(_ head: ListNode?, _ n: Int) -> ListNode? {
    let dummyNode = ListNode(0, head)
    var n = n
    var curr = head
    var l: ListNode? = dummyNode
    var r: ListNode? = head

    while n > 0 && r != nil {
        r = r?.next
        n -= 1
    }

    while r != nil {
        l = l?.next
        r = r?.next
    }

    l?.next = l?.next?.next

    return dummyNode.next
}
``` 

#### Explanation
We can solve this problem in a more memory-efficient way by using the two-pointers technique. We create an offset between the `left` and `right` pointers by looking at the `nth` node. The `right` pointer is shifted by `n`, while the `left` pointer starts from `0` and moves by `1`. When the `right` pointer becomes `nil`, the `left` pointer will point to the node we need to delete.

For example, for the input `[1, 2, 3, 4, 5]` and `n = 2`, at the start, the `left` pointer will be at `1`, and the `right` pointer will be at `3`. We keep shifting pointers until the `right` pointer reaches the end of the list. At this point, the `left` pointer will be at `4`, and the `right` pointer will be `nil`.

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
