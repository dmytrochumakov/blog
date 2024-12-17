+++
title = 'LeetCode - Blind 75 - Reverse Linked List'
date = 2024-12-17T07:16:48+03:00
tags = ["LeetCode", "Blind 75", "Reverse Linked List", "Swift"]
draft = false
+++

### The problem  
Given the `head` of a singly linked list, reverse the list, and return the reversed list.

#### Examples  
![alt image](images/rev1ex1.jpg#center)  
``` 
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

![alt image](images/rev1ex2.jpg#center)  
```
Input: head = [1,2]
Output: [2,1]
```

```
Input: head = []
Output: []
```

#### Constraints  
* The number of nodes in the list is in the range [0, 5000].  
* -5000 <= Node.val <= 5000  

Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?

### Recursive solution  
```swift
func reverseList(_ head: ListNode?) -> ListNode? {
    guard let head = head else {
        return nil
    }

    var newHead: ListNode? = head
    if head.next != nil {
        newHead = reverseList(head.next)
        head.next?.next = head
    }
    head.next = nil

    return newHead
}
```

#### Explanation  
For the recursive solution, we are going to reverse links between nodes; for example, for a node with values `1 -> 2 -> 3`, we are going to change them to `3 -> 2 -> 1`.  
We can do it by replacing the link between the last and next element: `head.next?.next = head`.

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(n)  

---

### Iterative solution  
```swift
func reverseList(_ head: ListNode?) -> ListNode? {
    var prev: ListNode? = nil
    var curr: ListNode? = head

    while curr != nil {
        var next = curr?.next
        curr?.next = prev
        prev = curr
        curr = next
    }

    return prev
}
```  

#### Explanation  
The iterative solution looks less confusing than the recursive one. We can use three pointers `prev`, `curr`, and `next` and update them accordingly. In the case of input `1, 2, 3`, the `prev` pointer will look like this: `nil <- 1 <- 2 <- 3`.

##### Time/Space Complexity  
* Time complexity: O(n)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
