+++
title = 'How to prevent memory leaks?'
date = 2023-12-24T00:00:00+03:00
tags = ["Swift", "Memory Leak"]
draft = false
+++

I was searching for tools that could help me find memory leaks faster and would be simple in implementation without affecting performance and memory size of application.

I found a fantastic fit for this task [`LifetimeTracker`](https://github.com/krzysztofzablocki/LifetimeTracker) developed by [Krzysztof Zabłocki](https://twitter.com/merowing_).

All you need is to add `LifetimeTracker` package to the project, inherit from `LifetimeTrackable` protocol, and add two lines of code.

``` swift
class Department: LifetimeTrackable {}
```

`trackLifetime` method to `init` of instance that you are going to verify, and `lifetimeConfiguration` property where you set max number of valid instances.

``` swift
class Department: LifetimeTrackable {

    static var lifetimeConfiguration = LifetimeConfiguration(maxCount: 1, groupName: "Department")

    let name: String

    init(name: String) {
        self.name = name
        print("\(Self.self) is being initialized")
        trackLifetime()
    }

    var employee: Employee?

    deinit {
        print("\(Self.self) is being deinitialized")
    }

}
```

The final step is to add `LifetimeTracker.setup` to `didFinishLaunchingWithOptions` to be able to see Dashboard with detected issues.

``` swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    #if DEBUG
      LifetimeTracker.setup(
        onUpdate: LifetimeTrackerDashboardIntegration(
        visibility: .alwaysVisible,
        style: .bar,
        textColorForNoIssues: .systemGreen,
        textColorForLeakDetected: .systemRed
        ).refreshUI
      )
    #else
    #endif
    return true
}
```

I hope this article will help you save time finding and debugging this tricky task :-).