+++
title = 'LeetCode - Blind 75 - Merge Two Sorted Lists'
date = 2024-12-20T09:11:43+03:00
tags = ["LeetCode", "Blind 75", "Merge Two Sorted Lists", "Swift"]
draft = false
+++

### The Problem 
You are given the heads of two sorted linked lists, `list1` and `list2`.  
Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.  
Return the head of the merged linked list.

#### Examples
![alt image](images/merge_ex1.jpg#center)

```swift
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

```swift
Input: list1 = [], list2 = []
Output: []
```

```swift
Input: list1 = [], list2 = [0]
Output: [0]
```

#### Constraints
* The number of nodes in both lists is in the range `[0, 50]`.
* `-100 <= Node.val <= 100`
* Both `list1` and `list2` are sorted in **non-decreasing** order.

---

### Recursive Solution 
```swift
func mergeTwoLists(_ list1: ListNode?, _ list2: ListNode?) -> ListNode? {
    guard let list1 = list1 else {
        return list2
    }
    guard let list2 = list2 else {
        return list1
    }
    if list1.val <= list2.val {
        list1.next = mergeTwoLists(list1.next, list2)
        return list1
    } else {
        list2.next = mergeTwoLists(list1, list2.next)
        return list2
    }
}
```

#### Explanation
Before moving to the part where we use recursion, we need to handle the base case and check `list1` and `list2` for `nil` values. Then we compare the node values and move the `next` pointer accordingly.  

For example, with input `list1 = [1,2,4]` and `list2 = [1,3,5]`, we first move the `list1.next` pointer because the condition `list1.val <= list2.val` with values `1` and `1` is `true`. The output at this step looks like `1`. After that, we move `list2.next` because the `list1` input now equals `2` and the condition `list1.val <= list2.val` becomes `false`.

![alt image](images/problem-21.png#center)

##### Time and Space Complexity
* **Time Complexity**: O(n)  
* **Space Complexity**: O(n)

---

### Iterative Solution 
```swift
func mergeTwoLists(_ list1: ListNode?, _ list2: ListNode?) -> ListNode? {
    let dummyNode = ListNode()
    var list1 = list1
    var list2 = list2
    var tail: ListNode? = dummyNode

    while list1 != nil && list2 != nil {
        if list1!.val < list2!.val {
            tail?.next = list1
            list1 = list1?.next
        } else {
            tail?.next = list2
            list2 = list2?.next
        }
        tail = tail?.next
    }

    tail?.next = list1 ?? list2

    return dummyNode.next
}
```

#### Explanation
The iterative solution uses the same logic as the recursive one with an additional trick: the `dummyNode`. This helps avoid edge cases related to the initialization of the node. This solution is memory-efficient (`O(1)`) because it does not require additional memory allocation.

##### Time and Space Complexity
* **Time Complexity**: O(n)  
* **Space Complexity**: O(1)

#### Thank you for reading! 😊
