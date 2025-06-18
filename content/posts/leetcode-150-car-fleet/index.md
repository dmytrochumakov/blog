+++
title = "LeetCode - 150 - Car Fleet"
date = 2025-06-18T07:29:11+03:00
tags = ["LeetCode", "150", "Car Fleet", "Swift"]
draft = false
+++

### The problem

There are `n` cars at given miles away from the starting mile 0, traveling to reach the mile `target`.

You are given two integer array `position` and `speed`, both of length `n`, where `position[i]` is the starting mile of the `ith` car and `speed[i]` is the speed of the `ith` car in miles per hour.

A car cannot pass another car, but it can catch up and then travel next to it at the speed of the slower car.

A car fleet is a car or cars driving next to each other. The speed of the car fleet is the minimum speed of any car in the fleet.

If a car catches up to a car fleet at the mile `target`, it will still be considered as part of the car fleet.

Return the number of car fleets that will arrive at the destination.

#### Examples

```
Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
Output: 3
Explanation:
* The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting each other at 12. The fleet forms at target.
* The car starting at 0 (speed 1) does not catch up to any other car, so it is a fleet by itself.
* The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches target.
```

```
Input: target = 10, position = [3], speed = [3]
Output: 1
Explanation:
There is only one car, hence there is only one fleet.
```

```
Input: target = 100, position = [0,2,4], speed = [4,2,1]
Output: 1
Explanation:
* The cars starting at 0 (speed 4) and 2 (speed 2) become a fleet, meeting each other at 4. The car starting at 4 (speed 1) travels to 5.
* Then, the fleet at 4 (speed 2) and the car at position 5 (speed 1) become one fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches target.
```

#### Constraints

* n == position.length == speed.length
* 1 <= n <= 10^5
* 0 < target <= 10^6
* 0 <= position\[i] < target
* All the values of position are unique.
* 0 < speed\[i] <= 10^6

#### Explanation

Before we jump into the solution, let's draw a visual representation of two cars that are traveling with different speeds and different positions.
The first car is traveling with speed 10 miles per hour and it's first in the line. The second car is traveling with speed 20 miles per hour and it's second in the line.
![alt image](images/853.png#center)
The faster car will eventually catch up and will be travelling with the same speed as the first car because the road has a single line.
When the faster car catches up, its speed will be reduced to the speed of the slower one.
Once cars are traveling next to each other, that is called a car fleet and cars are assumed to have the same exact position.
![alt image](images/853-1.png#center)

### Stack Solution

```swift
func carFleet(_ target: Int, _ position: [Int], _ speed: [Int]) -> Int {
    let pair = zip(position, speed).sorted { $0.0 > $1.0 }
    var stack: [Double] = []

    for (p, s) in pair {
        stack.append(Double(target - p) / Double(s))

        if stack.count >= 2 && stack[stack.count - 1] <= stack[stack.count - 2] {
            stack.removeLast()
        }
    }

    return stack.count
}
```

#### Explanation

Let's look at another example with `[position, speed] = [[3, 3], [5, 2], [7, 1]]`
![alt image](images/853-2.png#center)

We start with position `3` and speed `3`:
- At one second, it will be at position `6`; 
- at two seconds, it will be at position `9`; 
- and at three seconds, it will be at position `12`.

Next, the position starts at `5` and speed `2`:
- At one second, it will be at position `7`; 
- at two seconds, it will be at position `9`; 
- and at three seconds, it will be at position `11`.

Lastly, we have position `7` and speed `1`:
- At one second, it will be at position `8`; 
- at two seconds, it will be at position `9`; 
- and at three seconds, it will be at position `10`.

You can see that lines intersect. It tells us that they are going to be a fleet. You can also see that the blue line intersects with the orange line before the green line does, meaning that it will be traveling with the same rate as the orange one.

Now when we know the idea, we can get into the solution to this problem.
![alt image](images/853-3.png#center)
One way to solve this problem is to calculate intersections, but more easier way would be to determine in what time cars are going to reach their destination.

- If the car with position `5` reaches the destination before or at the same time as the position `7`, this means that they become a car fleet.

We know at what speed each car is traveling, so we can calculate the time when they will reach the destination. 
- To do that, we will take our target, subtract the position, and divide it by the speed.

After calculation, you can see that cars at position `5` and `7` are going to collide.
Now we can delete a car. 
- We are going to remove the car that is not ahead, because when the cars collide, the speed is going to be reduced to the car that is ahead.
- We are going to look from right to left because, if we go from left to right, we do not know at what speed the blue car is going to travel, and we can't assume that the car is going to travel with the same speed and will not collide with somebody else and slow down.

We will be using a stack to solve this problem:

* Initially, our stack is going to be empty
* We are going to go through elements in reverse order
* We are going to add the first car to our stack
* Next, we are going to take the next car and add it to the top of the stack
* Then we are going to compare both cars to see if they collide with each other
* Then we are going to remove the car that is on top of the stack
* Next steps will be the same as above

#### Time/ Space complexity

* Time complexity: O(n\*logn)
* Space complexity: O(n)

#### Thank you for reading! 😊
