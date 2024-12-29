+++
title = 'LeetCode - Blind 75 - Merge k Sorted Lists'
date = 2024-12-29T12:33:03+03:00
tags = ["LeetCode", "Blind 75", "Merge k Sorted Lists", "Swift"]
draft = false
+++

### The problem  
You are given an array of `k` linked lists `lists`, each linked list is sorted in ascending order.  
Merge all the linked lists into one sorted linked list and return it.

#### Examples
```swift
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

```swift
Input: lists = []
Output: []
```

```swift
Input: lists = [[]]
Output: []
```

#### Constraints  
* `k == lists.length`  
* `0 <= k <= 10^4`  
* `0 <= lists[i].length <= 500`  
* `-10^4 <= lists[i][j] <= 10^4`  
* `lists[i]` is sorted in ascending order.  
* The sum of `lists[i].length` will not exceed `10^4`.

---

### Brute Force Solution  
```swift
func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
    var nodes: [Int] = []
	
    // Step 1
    for i in 0..<lists.count {
        var lst = lists[i]
        while lst != nil {
            nodes.append(lst!.val)
            lst = lst?.next
        }
    }
	
    // Step 2
    nodes.sort()
	
    // Step 3
    var res = ListNode(0)
    var curr: ListNode? = res

    for node in nodes {
        curr?.next = ListNode(node)
        curr = curr?.next
    }

    return res.next
}
```

#### Explanation  
We can solve this problem by separating it into three steps:  
1. **Find all node values in the `lists`:** This way, we can create an unsorted single list.  
2. **Sort the created list:** This allows us to form a sorted linked list.  
3. **Create a sorted linked list** from the sorted values.

##### Time/Space Complexity  
* **Time complexity:** `O(n * m)` where `n` is the length of `lists` and `m` is the length of the linked lists.  
* **Space complexity:** `O(n)`.  

---

### Solution 2  
```swift
func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
    if lists.isEmpty {
        return nil
    }

    var lists = lists

    while lists.count > 1 {
        var mergedLists: [ListNode?] = []

        for i in stride(from: 0, to: lists.count, by: 2) {
            var l1 = lists[i]
            var l2: ListNode?

            if i + 1 < lists.count {
                l2 = lists[i + 1]
            }
            mergedLists.append(merge(l1, l2))
        }

        lists = mergedLists
    }

    return lists[0]
}

func merge(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
    let dummyNode = ListNode()
    var l1 = l1
    var l2 = l2
    var tail: ListNode? = dummyNode

    while l1 != nil && l2 != nil {
        if l1!.val < l2!.val {
            tail?.next = l1
            l1 = l1!.next
        } else {
            tail?.next = l2
            l2 = l2!.next
        }
        tail = tail?.next
    }

    tail?.next = l1 ?? l2

    return dummyNode.next
}
```

#### Explanation  
We can solve this problem by dividing it into sub-problems:  
1. **Find the first and second linked lists:**  
   - Iterate through `lists.count` until it equals `1`, using `stride` with `step = 2` to find the first and second portions of the linked lists.  
2. **Merge them:**  
   - Use the merge function for linked lists, which is a common problem in itself.

##### Time/Space Complexity  
* **Time complexity:** `O(n * log k)`  
* **Space complexity:** `O(k)`  
Where `k` is the total number of lists, and `n` is the total number of nodes across `k` lists.  

#### Thank you for reading! 😊
