+++
title = 'DSA - Trie'
date = 2024-09-06T06:56:18+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Trie", "Swift"]
draft = false
+++

### What is a Trie?

A [Trie](https://en.wikipedia.org/wiki/Trie) is a data structure usually called a "prefix tree," often represented as a nested tree of dictionaries where each key is a character that maps to the next character in a word.

Let's look at some examples of a Trie. The Trie consists of two main classes: the `TrieNode` class and the `Trie` class. 

### TrieNode

The `TrieNode` class has two properties: 
- `children` - a property that represents all characters in a given word and points to the next character.
- `isEndSymbol` - a property that indicates the end of the word in a given sequence of characters.

```swift
final class TrieNode {

    var children: [Character: TrieNode?]
    var isEndSymbol: Bool

    init() {
        self.children = [:]
        self.isEndSymbol = false
    }

}
```

### Trie
The `Trie` class has two main operations, `insert` and `exists`, and a `root` property that stores all possible combinations of words.

```swift
final class Trie {

    var root: TrieNode

    init() {
        self.root = TrieNode()
    }

}
```

#### Insert
``` swift 
extension Trie {

    func insert(_ word: String) {
        var current = self.root

        for c in word {
            if current.children[c] == nil {
                let newNode = TrieNode()
                current.children[c] = newNode
                current = newNode
            } else {
                current = current.children[c]!!
            }
        }
        current.isEndSymbol = true
    }

}
```

The `insert` operation loops through all the characters of `word` and checks if the character exists in the `current` node:
- If it does not exist, it creates a `newNode`, sets it in the `current.children` node, and updates the `current` node.
- If it does exist, it updates the `current` node with the `children` node containing that character.

#### Exists
``` swift 
extension Trie {

    func exists(_ word: String) -> Bool {
        var current = self.root
        for c in word {
            if current.children[c] == nil {
                return false
            } else {
                current = current.children[c]!!
            }
        }
        return current.isEndSymbol
    }

}
```

The `exists` operation loops over all the characters in the input word and checks if the `current` node contains the character:
- If the `current` node does not have the character, it returns `false`.
- If the `current` node does have the character, it updates the `current` node with the node containing that character.
- When the loop completes, it returns the result based on whether the `current` node's `isEndSymbol` is true. 

#### Thank you for reading! 😊
