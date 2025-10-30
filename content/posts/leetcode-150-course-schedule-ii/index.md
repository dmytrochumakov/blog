+++
title = "LeetCode - 150 - Course Schedule II"
date = 2025-10-30T07:52:44+03:00
tags = ["LeetCode", "150", "Course Schedule II", "Swift"]
draft = false
+++

### The problem

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

* For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

#### Examples

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

```
Input: numCourses = 1, prerequisites = []
Output: [0]
```

#### Constraints

* 1 <= numCourses <= 2000
* 0 <= prerequisites.length <= numCourses * (numCourses - 1)
* prerequisites[i].length == 2
* 0 <= ai, bi < numCourses
* ai != bi
* All the pairs [ai, bi] are distinct.

#### Explanation

From the description of the problem, we learn that we are given `n` courses that are labeled from `0` to `n - 1`. We also know that some courses have `prerequisites`, and we want to return the order of courses that we need to take in order to finish all the courses. If it’s not possible, we will return an `empty` array.

Let’s look at the first example
![alt image](images/210.png#center)
We are given `two` courses `0` and `1` and `prerequisites = [[1, 0]]`.

* In order to take course `1`, we have to take course `0` first.
  As a result, we have the array `[0, 1]`.
  You can see that we can represent our `prerequisites` as a graph; therefore, we will be creating our solution around it.

Now, let’s look at another example when we have `two prerequisites` - `[[1, 0], [0, 1]]`
![alt image](images/210-1.png#center)

* The first prerequisite tells us that in order to take course `1`, we need to take course `0`.
* The second prerequisite tells us that in order to take course `0`, we need to take course `1`.
  You can see that we can’t take those courses because there is no valid order, and our prerequisites form a `cycle`. So as a result, we will return an `empty` array.

### Topological Sort Solution

```swift
func findOrder(_ numCourses: Int, _ prerequisites: [[Int]]) -> [Int] {
    var prereq: [Int: [Int]] = [:]
    for element in prerequisites {
        let crs = element[0]
        let pre = element[1]
        prereq[crs, default: []].append(pre)
    }

    var output: [Int] = []
    var visited: Set<Int> = []
    var cycle: Set<Int> = []

    func dfs(_ crs: Int) -> Bool {
        if cycle.contains(crs) {
            return false
        }
        if visited.contains(crs) {
            return true
        }

        cycle.insert(crs)

        for pre in prereq[crs, default: []) {
            if dfs(pre) == false {
                return false
            }
        }

        cycle.remove(crs)
        visited.insert(crs)
        output.append(crs)

        return true
    }

    for c in 0 ..< numCourses {
        if dfs(c) == false {
            return []
        }
    }

    return output
}
```

#### Explanation

We are going to solve this problem by using the topological sort algorithm.
Let’s look at the example:
![alt image](images/210-2.png#center)

* The first step that we are going to do is to build an adjacency list that will tell us what prerequisites we have for each node.
  Next, we are going to build the output using the depth-first search algorithm, visiting every single node:
  ![alt image](images/210-3.png#center)
* We are going to start our DFS from node `0` and visit our prerequisite `1` that we have in our adjacency list.
* Next, we are going to visit the prerequisite of `1`, which is the single node `3`.
* Next, we are going to visit the prerequisite of `3`, which is node `2` that has no prerequisites. Since node `2` has no prerequisites, we are allowed to take course `2`, and now we can add it to our output. We are also going to mark node `2` as visited so we don’t end up visiting it twice.
  ![alt image](images/210-4.png#center)
* Next, in our DFS, we are going to go back to node `3`, and since it does not have more prerequisites, we are going to mark it as visited and add it to our output.
* Next, we are going back to node `1`, and you can see that it does not have more prerequisites; therefore, we are going to mark it as visited and add it to our output.
  ![alt image](images/210-5.png#center)
* Next, we are going to node `0`. Node `0` has prerequisite `1`, which we already visited, and it also has a prerequisite with node `2` that we can ignore because we have already visited it earlier in our DFS. After that, we can mark node `0` as visited and add it to our result.
  ![alt image](images/210-6.png#center)
* Next, we are going to visit node `4`, mark it as visited, and add it to our output since it only has one prerequisite `0` that we already visited.
* Lastly, we are going to do the same for node `5`, mark it as visited, and add it to our output.

> In case we detect a cycle, we will return an empty array.

Let’s look at an example with a cycle
![alt image](images/210-7.png#center)
We are going to follow the same steps as we did above, except now we have node `2` that points to node `0` and forms a cycle:

* We are going to start at node `0`, next we are going to go to `1`, then we are going to go to `3`, then to node `2`, and lastly, we are going to go to node `0`. This will tell us that we found a cycle, and we are going to stop our algorithm and return `[]`.

To detect a cycle, we will be using a hash set where we will store the current path, and it will help us recognize the cycle.

#### Time/Space complexity

* Time complexity: O(V + E)
* Space complexity: O(V + E)
* Where `V` is the number of courses and `E` is the number of prerequisites

#### Thank you for reading! 😊
