+++
title = "LeetCode - 150 - Reverse Nodes in k-Group"
date = 2025-10-01T08:03:59+03:00
tags = ["LeetCode", "150", "Reverse Nodes in k-Group", "Swift"]
draft = false
+++

### The problem

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then the left-out nodes at the end should remain as they are.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

#### Examples

![alt image](images/reverse_ex1.jpg#center)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

![alt image](images/reverse_ex2.jpg#center)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

#### Constraints

* The number of nodes in the list is n.
* 1 <= k <= n <= 5000
* 0 <= Node.val <= 1000

Follow-up: Can you solve the problem in O(1) extra memory space?

#### Explanation

From the description of the problem, we learn that we are given a linked list and we want to reverse nodes `k at a time`.
Let’s figure out what `k at a time` means by using the example `1, 2, 3, 4, 5` and `k = 2`.
![alt image](images/25.png#center)

* We can take our input and divide it into groups where each group has a size `k = 2`.
* Now we have two groups that we can take and reverse one by one. We also need to not forget to update our links where our nodes are pointing to.
* As for our last node that stays without a group, with value `5`, we don’t need to make any changes to it because the problem statement says that we can leave it as it is.
* Lastly, after reversing groups, we must put them together by updating pointers.

### Solution

```swift
func reverseKGroup(_ head: ListNode?, _ k: Int) -> ListNode? {
    let dummy = ListNode(0, head)
    var groupPrev: ListNode? = dummy

    while true {
        let kth = getKth(groupPrev, k)
        if kth == nil {
            break
        }
        var groupNext = kth?.next
        // reverse
        var prev = kth?.next
        var curr = groupPrev?.next

        while curr !== groupNext {
            let tmp = curr!.next
            curr!.next = prev
            prev = curr
            curr = tmp
        }

        let tmp = groupPrev?.next
        groupPrev?.next = kth
        groupPrev = tmp
    }

    return dummy.next
}

func getKth(_ curr: ListNode?, _ k: Int) -> ListNode? {
    var curr = curr
    var k = k
    while curr != nil && k > 0 {
        curr = curr!.next
        k -= 1
    }
    return curr
}
```

#### Explanation

Now, when we figure out the general idea, let’s look at the steps that we need to take to solve this problem. We will be using the same example as we did above.
![alt image](images/25-1.png#center)

* For starters, we will need a `dummy` node because we are potentially modifying the `head` of the linked list. It will help us deal with edge cases.
* Next, we will have a pointer that starts from the `dummy` node. We are going to iterate `k` times to find our first group and reverse it. After we reverse it, we can set our node with value `1` to the node where our condition is stopped — node after `k`, which is equal to `3`.
  ![alt image](images/25-2.png#center)
* Next, we are going to set the next pointer of our node with value `1` to the pointer where our condition is stopped — node with value `3`.
* We also need to update the `dummy` pointer so that it points to the node with value `2`.
  ![alt image](images/25-3.png#center)
* Now, we can move to the next group with values `3` and `4`.
* We are going to update the next pointer of the node with value `3` from pointing to `4` to pointing to `5`.
* Next, we are going to reverse the group.
  ![alt image](images/25-4.png#center)
* Next, we are going to set the next pointer of the node with value `1` to the node with value `4`.
* Lastly, we will try to check if we have another group, and we will find that we don’t, because the next group only contains a single node with value `5`. Therefore, we will stop.

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
