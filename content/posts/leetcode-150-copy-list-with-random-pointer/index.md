+++
title = "LeetCode - 150 - Copy List with Random Pointer"
date = 2025-07-13T07:50:36+03:00
tags = ["LeetCode", "150", "Copy List with Random Pointer", "Swift"]
draft = false
+++

### The problem

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [deep copy](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointers of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

* `val`: an integer representing `Node.val`
* `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will only be given the `head` of the original linked list.

#### Examples

![alt image](images/e1.png#center)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

![alt image](images/e2.png#center)

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

![alt image](images/e3.png#center)

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

#### Constraints

* 0 <= n <= 1000
* -10^4 <= Node.val <= 10^4
* Node.random is null or is pointing to some node in the linked list.

#### Explanation

When we look at the first example, we can see that we have a singly linked list. The only difference between the given linked list and a regular linked list is that every single node has one extra pointer called `random`. You can see that it points to different nodes and that pointer can point anywhere.
![alt image](images/138.png#center)
From the description of the problem, we learn that we need to create a deep copy of the given input.
Deep copy means that we allocate new memory by creating new nodes.
In the first example, we have `five` nodes in the input, so we need to create five nodes in the output.
![alt image](images/138-1.png#center)
The problem occurs when we are trying to create `random` pointers.
Let's take the `third` node for example:

![alt image](images/138-2.png#center)

* We create a clone of the first node, then we create the clone of the second node, and lastly we create a clone of the third node
* The `random` pointer at the first node is going to be at `null`
* The `random` pointer of the second node is going to be pointing at the `first` node
* As for the `random` pointer of the third node, it's going to point at the `fifth` node, but we haven't created a deep copy of the fifth node yet.
  
The question becomes, how can we assign a `random` pointer before the deep copy has been created?

The answer is we are going to use two passes:
* At the first pass, we are going to create a deep copy of each node without linking them. We will be doing it with a hash map, where we will map the old node to a new copy.
* At the second pass, we will be connecting our pointers

### Hash Map (Two Pass) Solution

```swift
func copyRandomList(_ head: Node?) -> Node? {
    var oldToCopy: [Node?: Node?] = [nil : nil]
    var curr = head

    while curr != nil {
        let copy = Node(curr!.val)
        oldToCopy[curr!] = copy
        curr = curr!.next
    }

    curr = head
    while curr != nil {
        let copy = oldToCopy[curr!]!
        copy?.next = oldToCopy[curr!.next]!
        copy?.random = oldToCopy[curr!.random]!
        curr = curr!.next
    }

    return oldToCopy[head]!
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
