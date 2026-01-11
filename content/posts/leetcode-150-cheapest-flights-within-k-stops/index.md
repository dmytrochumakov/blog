+++
title = "LeetCode - 150 - Cheapest Flights Within K Stops"
date = 2026-01-11T08:57:42+03:00
tags = ["LeetCode", "150", "Cheapest Flights Within K Stops", "Graph", "Swift"]
draft = false
+++

### The problem

There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [from_i, to_i, price_i]` indicates that there is a flight from city `from_i` to city `to_i` with cost `price_i`.

You are also given three integers `src`, `dst`, and `k`, return the **cheapest price** from `src` to `dst` with at most `k` stops. If there is no such route, return `-1`.

#### Examples

![alt image](images/cheapest-flights-within-k-stops-3drawio.png#center)

```
Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.
```

![alt image](images/cheapest-flights-within-k-stops-1drawio.png#center)

```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.
```

![alt image](images/cheapest-flights-within-k-stops-2drawio.png#center)

```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500
Explanation:
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.
```

#### Constraints

* `2 <= n <= 100`
* `0 <= flights.length <= (n * (n - 1) / 2)`
* `flights[i].length == 3`
* `0 <= from_i, to_i < n`
* `from_i != to_i`
* `1 <= pricei <= 10^4`
* There will not be any multiple flights between two cities.
* `0 <= src, dst, k < n`
* `src != dst`

#### Explanation

![alt image](images/787.png#center)
From the description of the problem, we learn that this is a graph problem where we are given an array of nodes, where each node represents a city, and each edge in the graph is **directed** and represents a `flight` that connects two cities together. We also learn that each edge has a weight that represents a `cost`. We need to find the **cheapest cost** from `src` to `dst` with at most `k` stops.

You might think that we can use Dijkstra's algorithm to find the shortest path, but in order to do so, we would need to modify it because we have a `k` stops constraint that is not part of Dijkstra's algorithm.

### Bellman-Ford Solution

```swift
func findCheapestPrice(_ n: Int, _ flights: [[Int]], _ src: Int, _ dst: Int, _ k: Int) -> Int {
    var prices: [Int] = Array(repeating: Int.max, count: n)
    prices[src] = 0

    for i in 0 ..< k + 1 {
        var tmpPrices = prices

        for flight in flights {
            let src = flight[0]
            let dst = flight[1]
            let price = flight[2]

            if prices[src] == Int.max {
                continue
            }

            if prices[src] + price < tmpPrices[dst] {
                tmpPrices[dst] = prices[src] + price
            }
        }

        prices = tmpPrices
    }

    if prices[dst] == Int.max {
        return -1
    }

    return prices[dst]
}
```

#### Explanation

To solve this problem, we can use the Bellman-Ford algorithm, as it can calculate the shortest path and incorporate the idea of `k` stops.

Let's take a closer look at how the Bellman-Ford algorithm works:
![alt image](images/787-1.png#center)

* We are going to start from the `src` node
* Next, we are going to execute the Breadth First Search (BFS) algorithm on the `src` node
* Next, for each node that we have visited or can visit, we are going to keep track of the **minimum price** that it takes to reach that node within `k` stops

We are going to have an array that will help us track the price to reach each node.
![alt image](images/787-2.png#center)

* It will take us `0` cost to reach node `A`, and for nodes `B` and `C` we will put `infinity` and calculate as we go through the BFS algorithm.

![alt image](images/787-3.png#center)
Let's look at the first edge that connects nodes `A` and `B`.
The cost to reach node `B` equals the cost to reach node `A` plus the weight of the edge; as a result, it equals `0 + 100 = 100`. After that, we are going to put the calculated cost into the **temporary** prices array, and when it's filled with all prices, it will be used to update the **original** prices array.

> We are going to use a **temporary** array because it is going to help us avoid using an extra node in our path. We are not allowed to do so because we can only make `k` stops.

![alt image](images/787-4.png#center)
Next, let's look at the edge that connects nodes `A` and `C`.
The calculation for the cost to reach node `C` is the same as we did above — the cost to reach node `A` plus the edge weight `500`; as a result, `0 + 500 = 500`, which we will put into the temporary array.

![alt image](images/787-5.png#center)
Next, since we have completed the first layer of BFS, we are going to update our **original** prices array with **temporary** prices values and move to the second layer of BFS.

> We are always going to have `k + 1` layers in our BFS because the problem is defined in a way that we can only do `k` stops between the `src` and `dst` nodes; therefore, we can only have `1` stop between those two nodes.

![alt image](images/787-6.png#center)
Now, when we move to our second BFS layer, we are again going to go through every edge:

* We are going to visit the edge between nodes `A` and `B`, where we find that increasing stops between them is not going to reduce the cost; it will stay the same, therefore we **won't change the price in our temporary array**.
* The next edge is going to be between nodes `A` and `C`, and it will behave exactly the same as in the previous example and stay unchanged.
* Lastly, we are going to visit the edge between nodes `B` and `C` and take the cost that it takes us to visit node `B` plus the cost of the edge to `C`; as a result, `100 + 100 = 200`.

After the above steps are done, we are going to update our **original** prices array and return the result.

#### Time/ Space complexity

* Time complexity: O(n + (m * k))
* Space complexity: O(n)
* Where `n` is the number of cities, `m` is the number of flights, and `k` is the number of stops.

#### Thank you for reading! 😊
