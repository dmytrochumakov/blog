+++
title = "LeetCode - Blind 75 - Design Add and Search Words Data Structure"
date = 2025-02-14T09:53:50+03:00
tags = ["LeetCode", "Blind 75", "Design Add and Search Words Data Structure", "Swift"]
draft = false
+++

### The problem  
Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:  
* `WordDictionary()` Initializes the object.  
* `void addWord(word)` Adds `word` to the data structure; it can be matched later.  
* `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots (`'.'`), where dots can be matched with any letter.  

#### Examples  
```  
Input  
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]  
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]  
Output  
[null,null,null,null,false,true,true,true]  

Explanation  
WordDictionary wordDictionary = new WordDictionary();  
wordDictionary.addWord("bad");  
wordDictionary.addWord("dad");  
wordDictionary.addWord("mad");  
wordDictionary.search("pad"); // return False  
wordDictionary.search("bad"); // return True  
wordDictionary.search(".ad"); // return True  
wordDictionary.search("b.."); // return True  
```  

#### Constraints  
* `1 <= word.length <= 25`  
* `word` in `addWord` consists of lowercase English letters.  
* `word` in `search` consists of `'.'` or lowercase English letters.  
* There will be at most 2 dots in `word` for `search` queries.  
* At most `10^4` calls will be made to `addWord` and `search`.  

### Brute force solution  
```swift  
class WordDictionary {  

    private var store: [String]  

    init() {  
        self.store = []  
    }  

    func addWord(_ word: String) {  
        self.store.append(word)  
    }  

    func search(_ word: String) -> Bool {  
        let wordArray = Array(word)  
        let wordCount = wordArray.count  

        for w in self.store {  
            let wArray = Array(w)  
            let wCount = wArray.count  

            if wCount != wordCount {  
                continue  
            }  

            var i = 0  
            while i < wCount {  
                if wArray[i] == wordArray[i] || wordArray[i] == "." {  
                    i += 1  
                } else {  
                    break  
                }  
                if i == wCount {  
                    return true  
                }  
            }  
        }  
        return false  
    }  
}  
```  

#### Explanation  
We can solve this problem in a brute-force way by using an array.  
It is not the most efficient solution because the `search` operation will take O(m * n) time. We can optimize it by using a Trie (Prefix Tree) data structure.  
- For the `addWord` method, we will use the `append` operation, which takes O(1) time.  
- For `search`, we need a little more effort because we cannot only search for a specific `word`, but the input may contain dots (`'.'`), which can be matched with any letter. For example, if we have input `["c", "d"]` and we are looking for `“.d”`, we will iterate through each element in `store` and compare it with the input `word` character by character, also checking if a character can be `.`.  

##### Time/space complexity  
* Time complexity: O(1) for `addWord`, and O(m * n) for `search`.  
* Space complexity: O(m * n),  
  where `m` is the number of words added to the array and `n` is the length of the word.  

### Trie (Prefix Tree) solution  
```swift  
final class TrieNode {  

    var children: [Character: TrieNode?]  
    var endOfWord: Bool  

    init() {  
        self.children = [:]  
        self.endOfWord = false  
    }  
}  

class WordDictionary {  

    private let root: TrieNode  

    init() {  
        self.root = TrieNode()  
    }  

    func addWord(_ word: String) {  
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
        func dfs(_ j: Int, _ node: TrieNode) -> Bool {  
            var curr = node  
            let wordArray = Array(word)  
            let wordCount = wordArray.count  

            for i in j ..< wordCount {  
                let c = wordArray[i]  
                if c == "." {  
                    for child in curr.children.values {  
                        if let childNode = child, dfs(i + 1, childNode) {  
                            return true  
                        }  
                    }  
                    return false  
                } else {  
                    if curr.children[c] == nil {  
                        return false  
                    }  
                    curr = curr.children[c]!!  
                }  
            }  
            return curr.endOfWord  
        }  
        return dfs(0, self.root)  
    }  
}  
```  

#### Explanation  
We can solve this problem in a more optimal way by using a Trie (Prefix Tree) data structure. It will help us decrease the time complexity for `search` from O(m * n) to O(n).  
To do that, we need an additional class `TrieNode` that will store `children` nodes by character and an `endOfWord` property that will tell us when a sequence of characters forms a complete word.  

![alt image](images/p-211.png#center)  

- For `addWord`, we iterate through all characters in the input `word` and mark it as the `endOfWord`.  
- For `search`, we need to add a depth-first search (DFS) helper (backtracking). This allows us to search for a match in every node when we have `.` in the input because `.` matches any character.  
  For example, when we `search` for `.ad` and we have `["bad", "dad", "mad"]` as added words, we return `true` in all these cases because `.ad` matches all of them.  

![alt image](images/p-211-1.png#center)  

This solution is more efficient because when we find a match, we return `true` and stop iterating, using recursion (DFS).  

##### Time/space complexity  
* Time complexity: O(n) for `addWord` and `search`.  
* Space complexity: O(t + n),  
  where `n` is the length of the word and `t` is the total number of `TrieNode`s created in the Trie.  

#### Thank you for reading! 😊
