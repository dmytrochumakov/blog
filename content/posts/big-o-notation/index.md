+++
title = 'Big O notation'
date = 2024-02-21T08:29:30+03:00
tags = ["Big O", "algorithm complexity"]
draft = false
+++

### What is a Big O notation?

The Big O notation helps identify algorithm efficiency. It can measure computation and memory growth with respect to input.
![alt image](images/0.jpg#center)

### Real-world code example
O(n) — Linear Time
``` swift 
func containsValue(array: [Int], value: Int) -> Bool {
    for element in array {
        if element == value {
            return true
        }
    }
    return false
}
```

O(1) — Constant Time
``` swift 
func findFirstElement(array: [Int]) -> Int? {
    return array.first
}
```

Thank you for reading!
