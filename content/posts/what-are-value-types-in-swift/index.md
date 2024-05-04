+++
title = 'What are value types in Swift?'
date = 2023-12-28T00:00:00+03:00
tags = ["Swift", "Value Type"]
draft = false
+++

### What are value types?
Value types play a central role in programming languages by grouping data values.
>`Value type” is a type of data copied when assigned to a new variable.

``` swift
struct Storage {
    var data: String = "some data"
}
let originalStorage = Storage()
var copiedStorage = originalStorage  // `originalStorage` is copied to `copiedStorage`
```

### How can you pass value types?
You can pass value type by copying value.
``` swift
struct Storage {
    var data: String = "some data"
}
let originalStorage = Storage()
var copiedStorage = originalStorage                        // `originalStorage` is copied to `copiedStorage`
copiedStorage.data = "new data"                            // Changes `copiedStorage`, not `originalStorage`
print("\(originalStorage.data), \(copiedStorage.data)")    // prints "some data, new data"
```

The effect of assignment, initialization, and argument passing creates an independent instance with a unique copy of its data.

### What types are value types?
Value types can be `struct`, `enum`, `tuple`.

### What data types are value types?
`Strings`, `Arrays`, `Dictionaries`, `Numbers`, `Booleans`, `Floating-point` numbers, and `Integers` are all value types.

### How value types are stored in memory?
The value types use `Stack` data structure for memory management.

### What is Copy-on-write mechanism?
The `copy-on-write` mechanism is a resource-management technique used to optimize value types performance. It improves performance by avoiding unnecessary copies of value types. If resource is duplicated but not modified it’s unnecessary to create new resource.

### When to use value types?
Choose value types if you don’t have shared mutable state.