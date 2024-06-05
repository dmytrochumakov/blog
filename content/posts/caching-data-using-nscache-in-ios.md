+++
title = 'Caching data using NSCache in iOS'
date = 2024-06-05T07:13:58+03:00
tags = ["Caching", "Caching data", "NSCache", "iOS"]
draft = false
+++

### Introduction
I was curious about caching data using [`NSCache`](https://developer.apple.com/documentation/foundation/nscache) for an iOS app. So, I did some digging. Here is what I found:

### Quick Overview
- `NSCache` helps store data in memory. When the application gets killed, it frees memory; it’s not persisted on disk.
- Storing data is carried out using a key-value pair mechanism like `Dictionary`.
- You can set automatic eviction to delete objects automatically.
- `NSCache` has multi-platform support: iOS, iPadOS, watchOS, macOS, and tvOS.

#### Caveats
`NSCache` has Objective-C roots. It can’t use `struct` because it is constrained to conform to `AnyObject`, meaning you must use `class` and `NSString` instead of `String`.

### Store Object
You can store an object by setting it in the cache:
```swift
func storeImage(_ image: UIImage, for key: String) {
    cache.setObject(image, forKey: key as NSString)
}
```

### Retrieve Object
You can retrieve an object by getting the object for the key:
```swift
func retrieveImage(for key: String) -> UIImage? {
    cache.object(forKey: key as NSString)
}
```

### Removing Object
You can remove an object by removing the object for the key, or remove all objects:
```swift
func removeImage(for key: String) {
    cache.removeObject(forKey: key as NSString)
}
``` 
```swift
func removeAllImages() {
    cache.removeAllObjects()
}
```

### Automatically Cache Cleaning
You can limit the number of objects in memory by setting `countLimit`. `countLimit` depends on the size of the object that you need to store in the cache. If it’s a large image, the limit can be less.
```swift
cache.countLimit = 5
```
Another way to do automatic cleaning is to set up `totalCostLimit`. `NSCache` will automatically delete objects until the total cost of the cache is under the `totalCostLimit`.
```swift
cache.totalCostLimit = 10 * 1024 * 1024 // 10 MB
```

#### Caveats
Even if you don’t set any deletion conditions, `NSCache` will automatically clean up when the system really needs memory.

### NSCacheDelegate
`cache(_:willEvictObject:)` notifies when an object is being removed. It helps in cases when you need to react to these changes.
```swift
extension CacheService: NSCacheDelegate {
    func cache(_ cache: NSCache<AnyObject, AnyObject>, willEvictObject obj: Any) {
        print("Object will be evicted: \(obj)")
    }
}
```

### Complete Sample
```swift
final class CacheService: NSObject {

    private let cache: NSCache<NSString, UIImage>

    override init() {
        cache = NSCache()
        cache.name = "Remote Image Cache"
        cache.countLimit = 5
        cache.totalCostLimit = 10 * 1024 * 1024 // 10 MB
    }

    func storeImage(_ image: UIImage, for key: String) {
        cache.setObject(image, forKey: key as NSString)
    }

    func retrieveImage(for key: String) -> UIImage? {
        cache.object(forKey: key as NSString)
    }

    func removeImage(for key: String) {
        cache.removeObject(forKey: key as NSString)
    }

    func removeAllImages() {
        cache.removeAllObjects()
    }

}

// MARK: - NSCacheDelegate
extension CacheService: NSCacheDelegate {

    func cache(_ cache: NSCache<AnyObject, AnyObject>, willEvictObject obj: Any) {
        print("Object will be evicted: \(obj)")
    }

}

```

### Resources
Thanks to [Andy Ibanez](https://www.andyibanez.com/posts/caching-content-with-nscache/) for his amazing straightforward explanation with examples. It helped a lot to quickly understand this topic.

#### Thank you for reading! 😊
