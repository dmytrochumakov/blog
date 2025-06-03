+++
title = "LeetCode - 150 - Min Stack"
date = 2025-06-03T07:29:07+03:00
tags = ["LeetCode", "150", "Min Stack", "Swift"]
draft = false
+++

### The problem

Design a stack that supports `push`, `pop`, `top`, and **retrieving the minimum element in constant time**.

Implement the `MinStack` class:

* `MinStack()` initializes the stack object.
* `void push(int val)` pushes the element `val` onto the stack.
* `void pop()` removes the element on the top of the stack.
* `int top()` gets the top element of the stack.
* `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

#### Examples

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

#### Constraints

* -2^31 <= val <= 2^31 - 1
* Methods `pop`, `top`, and `getMin` operations will always be called on non-empty stacks.
* At most 3 \* 10^4 calls will be made to `push`, `pop`, `top`, and `getMin`.

#### Explanation

Before we jump into the solution,
let's look at our example from the description
![alt image](images/155.png#center)

* we push value `-2`
* next, we push value `0`
* next, we push value `-3`
  
Now we want to `getMin`. The brute-force way to do this is to look into every single value and find the **min** value. This approach will take O(n) time.

### Two Stacks Solution

```swift
class MinStack {

    private var stack: [Int]
    private var minStack: [Int]

    init() {
        self.stack = []
        self.minStack = []
    }

    func push(_ val: Int) {
        self.stack.append(val)

        if self.minStack.isEmpty {
            self.minStack.append(val)
            return
        } else {
            let val = min(val, self.minStack.last!)
            self.minStack.append(val)
        }
    }

    func pop() {
        if self.stack.isEmpty {
            return
        }
        self.stack.removeLast()
        self.minStack.removeLast()
    }

    func top() -> Int {
        if self.stack.isEmpty {
            return -1
        }
        return self.stack.last!
    }

    func getMin() -> Int {
        if self.minStack.isEmpty {
            return -1
        }
        return self.minStack.last!
    }
}
```

#### Explanation

Above we learned that we can solve the problem in a brute-force way in O(n) time. But in the problem description, we see that we need to solve it in O(1) time.
To do that, we can try to create a single variable that will track our **minimum**
![alt image](images/155-1.png#center)
The problem with this approach occurs when the **popped** element is the minimum. Now we don't know what the new **minimum** is
![alt image](images/155-2.png#center)

A good workaround to this problem is to store the current **minimum** at each position, so when we **pop**, we will know our new **minimum**
![alt image](images/155-3.png#center)
You can see that we defined another stack:

* one stack tells us the order and values that we added so far
* the other stack tells us what the **minimum** values are that we added so far in each position of the stack
* if we add a value, we will be inserting into both stacks
* if we pop a value, we will be popping from both stacks

#### Time/ Space complexity

* Time complexity: O(1) for all operations
* Space complexity: O(n)

#### Thank you for reading! 😊
