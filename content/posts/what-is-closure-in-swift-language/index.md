+++
title = 'What is closure in Swift language?'
date = 2023-12-10T00:00:00+03:00
tags = ["Swift", "Closure"]
draft = false
+++

### Introduction
In this article, I’m going to briefly explain what closure is.

> Closures is self-conitained blocks of funcionality that can be passed around and used in your code.
— Apple

Expression:
``` swift
{ (params) -> return value in
  statements
}
```

### @escaping
When closure is marked as escaping, it will outlive or leave the scope you passed.
``` swift
func response(_ completionHandler: @escaping(Result) -> Void) {
   completionHandler(.success)
}
```

### @nonescaping
By default, closures are nonescaping, meaning closure will no longer exist in memory after complete execution in the scope you have passed it to.

``` swift
func filterImage(_ completionHandler: (Image) -> Void) {
   completionHandler(UIImage.filtered)
}
```

### @autoclosure
Autoclosures automatically create closure from the argument that you passed into a function.
``` swift
func animate(_ animation: @autoclosure @escaping () -> Void,
             duration: TimeInterval = 0.25) {
    UIView.animate(withDuration: duration, animations: animation)
}
```

Closure conceptualy looks like this first pointer points to the code that implements closure the second pointer pointed to the reference counted object.
``` swift
struct Closure {
    var functionPointer: UnsafeRawPointer
    var closureContext: AnyObject?
}
```