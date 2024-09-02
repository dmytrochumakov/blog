+++
title = 'DSA - Binary Search Tree'
date = 2024-09-02T07:12:55+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Binary Search Tree", "Swift"]
draft = false
+++

### What is a Tree?
A [`Tree`](https://en.wikipedia.org/wiki/Tree_(data_structure)) is a data structure that has a root and subtrees of children, representing a set of linked nodes. `Trees` behave similarly to a [`LinkedList`](https://dmytrosblog.substack.com/p/dsa-linked-list?r=2fepxg) in that they have a collection of nodes starting with a `head (root)`. The main difference is that `Trees` can have multiple children, whereas a `LinkedList`, on the other hand, can have only one `next` child.

I’m going to focus on a commonly used type of tree, the Binary Search Tree.

### Binary Search Tree

A [Binary Search Tree](https://en.wikipedia.org/wiki/Binary_tree) (`BST`) is also called an ordered tree. A `BST` has two children, `left` and `right`. The `left` child value is always less than its parent value, and the `right` child value is always greater than its parent value. Duplicate values are not allowed. These constraints help to `add`, `remove`, and `find` nodes very quickly (on average O(log n), worst case O(n)).

### Code Example

```swift
final class BSTNode {

    let val: Int
    var left: BSTNode?
    var right: BSTNode?

    init(
        val: Int,
        left: BSTNode? = nil,
        right: BSTNode? = nil
    ) {
        self.val = val
        self.left = left
        self.right = right
    }

}
```

### BST - Insert

Let’s look at how binary search tree `insertion` works:
- One of the `BST` constraints is that duplicate values are not allowed, so we need to check for duplicates before adding any logic.
- The next step is to check if the `inserting value` is less than `self.val` and recursively insert this `value` into the `left` child node, or create a new `left` child node if it does not exist.
- The final step is to recursively insert the `value` into the `right` child if it exists, or create a new node.

```swift 
func insert(_ val: Int) {
    if self.val == val {
        return
    }

    if val < self.val {
        if self.left != nil {
            self.left!.insert(val)
            return
        } else {
            self.left = BSTNode(val: val)
            return
        }
    }

    if self.right != nil {
        self.right!.insert(val)
        return
    }

    self.right = BSTNode(val: val)
}
```

### BST - Delete

Let's look at another operation, `delete`, which is slightly more complicated than `insert`:
- The first step is to compare the value the user is trying to delete with `self.val`. If it is less and the `left` child exists, then delete the value from `left` recursively and update `left`.
- The second step is the opposite of the previous step but for the `right` child.
- The third and fourth steps are base cases, where we need to check if `right` and `left` children exist. If the `right` child does not exist, return the `left` child, and vice versa for the `left` child.
- The final step is to find the `minLargerNode`, update the value, and replace the `right` child with this node.

```swift 
func delete(_ val: Int) -> BSTNode? {
    if val < self.val {
        if self.left != nil {
            self.left = self.left!.delete(val)
        }
        return self
    }

    if val > self.val {
        if self.right != nil {
            self.right = self.right!.delete(val)
        }
        return self
    }

    if self.right == nil {
        return self.left
    }

    if self.left == nil {
        return self.right
    }

    var minLargerNode = self.right
    while minLargerNode?.left != nil {
        minLargerNode = minLargerNode!.left
    }

    self.val = minLargerNode!.val
    self.right = self.right?.delete(minLargerNode!.val)

    return self
}
```

#### Thank you for reading! 😊
