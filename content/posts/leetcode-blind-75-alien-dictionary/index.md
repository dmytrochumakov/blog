+++
title = "LeetCode - Blind 75 - Alien Dictionary"
date = 2025-03-06T07:40:15+03:00
tags = ["LeetCode", "Blind 75", "Alien Dictionary", "Swift"]
draft = false
+++

### The Problem  
There is a new alien language that uses the English alphabet. However, the order of the letters is unknown to you.  

You are given a list of strings `words` from the alien language's dictionary. It is claimed that the strings in `words` are sorted lexicographically by the rules of this new language.  

> A string `a` is lexicographically smaller than a string `b` if, in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alien language than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters do not differ, then the shorter string is the lexicographically smaller one.  

If this claim is incorrect and the given arrangement of strings in `words` cannot correspond to any order of letters, return `""`.  

Otherwise, return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there are multiple solutions, return any of them.  

#### Examples  

``` 
Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
```

```
Input: words = ["z","x"]
Output: "zx"
```

```
Input: words = ["z","x","z"]
Output: ""
Explanation: The order is invalid, so return "".
```

#### Constraints  
* 1 <= words.length <= 100  
* 1 <= words[i].length <= 100  
* words[i] consists of only lowercase English letters.  

---

### Depth-First Search Solution  

```swift
func alienOrder(_ words: [String]) -> String {
    var adj: [Character: Set<Character>] = [:]
    for w in words {
        for c in w {
            adj[c] = []
        }
    }

    for i in 0 ..< words.count - 1 {
        let w1 = Array(words[i])
        let w2 = Array(words[i + 1])
        let minLen = min(w1.count, w2.count)

        if w1.count > w2.count && String(w1.prefix(minLen)) == String(w2.prefix(minLen)) {
            return ""
        }

        for j in 0 ..< minLen {
            if w1[j] != w2[j] {
                adj[w1[j], default: []].insert(w2[j])
                break
            }
        }
    }

    var visit: [Character: Bool] = [:]
    var res: [Character] = []

    func dfs(_ c: Character) -> Bool {
        if visit[c] != nil {
            return visit[c]!
        }

        visit[c] = true

        for nei in adj[c]! {
            if dfs(nei) {
                return true
            }
        }

        visit[c] = false
        res.append(c)

        return false
    }

    for c in adj.keys {
        if dfs(c) {
            return ""
        }
    }

    return String(res.reversed())
}
```

#### Explanation  
From the description, we learn that we need to figure out the ordering of the alien alphabet. We know that the English language has the ordering `abc…z` with 26 characters, but in this problem, they state that we have a different ordering of these characters, and our goal is to find out this ordering.  

To determine the ordering, we will compare each character in adjacent words. If a character in the second word comes before the corresponding character in the first word, we will update the ordering. For example, given the words `["apple", "ape"]`, we realize that `"ape"` should come before `"apple"`.  

Let's visualize the first example given in the problem:  

`Input: words = ["wrt","wrf","er","ett","rftt"]`  

![alt image](images/p-269.png#center)  

We can see that we can use these characters and their relative order to build a graph. Now, we can traverse the graph.  

We also need to take care of corner cases such as cycles in the graph or the presence of multiple separate graphs.  
- If we detect a cycle in the graph, we return `""`.  
- If we have multiple separate graphs, we use postorder DFS to avoid incorrect ordering and merge separate graphs into a single ordering. 

To do this, we use a `visit` dictionary to track visited nodes and detect cycles.  

##### Time/Space Complexity  
* Time complexity: O(N + V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of unique characters, `E` is the number of edges, and `N` is the sum of the lengths of all strings.  

---

### Topological Sort (Kahn’s Algorithm) Solution  

```swift
func alienOrder(_ words: [String]) -> String {
    var adj: [Character: Set<Character>] = [:]
    var indegree: [Character: Int] = [:]

    for w in words {
        for c in w {
            adj[c] = []
            indegree[c] = 0
        }
    }

    for i in 0..<words.count - 1 {
        let w1 = Array(words[i])
        let w2 = Array(words[i + 1])
        let minLen = min(w1.count, w2.count)

        if w1.count > w2.count && w1.prefix(minLen) == w2.prefix(minLen) {
            return ""
        }

        for j in 0..<minLen {
            if w1[j] != w2[j] {
                if !adj[w1[j]]!.contains(w2[j]) {
                    adj[w1[j]]!.insert(w2[j])
                    indegree[w2[j], default: 0] += 1
                }
                break
            }
        }
    }

    var q: [Character] = []
    for (char, degree) in indegree {
        if degree == 0 {
            q.append(char)
        }
    }

    var res: [Character] = []

    while !q.isEmpty {
        let char = q.removeFirst()
        res.append(char)
        if let neighbors = adj[char] {
            for neighbor in neighbors {
                indegree[neighbor, default: 0] -= 1
                if indegree[neighbor] == 0 {
                    q.append(neighbor)
                }
            }
        }
    }

    return res.count == indegree.count ? String(res) : ""
}
```  

#### Explanation  
We can also solve this problem using **Topological Sort (Kahn’s Algorithm)**. The difference between topological sort and depth-first search is that topological sort takes a directed graph and returns an array of nodes where each node appears before all the nodes it points to.  

> The ordering of the nodes in the array is called **topological ordering**.  

For example:  

![alt image](images/p-269-1.png#center)  

Since node `1` points to nodes `2` and `3`, node `1` appears before them in the ordering. And since nodes `2` and `3` both point to node `4`, they appear before it in the ordering.  

![alt image](images/p-269-2.png#center)  

So `[1, 2, 3, 4, 5]` would be a topological ordering of the graph.  

> Can a graph have more than one topological ordering? **Yes!** In the example above, `[1, 3, 2, 4, 5]` is also a valid ordering.  

##### Time/Space Complexity  
* Time complexity: O(N + V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of unique characters, `E` is the number of edges, and `N` is the sum of the lengths of all strings.

#### Thank you for reading! 😊
