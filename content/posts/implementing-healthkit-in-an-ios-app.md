+++
title = 'Implementing HealthKit in an iOS App'
date = 2024-07-26T07:18:58+03:00
tags = ["Implementing", "HealthKit", "iOS", "App", "Swift", "SwiftUI", "Modern Concurrency"]
draft = false
+++

### Introduction
Previously, I worked with a healthcare app that used the HealthKit framework, but I did not get the opportunity to implement it myself. I decided to look into it and share what I found. In this article, I will focus on the steps to integrate HealthKit, write, and access its data.

### Preparation
Before we dive into implementation, I assume that you have an active Apple Developer account; without it, you will not be able to access the HealthKit Store.
Let’s add:
- `HealthKit Capability` to the project
- Privacy permission `Privacy – Health Share Usage Description`, `Privacy – Health Update Usage Description` keys to `Info.plist`. 

### Implementation

#### First Step
The first step is to call the `isHealthDataAvailable` method and make sure that it is available.
```swift 
guard HKHealthStore.isHealthDataAvailable() else {  
    throw HealthkitSetupError.notAvailableOnDevice
}
``` 

#### Data Type Preparation
Next, you need to prepare the data types that you will be reading and writing from the HealthKit central repository.
```swift 
guard let height = HKObjectType.quantityType(forIdentifier: .height),
      let bodyMass = HKObjectType.quantityType(forIdentifier: .bodyMass),
      let activeEnergyBurned = HKObjectType.quantityType(forIdentifier: .activeEnergyBurned)
else {
    throw HealthkitSetupError.dataTypeNotAvailable
}

let typesToWrite: Set<HKSampleType> = [height, bodyMass, HKObjectType.workoutType()]
let typesToRead: Set<HKObjectType> = [height, bodyMass, activeEnergyBurned, HKObjectType.workoutType()]
```

#### Request Authorization
Next, you need to request authorization with the data types that you defined above.
```swift 
try await HKHealthStore().requestAuthorization(toShare: typesToWrite, read: typesToRead)
``` 

```swift 
import Inject
import SwiftUI

public struct ContentView: View {
    @ObserveInjection var inject
    @State var healthKitService: HealthKitService = .init()
    public init() {}

    public var body: some View {
        VStack {}.onAppear {
            Task {
                do {
                    try await healthKitService.requestAuthorization()
                    print("HealthKit authorization request success")
                } catch {
                    print("HealthKit authorization request failed error: \(error)")
                }
            }
        }
        .enableInjection()
    }
}
``` 

### Reading/Saving Data

#### Reading Data
When you need to read/save data using HealthKit, you can do it by applying the `HKHealthStore` object.
> :bulb: [`HKHealthStore`](https://developer.apple.com/documentation/healthkit/hkhealthstore) - The access point for all data managed by HealthKit.

As an example, you can read characteristic data by utilizing HKHealthStore-defined methods such as `biologicalSex`, `bloodType`, `dateOfBirthComponents`, etc.
 
```swift 
func getProfileData() throws -> ProfileData {
    let healthStore = HKHealthStore()

    let dateOfBirthComponents = try healthStore.dateOfBirthComponents()
    let today = Date()
    let calendar = Calendar.current
    let todayDateComponents = calendar.dateComponents([.year], from: today)
    let thisYear = todayDateComponents.year!
    let age = thisYear - dateOfBirthComponents.year!

    let biologicalSex = try healthStore.biologicalSex()
    let bloodType = try healthStore.bloodType()

    let profileData = ProfileData(age: age, biologicalSex: biologicalSex.biologicalSex, bloodType: bloodType.bloodType)
    return profileData
}

struct ProfileData {
    var age: Int?
    var biologicalSex: HKBiologicalSex?
    var bloodType: HKBloodType?
}
``` 

#### Saving Data
In the case of saving data, you need to specify the quantity type, quantity, and create a sample.
```swift 
func saveHeight(height: Double, date: Date) async throws {
    let quantityType = HKQuantityType.quantityType(forIdentifier: .height)!
    let quantity = HKQuantity(unit: .meter(), doubleValue: height)
    let sample = HKQuantitySample(type: quantityType, quantity: quantity, start: date, end: date)
    try await HKHealthStore().save(sample)
}
``` 

> :bulb: [`HKQuantityType`](https://developer.apple.com/documentation/healthkit/HKQuantityType) - A type that identifies samples that store numerical values.

> :bulb: [`HKQuantity`](https://developer.apple.com/documentation/healthkit/HKQuantity) - An object that stores a value for a given unit.

> :bulb: [`HKQuantitySample`](https://developer.apple.com/documentation/healthkit/HKQuantitySample) - A sample that represents a quantity, including the value and the units.

### Complete Example
``` swift 
import HealthKit

enum HealthkitSetupError: Error {
    case notAvailableOnDevice
    case dataTypeNotAvailable
}

final class HealthKitService {
    func requestAuthorization() async throws {
        guard HKHealthStore.isHealthDataAvailable() else {
            throw HealthkitSetupError.notAvailableOnDevice
        }
        guard let height = HKObjectType.quantityType(forIdentifier: .height),
              let bodyMass = HKObjectType.quantityType(forIdentifier: .bodyMass),
              let activeEnergyBurned = HKObjectType.quantityType(forIdentifier: .activeEnergyBurned),
              let dateOfBirth = HKObjectType.characteristicType(forIdentifier: .dateOfBirth),
              let bloodType = HKObjectType.characteristicType(forIdentifier: .bloodType),
              let biologicalSex = HKObjectType.characteristicType(forIdentifier: .biologicalSex)
        else {
            throw HealthkitSetupError.dataTypeNotAvailable
        }
        let typesToWrite: Set<HKSampleType> = [height, bodyMass, HKObjectType.workoutType()]
        let typesToRead: Set<HKObjectType> = [height, bodyMass, activeEnergyBurned, HKObjectType.workoutType(), dateOfBirth, bloodType, biologicalSex]

        try await HKHealthStore().requestAuthorization(toShare: typesToWrite, read: typesToRead)
    }

    func getProfileData() throws -> ProfileData {
        let healthStore = HKHealthStore()

        let dateOfBirthComponents = try healthStore.dateOfBirthComponents()
        let today = Date()
        let calendar = Calendar.current
        let todayDateComponents = calendar.dateComponents([.year],
                                                          from: today)
        let thisYear = todayDateComponents.year!
        let age = thisYear - dateOfBirthComponents.year!

        let biologicalSex = try healthStore.biologicalSex()
        let bloodType = try healthStore.bloodType()

        let profileData = ProfileData(age: age, biologicalSex: biologicalSex.biologicalSex, bloodType: bloodType.bloodType)
        return profileData
    }

    func saveHeight(height: Double, date: Date) async throws {
        let quantityType = HKQuantityType.quantityType(forIdentifier: .height)!
        let quantity = HKQuantity(unit: .meter(), doubleValue: height)
        let sample = HKQuantitySample(type: quantityType, quantity: quantity, start: date, end: date)
        try await HKHealthStore().save(sample)
    }
}

struct ProfileData {
    var age: Int?
    var biologicalSex: HKBiologicalSex?
    var bloodType: HKBloodType?
}

``` 

#### Thank you for reading! 😊
