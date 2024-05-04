+++
title = 'DispatchGroup in Swift'
date = 2024-01-10T00:00:00+03:00
tags = ["Swift", "Concurrency", "Dispatchgroup"]
draft = false
+++

### What is DispatchGroup?
DispatchGroup provides a mechanism to track the completion group of tasks.

### How DispatchGroup works?
DispatchGroup has three main methods, `enter`, `leave` and `notify`, that allow you to control the completion of a specific task.

``` swift
let dispatchGroup = DispatchGroup()
dispatchGroup.enter()
dispatchGroup.leave()
dispatchGroup.notify(queue: .main) {}
```

### Let`s talk about each of these methods.

`enter` — manually indicate a block has entered group.
`leave` — manually indicate a block in the group has been completed.
`notify(queue: )` — schedule a block to be submitted to a queue when all the blocks associated with a group have been completed. The `queue` parameter is the queue to which the supplied block will be submitted when the group is complete.

### How to implement DispatchGroup?
You can implement DispatchGroup following these steps:

``` swift 
let dispatchGroup = DispatchGroup()

dispatchGroup.enter()
fetchGitHubUser1 {
    print("fetchGitHubUser1 task completed")
    dispatchGroup.leave()
}

dispatchGroup.enter()
fetchGitHubUser2 {
    print("fetchGitHubUser2 task completed")
    dispatchGroup.leave()
}

dispatchGroup.notify(queue: .main) {
    print("All tasks completed")
}

// prints
fetchGitHubUser1 task started
fetchGitHubUser2 task started
fetchGitHubUser1 task completed
fetchGitHubUser2 task completed
All tasks completed
```

### Pros
You can create a group of tasks and track when all tasks finish their work.
You can specify a queue where you want to be notified about completed operations.

### Cons
You should manually manage `enter` and `leave` operations that increase complexity and the chance of error.
You can accidentally forget to write the `leave` operation, which can cause unpredictable behavior.
