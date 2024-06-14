+++
title = 'Adding Push Notifications to an iOS App'
date = 2024-06-14T07:29:01+03:00
tags = ["Adding", "Push Notifications", "iOS", "App"]
draft = false
+++

### Introduction
If you start a project from scratch, you need to always create some kind of service like `PushNotificationService` that will be responsible for handling push notification events. In this article, I want to explore a simple implementation of `PushNotificationService` to be able to reuse and customize it in future projects.

### First Step
The first step is to add the Push Notifications capability to your project. Go to your project -> Signing & Capabilities -> Tap `+ Capability` -> Search for Push Notifications.

![alt image](images/0.png#center)

### Second Step
The second step is to register push notifications.

```swift
func application(_: UIApplication, didFinishLaunchingWithOptions _: [UIApplication.LaunchOptionsKey: Any]? = nil) -> Bool {
    pushNotificationService.registerPushNotifications()
    return true
}
```

### Third Step
The third step is to add `didRegisterForRemoteNotificationsWithDeviceToken` and `didFailToRegisterForRemoteNotificationsWithError` to the `AppDelegate` file and pass the information that occurred to `PushNotificationService`.

```swift
func application(_: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    pushNotificationService.didRegisterForRemoteNotificationsWithDeviceToken(deviceToken: deviceToken)
}

func application(_: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
    pushNotificationService.didFailToRegisterForRemoteNotificationsWithError(error: error)
}
```

### Complete Example
``` swift 
import UIKit

class AppDelegate: NSObject, UIApplicationDelegate {
    private let pushNotificationService = PushNotificationService()

    func application(_: UIApplication, didFinishLaunchingWithOptions _: [UIApplication.LaunchOptionsKey: Any]? = nil) -> Bool {
        pushNotificationService.registerPushNotifications()
        return true
    }

    func application(_: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        pushNotificationService.didRegisterForRemoteNotificationsWithDeviceToken(deviceToken: deviceToken)
    }

    func application(_: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: any Error) {
        pushNotificationService.didFailToRegisterForRemoteNotificationsWithError(error: error)
    }
}
```

``` swift 
import UIKit

public final class PushNotificationService: NSObject {
    public func registerPushNotifications() {
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            granted, _ in
            guard granted else { return }
            DispatchQueue.main.async {
                UIApplication.shared.registerForRemoteNotifications()
            }
        }
    }

    public func didRegisterForRemoteNotificationsWithDeviceToken(deviceToken: Data) {
        let token = deviceToken.map { String(format: "%02.2hhx", $0) }.joined()
        print("Device token: \(token)")
    }

    public func didFailToRegisterForRemoteNotificationsWithError(error: any Error) {
        print("Failed to register for remote notifications: \(error)")
    }
}
```

#### Thank you for reading! 😊
