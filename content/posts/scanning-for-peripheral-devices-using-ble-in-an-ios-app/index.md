+++
title = 'Scanning for peripheral devices using BLE in an iOS app'
date = 2024-07-10T07:27:14+03:00
tags = ["Scanning", "peripheral", "devices", "BLE", "iOS", "app", "Swift"]
draft = false
+++

### Introduction
I had the chance to work on a project where communication via BLE was at the heart of the project.

Before adding any code to application, I always asked myself about two scenarios:
- The first scenario is when the device acts as a central device while searching for and connecting to peripheral devices.
- The second scenario is when the device acts as a peripheral device by using `CBCharacteristic` and changes its value.

In this article, I will focus on the first scenario and will show how to scan for peripheral devices.

### First Step
The first step is to add permission for Bluetooth: `Privacy - Bluetooth Always Usage Description`
![alt image](images/0.png#center)

### Second Step
The second step is to initialize `centralManager`:
```swift
private var centralManager: CBCentralManager!
    
override init() {
    super.init()
    centralManager = CBCentralManager(delegate: self, queue: nil, options: [CBCentralManagerOptionShowPowerAlertKey: true])
}
```

> [`CBCentralManager`](https://developer.apple.com/documentation/corebluetooth/cbcentralmanager/) - CBCentralManager objects manage discovered or connected remote peripheral devices (represented by CBPeripheral objects), including scanning for, discovering, and connecting to advertising peripherals.

> `CBCentralManagerDelegate` - The single required `centralManagerDidUpdateState` method indicates the availability of the central manager, while the optional methods allow for the discovery and connection of peripherals.

> `CBCentralManagerOptionShowPowerAlertKey` - An NSNumber (Boolean) indicating that the system should, if Bluetooth is powered off when `CBCentralManager` is instantiated, display a warning dialog to the user.

### Third Step
The third step is to `scanForPeripherals` after `centralManager` changes its state to `poweredOn`.
```swift
func centralManagerDidUpdateState(_ central: CBCentralManager) {
    switch central.state {
    case .poweredOn:
        print("CBManager is powered on")
        centralManager.scanForPeripherals(withServices: nil)
    case .poweredOff:
        print("CBManager is not powered on")
        return
    case .resetting:
        print("CBManager is resetting")
        return
    case .unauthorized:
        print("CBManager is unauthorized")
        return
    case .unknown:
        print("CBManager state is unknown")
        return
    case .unsupported:
        print("Bluetooth is not supported on this device")
        return
    @unknown default:
        print("A previously unknown central manager state occurred")
        return
    }
}
``` 

### Fourth Step
The fourth step is to implement the `didDiscover` method that is called when the central manager discovers a peripheral.
```swift
func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral, advertisementData: [String : Any], rssi RSSI: NSNumber) {
    // Reject if the signal strength is too low.
    // Change the minimum RSSI value depending on your app’s use case.
    guard RSSI.intValue >= -50 else {
        print("Discovered peripheral not in expected range, at \(RSSI.intValue)")
        return
    }

    print("Discovered \(String(describing: peripheral.name)) at \(RSSI.intValue)")
}
```

The sample implementation of this method uses the RSSI (Received Signal Strength Indicator) parameter to determine whether the signal is strong enough. RSSI values are provided as negative numbers, with a theoretical maximum of 0.

### Resources
https://developer.apple.com/documentation/corebluetooth/transferring-data-between-bluetooth-low-energy-devices

#### Thank you for reading! 😊
