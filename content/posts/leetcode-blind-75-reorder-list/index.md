+++
title = "LeetCode - Blind 75 - Reorder List"
date = 2024-12-23T09:58:35+03:00
tags = ["LeetCode", "Blind 75", "Reorder List", "Swift"]
draft = false
+++

### The Problem
You are given the head of a singly linked list. The list can be represented as:
> `L0 → L1 → … → Ln - 1 → Ln`  

Reorder the list to be in the following form:

> `L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …`  

You may not modify the values in the list's nodes. Only the nodes themselves may be changed.

#### Examples
![alt image](images/reorder1linked-list.jpg#center)
``` 
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

![alt image](images/reorder2-linked-list.jpg#center)
```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

#### Constraints
* The number of nodes in the list is in the range [1, 5 * 10⁴].
* 1 <= Node.val <= 1000

---

### Brute Force Solution
```swift
func reorderList(_ head: ListNode?) {
    guard let head = head else {
        return
    }

    var nodes: [ListNode] = []
    var curr: ListNode? = head
        
    while curr != nil {
        nodes.append(curr!)
        curr = curr!.next
    }

    var l = 0
    var r = nodes.count - 1
        
    while l < r {
        nodes[l].next = nodes[r]
        l += 1
        if l >= r {
            break
        }
        nodes[r].next = nodes[l]
        r -= 1
    }

    nodes[l].next = nil
}
```

#### Explanation
We can solve this problem using additional memory and the two-pointer technique.  
When we iterate over all nodes in `head` and add them to an array, we can precisely know the index of each value. By knowing this, we can reorder the list by updating the `next` pointer. For example, `Input: head = [1,2,3,4]` will look like this: `1 -> 4 -> 2 -> 3 -> nil`.

##### Time/Space Complexity
* **Time Complexity**: O(n)  
* **Space Complexity**: O(n)  

---

### Slow/Fast Pointers Solution
```swift
func reorderList(_ head: ListNode?) {
    guard let head = head else {
        return
    }

    var slow: ListNode? = head
    var fast = head.next

    // Find middle
    while fast != nil && fast?.next != nil {
        slow = slow?.next
        fast = fast?.next?.next
    }

    var second = slow?.next
    slow?.next = nil
    var prev: ListNode?

    // Reverse second half
    while second != nil {
        let tmp = second?.next
        second?.next = prev
        prev = second
        second = tmp
    }

    var first: ListNode? = head
    second = prev

    // Merge two halves
    while second != nil {
        let tmp1 = first?.next
        let tmp2 = second?.next
        first?.next = second
        second?.next = tmp1
        first = tmp1
        second = tmp2
    }
}
```

#### Explanation
To solve this problem in O(1) memory, we reverse the second half of the list.  
To find the second half, we use `slow` and `fast` pointers by shifting the `slow` pointer by `one` step and the `fast` pointer by `two` steps. Finally, we merge the two halves to get the result.

##### Time/Space Complexity
* **Time Complexity**: O(n)  
* **Space Complexity**: O(1)  

#### Thank you for reading! 😊
