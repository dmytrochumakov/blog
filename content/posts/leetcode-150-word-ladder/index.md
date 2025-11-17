+++
title = "LeetCode - 150 - Word Ladder"
date = 2025-11-17T07:48:00+03:00
tags = ["LeetCode", "150", "Word Ladder", "Swift"]
draft = false
+++

### The problem

A transformation sequence from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

* Every adjacent pair of words differs by a single letter.
* Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
* `sk` == `endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the number of words in the shortest transformation sequence from `beginWord` to `endWord`, or `0` if no such sequence exists.

#### Examples

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> "cog", which is 5 words long.
```

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```

#### Constraints

* 1 <= beginWord.length <= 10
* endWord.length == beginWord.length
* 1 <= wordList.length <= 5000
* wordList[i].length == beginWord.length
* beginWord, endWord, and wordList[i] consist of lowercase English letters.
* beginWord != endWord
* All the words in wordList are unique.

#### Explanation

From the description of the problem, we learn that we are given `beginWord`, `endWord`, and `wordList`.

We want to create a sequence from `beginWord` to `endWord` where `endWord` is going to be part of our `wordList`, and `beginWord` might not be a part of `wordList`.

We also have one restriction that says that every adjacent pair of words in the sequence must differ only by **one** character. In the end, we should return the **shortest** sequence.

Let's look at the first example:
![alt image](images/127.png#center)
You can see that the word `hit` is our `beginWord`, and it does not need to be part of `wordList`, but every other word does need to be part of `wordList`, including `endWord = cog`. As a result, the shortest sequence has length `5` because it has **five** different words in it.

As for the solution:

* We know that every single word will have the same length.
* We are going to take two words and figure out what the difference in characters between them is, and if we have **one** character difference, we are going to form a path by creating a bidirectional connection between those two words. For example, in our input we have words `log` and `cog` that differ only by **one** character; therefore, we can form a path between those two words, and each edge between them is going to be bidirectional.

You can see that our problem is more likely a graph problem where we need to find the shortest path from the `beginWord` all the way to the `endWord`. The way we are going to do it is by creating an adjacency list where we will be looking for **one** character difference between words.

### Solution

```swift
func ladderLength(_ beginWord: String, _ endWord: String, _ wordList: [String]) -> Int {
    if !wordList.contains(endWord) {
        return 0
    }
    
    var nei: [String: [String]] = [:]
    var wordList = wordList
    wordList.append(beginWord)
    
    for word in wordList {
        for j in 0 ..< word.count {
            let pattern = String(word.prefix(j)) + "*" + String(word.suffix(word.count - j - 1))
            nei[pattern, default: []].append(word)
        }
    }
    
    var visited: Set<String> = []
    var q: [String] = [beginWord]
    var res = 1
    
    while !q.isEmpty {
        for _ in 0 ..< q.count {
            let word = q.removeFirst()
            if word == endWord {
                return res
            }
            for j in 0 ..< word.count {
                let pattern = String(word.prefix(j)) + "*" + String(word.suffix(word.count - j - 1))
                if let neighbors = nei[pattern] {
                    for neiWord in neighbors {
                        if !visited.contains(neiWord) {
                            visited.insert(neiWord)
                            q.append(neiWord)
                        }
                    }
                }
            }
        }
        res += 1
    }
    
    return 0
}
```

#### Explanation

Above we learn that the first step to our solution is to build an adjacency list. Let's figure out the way we can do that:

To build our adjacency list, we are going to look for word patterns that we can create if we change **one** of the characters in our word. For example, let's look at the word `hot`:
![alt image](images/127-1.png#center)

* We can transform the first character to pattern `*ot`.
* Next, we can change the second character to pattern `h*t`.
* Lastly, we can change the third character to pattern `ho*`.

Now, let's look at the word `dot`:
![alt image](images/127-2.png#center)

* We can transform the first character to pattern `*ot`.
* Next, we can transform the second character to pattern `d*t`.
* Lastly, we can transform the third character to pattern `do*`.

You can see that the last two patterns of `hot` and `dot` are different, but the first pattern is the same, and that means those two words have one character difference. Now we can use the pattern idea to create our adjacency list where the `key` will be a `pattern` and the `value` will be a list of words. For example, for pattern `*ot`, we will have three words that fit that pattern — `["*ot": ["hot", "dot", "lot"]]`.
![alt image](images/127-3.png#center)

If we wanted to find all neighbors of the word `hot`, first we need to find all of its `patterns`:
![alt image](images/127-4.png#center)

* For the `*ot` pattern, our adjacency list will look like this: `["*ot" : ["dot", "lot"]]`.
* For the `h*t` pattern, it will be `["h*t" : ["hit"]]`.

In the end, after all our operations, we can build our graph and calculate the shortest transformation sequence that will look like this:
![alt image](images/127-5.png#center)

* We know that our beginning word is `hit` and the end word is `cog`.
* Next, we are going to use the Breadth First Search algorithm to find the shortest path. We will do it by visiting all neighbors of a particular word.
* We will also keep track of visited nodes because we don't want to visit the same node twice.

As a result, we are going to return `5`, because it took us **five** layers of our BFS to reach the `endWord`.

#### Time/ Space complexity

* Time complexity: O(n^2 * m)
* Space complexity: O(n^2)
* Where `n` is the number of words and `m` is the length of the word

#### Thank you for reading! 😊
