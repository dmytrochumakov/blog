+++
title = "LeetCode - Blind 75 - Word Search II"
date = 2025-02-17T12:29:06+03:00
tags = ["LeetCode", "Blind 75", "Word Search II", "Swift"]
draft = false
+++

### The Problem
Given an `m x n` `board` of characters and a list of strings `words`, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

#### Examples
![alt image](images/search1.jpg#center)
```
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
```

![alt image](images/search2.jpg#center)
```
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
```

#### Constraints
* m == board.length
* n == board[i].length
* 1 <= m, n <= 12
* board[i][j] is a lowercase English letter.
* 1 <= words.length <= 3 * 10^4
* 1 <= words[i].length <= 10
* words[i] consists of lowercase English letters.
* All the strings in `words` are unique.

### Brute Force Solution
```swift
func findWords(_ board: [[Character]], _ words: [String]) -> [String] {
    var board = board
    let ROWS = board.count
    let COLS = board[0].count
    var res: [String] = []

    func backtrack(_ r: Int, _ c: Int, _ word: [Character], _ i: Int) -> Bool {
        if i == word.count {
            return true
        }
        if (r < 0 || c < 0 || r >= ROWS ||
            c >= COLS || board[r][c] != word[i]
        ) {
            return false
        }
        let tmp = board[r][c]
        board[r][c] = "*"
        let res = (backtrack(r + 1, c, word, i + 1) ||
                   backtrack(r - 1, c, word, i + 1) ||
                   backtrack(r, c + 1, word, i + 1) ||
                   backtrack(r, c - 1, word, i + 1))
        board[r][c] = tmp
        return res
    }

    for word in words {
        let wordArray = Array(word)
        var flag = false
        for r in 0 ..< ROWS {
            if flag {
                break
            }
            for c in 0 ..< COLS {
                if board[r][c] != wordArray[0] {
                    continue
                }
                if backtrack(r, c, wordArray, 0) {
                    res.append(word)
                    flag = true
                    break
                }
            }
        }
    }

    return res
}
```

#### Explanation
We can solve this problem using a brute-force approach with a backtracking (depth-first search) algorithm. This algorithm allows us to visit neighboring cells.

According to the problem description, we need to construct words from characters available on the `board` that match the input `words`.

The brute-force approach involves visiting each cell on the board and backtracking in four directions to try to construct possible words until we reach the bottom of the grid. 

![alt image](images/p-212.png#center)

However, this is not an efficient solution and will take O(m * n * 4^t + s). We can optimize this solution by using a Trie (Prefix Tree).

##### Time/Space Complexity
* Time complexity: O(m * n * 4^t + s)
* Space complexity: O(t)
* where `m` is the number of rows, `n` is the number of columns, `t` is the maximum length of any word in the `words` array, and `s` is the sum of the lengths of all words.

### Trie (Prefix Tree) Solution
```swift
final class TrieNode {

    var children: [Character: TrieNode]
    var endOfWord: Bool

    init() {
        self.children = [:]
        self.endOfWord = false
    }

    func addWord(_ word: String) {
        var curr = self
        for c in word {
            if curr.children[c] == nil {
                curr.children[c] = TrieNode()
            }
            curr = curr.children[c]!
        }
        curr.endOfWord = true
    }
}

func findWords(_ board: [[Character]], _ words: [String]) -> [String] {
    var root = TrieNode()

    for w in words {
        root.addWord(w)
    }

    let ROWS = board.count
    let COLS = board[0].count
    var res: Set<String> = []
    var visit: Set<[Int]> = []

    func dfs(_ r: Int, _ c: Int, _ node: TrieNode?, _ word: String) {
        var word = word
        if (r < 0 || c < 0 || r >= ROWS ||
            c >= COLS || visit.contains([r, c]) ||
            node?.children[board[r][c]] == nil
        ) {
            return
        }

        visit.insert([r, c])
        let node = node?.children[board[r][c]]
        word += String(board[r][c])

        if node!.endOfWord {
            res.insert(word)
        }

        dfs(r + 1, c, node, word)
        dfs(r - 1, c, node, word)
        dfs(r, c + 1, node, word)
        dfs(r, c - 1, node, word)

        visit.remove([r, c])
    }

    for r in 0 ..< ROWS {
        for c in 0 ..< COLS {
            dfs(r, c, root, "")
        }
    }

    return Array(res)
}
```

#### Explanation
We can optimize the brute-force solution using a Trie (Prefix Tree) data structure. To do this, we need a `TrieNode` class with an `addWord` helper method. For the `findWords` function, we need an additional depth-first search (backtracking) algorithm. The `TrieNode` will help us store words, while DFS will help us search for words on the board in four directions.

In a Prefix Tree, every single character is a node. We insert all input words into the Prefix Tree. For example, with `words = ["app", "ape", "ace"]`:

![alt image](images/p-212-1.png#center)

After constructing the tree, we search for prefixes. If a prefix exists, we continue searching until we find a word or reach a base case. As a result, we find two words `["ape", "ace"]` that can be built using the given board.

##### Time/Space Complexity
* Time complexity: O(m * n * 4 * 3^(t-1) + s)
* Space complexity: O(s)
* where `m` is the number of rows, `n` is the number of columns, `t` is the maximum length of any word in `words`, and `s` is the sum of the lengths of all words.

#### Thank you for reading! 😊
