+++
title = "LeetCode - Blind 75 - Course Schedule"
date = 2025-02-25T13:14:37+03:00
tags = ["LeetCode", "Blind 75", "Course Schedule", "Swift"]
draft = false
+++

### The problem  
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.  

* For example, the pair `[0, 1]` indicates that to take course `0`, you have to first take course `1`.  

Return `true` if you can finish all courses. Otherwise, return `false`.  

#### Examples  

```  
Input: numCourses = 2, prerequisites = [[1,0]]  
Output: true  
Explanation: There are a total of 2 courses to take.  
To take course 1, you should have finished course 0. So it is possible.  
```  

```  
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]  
Output: false  
Explanation: There are a total of 2 courses to take.  
To take course 1, you should have finished course 0, and to take course 0, you should also have finished course 1. So it is impossible.  
```  

#### Constraints  
* 1 <= numCourses <= 2000  
* 0 <= prerequisites.length <= 5000  
* prerequisites[i].length == 2  
* 0 <= ai, bi < numCourses  
* All the pairs `prerequisites[i]` are unique.  

### Depth First Search solution  

```swift  
func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {  
    var preMap: [Int: [Int]] = [:]  
    for i in 0 ..< numCourses {  
        preMap[i] = []  
    }  

    for prerequisite in prerequisites {  
        let crs = prerequisite[0]  
        let pre = prerequisite[1]  
        preMap[crs]!.append(pre)  
    }  

    var visit: Set<Int> = []  

    func dfs(_ crs: Int) -> Bool {  
        if visit.contains(crs) {  
            return false  
        }  

        if preMap[crs] == [] {  
            return true  
        }  

        visit.insert(crs)  

        for pre in preMap[crs]! {  
            if !dfs(pre) {  
                return false  
            }  
        }  

        visit.remove(crs)  
        preMap[crs] = []  

        return true  
    }  

    for crs in 0 ..< numCourses {  
        if !dfs(crs) {  
            return false  
        }  
    }  

    return true  
}  
```  

#### Explanation  
If we look at the example in the description and try to visualize it `0 -> 1`, we can see that we can represent the connection between nodes as an edge and recognize that this is a graph problem.  

In the example with input `numCourses = 2, prerequisites = [[1,0]]`, we can represent it as `0 <- 1`. This means that if we want to take course `1`, we need to finish course `0`.  

Now, let's look at an example that is impossible to complete:  
Input: `numCourses = 2, prerequisites = [[1,0], [0,1]]`.  

![alt image](images/p-207.png#center)  

The first pair means that to finish course `1`, we need to finish course `0` (`0 -> 1`).  
The second pair means that to finish course `0`, we need to finish course `1` first (`0 <- 1`).  

As you can see in the picture, we found a cycle, and neither of the courses is possible to finish.  

Let’s look at another example with input:  
`numCourses = 5, prerequisites = [[0,1], [0,2], [1,3], [1,4], [3,4]]`.  

![alt image](images/p-207-1.png#center)  

We can use an adjacency list to represent courses with a list of all their prerequisites and repeat this for every course using the DFS algorithm.  
If the list of prerequisites is empty, it means that it is possible to finish this course. In our case, this applies to `4` and `2`.  
Recursively, we will end up going back from `4` to `3`, and since we were able to complete prerequisite `4`, this means that we can complete prerequisite `3`.  
If this is the case, we can now remove prerequisite `3` and continue DFS by going back to `1`, etc.  

##### Time/space complexity  
* Time complexity: O(V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of courses and `E` is the number of prerequisites  

---

### Topological Sort (Kahn's Algorithm) solution  

```swift  
func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {  
    var indegree = Array(repeating: 0, count: numCourses)  
    var adj: [Int: [Int]] = [:]  
    for i in 0 ..< numCourses {  
        adj[i] = []  
    }  

    for value in prerequisites {  
        let src = value[0]  
        let dst = value[1]  
        indegree[dst] += 1  
        adj[src]!.append(dst)  
    }  

    var q: [Int] = []  
    for n in 0 ..< numCourses {  
        if indegree[n] == 0 {  
            q.append(n)  
        }  
    }  

    var finish = 0  

    while !q.isEmpty {  
        let node = q.removeFirst()  
        finish += 1  
        for nei in adj[node]! {  
            indegree[nei] -= 1  
            if indegree[nei] == 0 {  
                q.append(nei)  
            }  
        }  
    }  

    return finish == numCourses  
}  
```  

#### Explanation  
We can solve this problem in another way by using [Topological Sort (Kahn's Algorithm)](https://en.wikipedia.org/wiki/Topological_sorting#Kahn's_algorithm).  
Kahn’s algorithm helps us solve course scheduling puzzles.  

From the problem description, we find that we need to figure out the order in which we can take courses, considering which ones depend on others.  
Kahn’s algorithm checks if we can create a workable schedule without any cycles (like taking a course that requires another that you haven’t taken yet). It also arranges courses in the right order.  

##### Time/space complexity  
* Time complexity: O(V + E)  
* Space complexity: O(V + E)  
* Where `V` is the number of courses and `E` is the number of prerequisites  

#### Thank you for reading! 😊
