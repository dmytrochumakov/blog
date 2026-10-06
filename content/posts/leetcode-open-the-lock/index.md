+++
title = "LeetCode - Open the Lock"
date = 2026-10-06T18:31:09+03:00
tags = ["LeetCode", "Open the Lock", "Graphs", "Medium", "Swift"]
draft = false
+++

### The problem

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example, we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends`, meaning if the lock displays any of these codes, the wheels of the lock will stop turning, and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

**Example 1:**

```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```

**Example 2:**

```
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation: We can turn the last wheel in reverse to move from "0000" -> "0009".
```

**Example 3:**

```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation: We cannot reach the target without getting stuck.
```

**Constraints:**

- `1 <= deadends.length <= 500`
- `deadends[i].length == 4`
- `target.length == 4`
- target **will not be** in the list `deadends`.
- `target` and `deadends[i]` consist of digits only.

#### Explanation

I think the biggest challenge for this problem is to recognize that this is a graph problem and how it can be used to solve it.

I had never seen a lock described like this, so it was quite difficult to even imagine what this problem was about. So I asked ChatGPT to create an image.

![alt image](images/752.png#center) 

My first hypothesis was to use backtracking, where you would go looking for all available combinations by increasing and decreasing numbers, but it was the wrong move since we are only asked to find a `target` with the minimum number of turns.

If you reframe the description as - "you should find the shortest path to the `target`" - you can apply a graph algorithm.

To find the shortest path, you will need the BFS algorithm. It will always return the minimum number of turns.

You will also need to build neighbors for the current combination. You can do this by incrementing or decrementing each wheel by `1`. In total, you will get `4 * 2 = 8` neighbors because each wheel has two choices.

> We should not forget that wheels can wrap around `9` to `0` or `0` to `9`. (For this case, I've built helper functions `nextNumber` and `prevNumber`.)

![alt image](images/752-1.png#center) 

![alt image](images/752-2.png#center) 

While executing BFS, we will check if the current combination equals the `target`, and if it does, we will return the number of turns that we have got so far.

In the end, we will need to take care of two edge cases and use a hash set for `deadends` to speed up lookups to constant time.

### Solution

#### Code

```swift
func openLock(_ deadends: [String], _ target: String) -> Int {
    let deadendsSet = Set(deadends)
    
    if deadendsSet.contains("0000")  {
        return -1
    }
    
    if target == "0000" {
        return 0
    }
    
    func bfs(_ initialCombination: String) -> Int {
        var visitedSet: Set<String> = []
        var q = [initialCombination]
        var res = 0
        
        while !q.isEmpty {
            res += 1
            for _ in 0 ..< q.count {
                let combination = q.removeFirst()
                for nei in buildNeighbors(combination) {
                    if nei == target {
                        return res
                    }
                    if !deadendsSet.contains(nei) && !visitedSet.contains(nei) {
                        visitedSet.insert(nei)
                        q.append(nei)
                    }
                }
            }
        }
        
        return -1
    }
    
    return bfs("0000")
}

func buildNeighbors(_ combination: String) -> [String] {
    var res: [String] = []
    let combinationArr = Array(combination)
    
    for index in 0 ..< combination.count {
        let number = combinationArr[index]
        var combinationCopy = combinationArr
        
        let next = nextNumber(number)
        combinationCopy[index] = next
        res.append(String(combinationCopy))
        
        let prev = prevNumber(number)
        combinationCopy[index] = prev
        res.append(String(combinationCopy))
    }
    
    return res
}

func nextNumber(_ cur: Character) -> Character {
    if cur == "9" {
        return "0"
    }
    let next = cur.wholeNumberValue! + 1
    return Character(String(next))
}

func prevNumber(_ cur: Character) -> Character {
    if cur == "0" {
        return "9"
    }
    let prev = cur.wholeNumberValue! - 1
    return Character(String(prev))
}
```

#### Time/ Space complexity

* Time complexity: O(10^4)
* Space complexity: O(10^4)

#### Thank you for reading! 😊
