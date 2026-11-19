+++
title = "LeetCode - Design a Food Rating System"
date = 2026-11-19T19:38:25+03:00
tags = ["LeetCode", "Design a Food Rating System", "Arrays_&_Hashing", "Heap", "Medium", "Swift"]
draft = false
+++

### The problem

Design a food rating system that can do the following:

- **Modify** the rating of a food item listed in the system.
- Return the highest-rated food item for a type of cuisine in the system.

Implement the `FoodRatings` class:

- `FoodRatings(String[] foods, String[] cuisines, int[] ratings)` Initializes the system. The food items are described by `foods`, `cuisines` and `ratings`, all of which have a length of `n`.


  - `foods[i]` is the name of the `ith` food,
  - `cuisines[i]` is the type of cuisine of the `ith` food, and
  - `ratings[i]` is the initial rating of the `ith` food.
- `void changeRating(String food, int newRating)` Changes the rating of the food item with the name `food`.
- `String highestRated(String cuisine)` Returns the name of the food item that has the highest rating for the given type of `cuisine`. If there is a tie, return the item with the **lexicographically smaller** name.

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before `y` in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

**Example 1:**

```
Input
["FoodRatings", "highestRated", "highestRated", "changeRating", "highestRated", "changeRating", "highestRated"]
[[["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]], ["korean"], ["japanese"], ["sushi", 16], ["japanese"], ["ramen", 16], ["japanese"]]
Output
[null, "kimchi", "ramen", null, "sushi", null, "ramen"]

Explanation
FoodRatings foodRatings = new FoodRatings(["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]);
foodRatings.highestRated("korean"); // return "kimchi"
                                    // "kimchi" is the highest rated korean food with a rating of 9.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // "ramen" is the highest rated japanese food with a rating of 14.
foodRatings.changeRating("sushi", 16); // "sushi" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "sushi"
                                      // "sushi" is the highest rated japanese food with a rating of 16.
foodRatings.changeRating("ramen", 16); // "ramen" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // Both "sushi" and "ramen" have a rating of 16.
                                      // However, "ramen" is lexicographically smaller than "sushi".

```

**Constraints:**

- `1 <= n <= 2 * 10^4`
- `n == foods.length == cuisines.length == ratings.length`
- `1 <= foods[i].length, cuisines[i].length <= 10`
- `foods[i]`, `cuisines[i]` consist of lowercase English letters.
- `1 <= ratings[i] <= 10^8`
- All the strings in `foods` are **distinct**.
- `food` will be the name of a food item in the system across all calls to `changeRating`.
- `cuisine` will be a type of cuisine of **at least one** food item in the system across all calls to `highestRated`.
- At most `2 * 10^4` calls **in total** will be made to `changeRating` and `highestRated`.

#### Explanation

This problem is more like an exercise in how you structure data, so there are quite a few things you need to consider to make it work.

Our objective is to structure data in such a way that it runs blazingly fast. Therefore, we need to use the fastest lookup data structure: **hashmap**.

We could try to use an array, but it would be very slow because searching would take **O(n)** time on each function call.

We can divide this problem into two parts:
- The first part, where we get constant-time lookup for finding food or cuisine.
- The second part, where we find food with the highest rating.

The first part is easy - we need to map **food to rating**, **food to cuisine**, and **cuisine to food with rating**, and **manage the rating update** case.

We need all three hashmaps because using just one will not be enough as we cannot store and access data efficiently; eventually we will end up using an array that will cost us O(n) in search.

The second part is more complex, as we need to find the **highest rating** efficiently, and using an array is not going to cut it.

So we can use **Max Heap** and get the **highest rating in O(logn)** time. But for this to work, we need to use **lazy deletion**.

We need it because there is no efficient way to delete from a heap, because after deletion, it would need to update its underlying order, which costs O(n) time.

Lazy deletion helps because it marks old records as ready to delete and deletes them only if marked elements exist and it is on **top** of the heap, so the algorithm eliminates O(n) heap restructuring and runs in O(logn) time.

#### Code
```swift
class FoodRatings {
    
    struct Food: Comparable {
        let rating: Int
        let food: String
        
        static func < (lhs: Food, rhs: Food) -> Bool {
            if lhs.rating == rhs.rating {
                return lhs.food > rhs.food
            }
            return lhs.rating < rhs.rating
        }
    }
    
    struct ElementToRemove: Hashable {
        let rating: Int
        let food: String
    }
    
    private var foodToCuisine: [String: String]
    private var foodToRating: [String: Int]
    private var cuisineToFood: [String: Heap<Food>]
    private var elementsToRemove: Set<ElementToRemove>
    
    init(_ foods: [String], _ cuisines: [String], _ ratings: [Int]) {
        self.foodToCuisine = [:]
        self.foodToRating = [:]
        self.cuisineToFood = [:]
        self.elementsToRemove = []
        
        for i in 0 ..< foods.count {
            let food = foods[i]
            let cuisine = cuisines[i]
            let rating = ratings[i]
            
            self.foodToCuisine[food] = cuisine
            self.foodToRating[food] = rating
            self.cuisineToFood[cuisine, default: Heap()].insert(Food(rating: rating, food: food))
        }
    }
    
    func changeRating(_ food: String, _ newRating: Int) {
        let oldRating = self.foodToRating[food, default: 0]
        self.elementsToRemove.insert(ElementToRemove(rating: oldRating, food: food))
        self.foodToRating[food] = newRating
        self.cuisineToFood[self.foodToCuisine[food, default: ""]]?.insert(Food(rating: newRating, food: food))
    }
    
    func highestRated(_ cuisine: String) -> String {
        guard var heap = self.cuisineToFood[cuisine] else { return "" }
        
        while let food = heap.max {
            if food.rating != self.foodToRating[food.food] {
                heap.removeMax()
            } else {
                self.cuisineToFood[cuisine] = heap
                return food.food
            }
        }
        
        return ""
    }
    
}
```

#### Time/ Space complexity
* Time complexity: O(nlogn + mlogn) - nlogn time comes from insertion of food in init (each insertion costs O(logn) mutiplied by number of foods)
* Space complexity: O(n + m)
* Where `n` is total number of foods and `m` is the total number of calls to `changeRating` and `highestRated` functions

#### Thank you for reading! 😊  
