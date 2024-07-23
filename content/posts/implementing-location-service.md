+++
title = 'Implementing Location Service'
date = 2024-07-23T07:04:25+03:00
tags = ["Implementing", "Location Service", "Swift", "CoreLocation"]
draft = false
+++

### Introduction 

Nowadays, location is an essential feature in almost every application. It's very important to know the best ways to implement it without affecting performance and user experience. In this article, I will focus on how to implement general methods in location service.

### Preparation

Before we begin, let's add location permission keys to `Info.plist`:
- `Privacy - Location When In Use Usage Description`
- `Privacy - Location Always and When In Use Usage Description`

### First Step

The first step is to create `LocationService` with the `requestPermissions` method to be able to receive location events.

```swift
private let locationManager: CLLocationManager

override init() {
    self.locationManager = CLLocationManager()
    super.init()
    requestPermissions()        
}

func requestPermissions() {
    locationManager.requestWhenInUseAuthorization()
}
```

> 💡 Use `requestWhenInUseAuthorization` only if you need location updates when the user is using your app.

### Second Step

The second step is to add `locationManager.delegate` to be able to handle location updates or errors.

```swift
// MARK: - CLLocationManagerDelegate
extension LocationService: CLLocationManagerDelegate {

    func locationManager(
        _ manager: CLLocationManager,
        didUpdateLocations locations: [CLLocation]
    ) {
        guard let location = locations.first else { return }
        let latitude = location.coordinate.latitude
        let longitude = location.coordinate.longitude

        print("Location: \(latitude), \(longitude)")
    }

    func locationManager(
        _ manager: CLLocationManager,
        didFailWithError error: Error
    ) {
        print("Error: \(error)")
    }
}
```

> 💡 [`CLLocationManager`](https://developer.apple.com/documentation/corelocation/cllocationmanager) - The object you use to start and stop the delivery of location-related events to your app.

> 💡 [`CLLocationManagerDelegate`](https://developer.apple.com/documentation/corelocation/cllocationmanagerdelegate) - The methods you use to receive events from an associated location-manager object.

### Authorization Status

Sometimes you need to know about authorization status and create logic around it. In this case, you can check the status by using the `authorizationStatus` property.

```swift
func authorizationStatus() {
    switch locationManager.authorizationStatus {
    case .notDetermined:
        print("Not determined")
    case .restricted:
        print("Restricted")
    case .denied:
        print("Denied")
    case .authorizedAlways:
        print("Authorized always")
    case .authorizedWhenInUse:
        print("Authorized when in use")
    @unknown default:
        print("Unknown")
    }
}
```

### One-Time Location Update

In case you want to ask the user for location only once, you can call the `locationManager.requestLocation()` method.

```swift
func requestLocationOnce() {
    locationManager.requestLocation()
}
```

### Real-Time Location Updates

In case you need to get real-time location updates, you can use the `startUpdatingLocation` and `stopUpdatingLocation` methods.

```swift
func requestRealTimeLocationUpdates() {
    locationManager.startUpdatingLocation()
    DispatchQueue.main.asyncAfter(deadline: .now() + 10) {
        self.locationManager.stopUpdatingLocation()
    }
}
```

> ⚠️ Do not forget to call the `stopUpdatingLocation` method, as it can cause performance issues.

### Complete Example

```swift
import CoreLocation

final class LocationService: NSObject {
    private let locationManager: CLLocationManager

    override init() {
        locationManager = CLLocationManager()
        super.init()
        locationManager.delegate = self
        requestPermissions()
    }

    func requestPermissions() {
        locationManager.requestWhenInUseAuthorization()
    }

    func requestLocationOnce() {
        locationManager.requestLocation()
    }

    func requestRealTimeULocationUpdates() {
        locationManager.startUpdatingLocation()
        DispatchQueue.main.asyncAfter(deadline: .now() + 10) {
            self.locationManager.stopUpdatingLocation()
        }
    }

    func authorizationStatus() {
        switch locationManager.authorizationStatus {
        case .notDetermined:
            print("Not determined")
        case .restricted:
            print("Restricted")
        case .denied:
            print("Denied")
        case .authorizedAlways:
            print("Authorized always")
        case .authorizedWhenInUse:
            print("Authorized when in use")
        @unknown default:
            print("Unknown")
        }
    }
}

// MARK: - CLLocationManagerDelegate

extension LocationService: CLLocationManagerDelegate {
    func locationManager(
        _: CLLocationManager,
        didUpdateLocations locations: [CLLocation]
    ) {
        guard let location = locations.first else { return }
        let lattitude = location.coordinate.latitude
        let longitude = location.coordinate.longitude

        print("Location: \(lattitude), \(longitude)")
    }

    func locationManager(
        _: CLLocationManager,
        didFailWithError error: Error
    ) {
        print("Error: \(error)")
    }
}

```

#### Thank you for reading! 😊
