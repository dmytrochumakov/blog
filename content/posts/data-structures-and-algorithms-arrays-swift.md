+++
title = 'Data Structures and Algorithms Arrays Swift'
date = 2024-08-12T07:01:34+03:00
tags = ["Data Structures and Algorithms", "DSA", "Array", "Swift", "LeetCode"]
draft = false
+++


### Introduction
I’ve always been curious about data structures and algorithms, and how they can improve user experiences while saving money for businesses through optimized computations.

In this series of articles, I’m going to solve LeetCode problems and share my approach with you.

I’ve just started my journey in solving LeetCode problems, so my solutions might not be as efficient as they could be, but I’m always looking for improvement.

Before each topic, I’ll provide a brief introduction to the data structure, algorithm, or technique I’ll be using to solve a specific problem.

### Data Structure
Let’s actually find out what a data structure is.  
A [data structure](https://en.wikipedia.org/wiki/Data_structure) is a way of organizing and storing data that is chosen for efficient access and modification. A data structure is a collection of data values and the relationships among them.

### Algorithm
An [algorithm](https://en.wikipedia.org/wiki/Algorithm) is a finite set of instructions that helps solve a specific problem. A real-world example could be a cooking recipe or the steps to prepare coffee with a coffee machine.

### Arrays
In the Swift programming language, arrays are dynamic by default. A [dynamic array](https://en.wikipedia.org/wiki/Dynamic_array) is a commonly used data structure that consists of a collection of elements of the same memory size, with each element identified by an index or key. You can access, remove, and add new elements to the tail by index in O(1) time.

#### Time Complexity
| Peak Index | Insert or Delete from Beginning | Insert or Delete from End | Insert or Delete from Middle |
|------------|---------------------------------|---------------------------|-----------------------------|
| O(1)       | O(n)                            | O(1)                       | O(n)                        |

### Resizing a Dynamic Array
A dynamic array increases its underlying capacity and resizes to double its size only when the array size equals its capacity, to avoid the expensive cost of resizing frequently.

### Problem
[27. Remove Element](https://leetcode.com/problems/remove-element/description/?envType=study-plan-v2&envId=top-interview-150).
Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.
Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:
* Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
* Return k.

#### Solution

``` swift
class Solution {
    func removeElement(_ nums: inout [Int], _ val: Int) -> Int {
        var i = 0
        while i < nums.count {
            if nums[i] == val {
                nums.remove(at: i)
            } else {
                i += 1
            }
        }
        return nums.count
    }
}

```

#### Approach to Solving Problems
One brute-force way to solve this problem is to loop through all values and check if a number is equal to a value. If it is, then remove the element with this index from the array; if not, increment the index.

**Time Complexity:** O(n) because the value can be at the end of the array, requiring a loop through all values.  
**Space Complexity:** O(1) because no additional space is required; all occurrences of the value are removed in-place.

### Resources
I’m very grateful for the [NeetCode roadmap](https://neetcode.io/roadmap) and in-depth algorithm explanations. I’ve learned a lot about new techniques and ways to solve problems more efficiently. I recommend checking out the [NeetCode site](https://neetcode.io) and [YouTube channel](https://www.youtube.com/c/neetcode).

#### Thank you for reading! 😊
