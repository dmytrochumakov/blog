+++
title = "LeetCode - Blind 75 - Implement Trie (Prefix Tree)"
date = 2025-02-10T13:32:09+03:00
tags = ["LeetCode", "Blind 75", "Implement Trie (Prefix Tree)", "Swift"]
draft = false
+++

### The problem  
> A [trie](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.  

Implement the `Trie` class:  
* `Trie()` Initializes the trie object.  
* `void insert(String word)` Inserts the string `word` into the trie.  
* `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.  
* `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the `prefix` prefix, and `false` otherwise.  

#### Examples  
```  
Input  
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]  
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]  
Output  
[null, null, true, false, true, null, true]  

Explanation  
Trie trie = new Trie();  
trie.insert("apple");  
trie.search("apple");   // return True  
trie.search("app");     // return False  
trie.startsWith("app"); // return True  
trie.insert("app");  
trie.search("app");     // return True  
```  

#### Constraints  
* 1 <= word.length, prefix.length <= 2000  
* `word` and `prefix` consist only of lowercase English letters.  
* At most 3 * 10⁴ calls in total will be made to `insert`, `search`, and `startsWith`.  

### Prefix Tree Array solution  
```swift  
final class TrieNode {  

    var children: [TrieNode?]  
    var endOfWord: Bool  

    init() {  
        self.children = Array(repeating: nil, count: 26)  
        self.endOfWord = false  
    }  

}  

class Trie {  

    private let root: TrieNode  
    private let aAsciiValue: Int = Int(Character("a").asciiValue!)  

    init() {  
        self.root = TrieNode()  
    }  

    func insert(_ word: String) {  
        var curr = self.root  
        for c in word {  
            let i = Int(c.asciiValue!) - aAsciiValue  
            if curr.children[i] == nil {  
                curr.children[i] = TrieNode()  
            }  
            curr = curr.children[i]!  
        }  
        curr.endOfWord = true  
    }  

    func search(_ word: String) -> Bool {  
        var curr = self.root  
        for c in word {  
            let i = Int(c.asciiValue!) - aAsciiValue  
            if curr.children[i] == nil {  
                return false  
            }  
            curr = curr.children[i]!  
        }  

        return curr.endOfWord  
    }  

    func startsWith(_ prefix: String) -> Bool {  
        var curr = self.root  
        for c in prefix {  
            let i = Int(c.asciiValue!) - aAsciiValue  
            if curr.children[i] == nil {  
                return false  
            }  
            curr = curr.children[i]!  
        }  
        return true  
    }  
}  
```  

#### Explanation  
Before we start implementing `Trie`, let's take a look at what we are going to do.  
![alt image](images/p-208.png#center)  

- We are going to create a node and `insert` it as a child of the previous character for every character of the word and mark it as the end of the word. To do that, we need an additional class `TrieNode` that will help us keep track of `children` nodes and `endOfWord`.  
- When we want to `search` for a word, we need to iterate through the word character by character and check if it exists as a `child` and if it is marked as `endOfWord`.  
  - For example, with `insert = "apple"` and `search = "apple"`, the algorithm will return `true` because this word has the same characters and is marked as `endOfWord`.  
  - However, if we try `insert = "apple"` and `search = "app"`, the algorithm will return `false` even though the first three characters are the same. It does so because `"app"` is not marked as `endOfWord`.  
- When we want to search for a word that `startsWith` a prefix, we need to iterate through the word character by character but without checking for `endOfWord` because we just need to find the prefix.  
  - For example, with `insert = "apple"` and searching for a word that `startsWith = "app"`, the algorithm will return `true` because this word has the same first three characters as the word `"apple"`.  

#### Time/Space Complexity  
* **Time complexity:** O(n) for each method call  
* **Space complexity:** O(t)  
  - where `n` is the length of the string and `t` is the total number of `TrieNode` objects created in the `Trie`.  

### Prefix Tree HashMap solution  
```swift  
final class TrieNode {

    var children: [Character: TrieNode?]
    var endOfWord: Bool

    init() {
        self.children = [:]
        self.endOfWord = false
    }

}

class Trie {

    private let root: TrieNode

    init() {
        self.root = TrieNode()
    }

    func insert(_ word: String) {
        var curr = self.root
        for c in word {
            if curr.children[c] == nil {
                curr.children[c] = TrieNode()
            }
            curr = curr.children[c]!!
        }
        curr.endOfWord = true
    }

    func search(_ word: String) -> Bool {
        var curr = self.root
        for c in word {
            if curr.children[c] == nil {
                return false
            }
            curr = curr.children[c]!!
        }
        return curr.endOfWord
    }

    func startsWith(_ prefix: String) -> Bool {
        var curr = self.root
        for c in prefix {
            if curr.children[c] == nil {
                return false
            }
            curr = curr.children[c]!!
        }
        return true
    }
}
```  

#### Explanation  
We can also solve this problem by using a `HashMap` for `TrieNode` instead of an array as we did above. This will result in less code and will be easier to read. The algorithm behind the solution stays the same, as do the time and space complexity.  

#### Time/Space Complexity  
* **Time complexity:** O(n) for each method call  
* **Space complexity:** O(t)  
  - where `n` is the length of the string and `t` is the total number of `TrieNode` objects created in the `Trie`.  

#### Thank you for reading! 😊
