+++
title = 'DSA - Red-Black Tree'
date = 2024-09-30T07:17:10+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Red-Black Tree", "Swift"]
draft = false
+++

### What is a Red-Black Tree?

A [Red-Black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree) is a [self-balancing](https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree) binary search tree data structure. When the tree is modified, the new tree is rearranged and "repainted" to restore the coloring properties that constrain how unbalanced the tree can become in the worst case.  
![alt image](images/Red-black_tree_example.png)  
[source](https://upload.wikimedia.org/wikipedia/commons/4/41/Red-black_tree_example_with_NIL.svg)

### Properties

A Red-Black tree has all binary search tree properties, with some additional properties:

1. Every node is either `red` or `black`.  
2. All `nil` nodes are considered `black`.  
3. A `red` node does not have a `red` child.  
4. If a node is `red`, then both its children are `black`.  
5. Every path from a given node to any of its descendant `nil` nodes goes through the same number of `black` nodes.

### Time Complexity

The (re-)balancing is not perfect, but guarantees searching in O(log n) time, where n is the number of entries in the tree. The insert and delete operations, along with tree rearrangement and recoloring, also execute in O(log n) time.

#### Thank you for reading! 😊
