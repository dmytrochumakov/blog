+++
title = "LeetCode - 150 - Reconstruct Itinerary"
date = 2025-12-12T07:55:47+03:00
tags = ["LeetCode", "150", "Reconstruct Itinerary", "Graph", "Swift"]
draft = false
+++

### The problem

You are given a list of airline `tickets` where `tickets[i] = [from_i, to_i]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

* For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

#### Examples

![alt image](images/itinerary1-graph.jpg#center)

```
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
```

![alt image](images/itinerary2-graph.jpg#center)

```
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
```

#### Constraints

* 1 <= tickets.length <= 300
* tickets[i].length == 2
* from_i.length == 3
* to_i.length == 3
* from_i and to_i consist of uppercase English letters.
* from_i != to_i

#### Explanation

From the description of the problem we learn that we are given the array of `tickets` where a single ticket represents a graph edge `from_i` and `to_i` (in other words `source` and `destination`) that connects two nodes together, and where every node represents an airport. In the result we should return the reconstructed itinerary (flight history of the given `tickets`) of the man who started his flight from `JFK` by using `tickets` **exactly once**. Lastly, in case we have multiple solutions we want to return the result in lexical order, for example:
![alt image](images/332.png#center)
Let's imagine that we start from node `A`:
One possible solution is to visit node `C`, and go back from `C` to `A` (`A -> C -> A`)

* Next, we are going to visit node `B`, and go back from `B` to `A` (`A -> B -> A`)
* As a result we would have `A -> C -> A -> B -> A` as a possible solution

We can also change the order and visit node `B` first so our result will look like this `A -> B -> A -> C -> A`

If we compare both of our results we can see that the second result is more preferable because it has the second character `B` that comes earlier in lexical order than `C`.

The way we are going to solve this problem is by using the Depth First Search algorithm and visiting our nodes but without reusing the same edge twice.

### Solution

```swift
func findItinerary(_ tickets: [[String]]) -> [String] {
    var adj: [String: [String]] = [:]
        
    let sortedTickets = tickets.sorted { $0[0] < $1[0] || ($0[0] == $1[0] && $0[1] < $1[1]) }.reversed()

    for ticket in sortedTickets {
        let src = ticket[0]
        let dst = ticket[1]
        adj[src, default: []].append(dst)
    }
        
    var res: [String] = []
        
    func dfs(_ src: String) {
        while let destinations = adj[src], !destinations.isEmpty {
            let dst = adj[src]!.removeLast()
            dfs(dst)
        }
        res.append(src)
    }
        
    dfs("JFK")

    return res.reversed()
}
```

#### Explanation

To solve this problem we are going to create an adjacency list that will help us traverse the graph. The adjacency list will have the **source** that we will map to its **destinations**.
![alt image](images/332-1.png#center)
We are also going to sort our **destinations** because from the description of the problem we learn that if we have multiple results we must return the result that has the smaller lexical order. The way we are going to sort our input is by sorting based on the **destination**.

We are going to start building our result by using DFS and a `visited` set that will help us avoid visiting the same node twice by tracking what `destinations` we visited so far:
![alt image](images/332-2.png#center)

* We will start from node `JFK`, after that we are going to visit its first destination `ATL`, remove it from our adjacency list and add it to our result
  ![alt image](images/332-3.png#center)
* Next, from node `ATL` we are going to visit node `JFK` because it comes first in lexical order, remove it from the adjacency list and add it to our result.
  ![alt image](images/332-4.png#center)
* Next, following the same pattern as we did before, from node `JFK` we only have **one** outgoing edge `SFO`
  ![alt image](images/332-5.png#center)
* Next, from node `SFO` we are going to visit node `ATL`
  ![alt image](images/332-6.png#center)
* Lastly, from node `ATL` we are going to visit node `SFO`

![alt image](images/332-7.png#center)
Last note, the example above shows a graph with a best case scenario where we can visit every node. Now let's look at the case when, if we visit the first node in lexical order, we can't go back.

In this example we start from node `A` and we have two choices:

* We can go to node `B` first or
* We can go to node `C`

![alt image](images/332-8.png#center)
If we try to go to node `B` first we can't go back to node `A` because we don't have an outgoing edge. Therefore, we are going to backtrack to node `A` and visit node `C` even though it comes after `B` in lexical order.

This example shows that in some cases we are going to need to backtrack. When we go along one of the edges we might realize that it does not work and we are going to have to reverse our decision and travel along a different edge.

#### Time/ Space complexity

* Time complexity: O(E * V)
* Space complexity: O(E * V)
* Where `E` is the number of tickets (edges) and `V` is the number of airports (vertices).

#### Thank you for reading! 😊
