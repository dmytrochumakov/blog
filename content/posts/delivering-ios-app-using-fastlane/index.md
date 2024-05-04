+++
title = 'Delivering iOS app using Fastlane'
date = 2024-04-19T06:49:13+03:00
tags = ["CLI", "Tools", "Delivering", "Fastlane"]
draft = false
+++

### Introduction
You can easily deliver an iOS app in two ways: through `beta` and `release` lanes.

### The First Way - TestFlight
By creating a `beta` `lane` inside the `Fastfile`. It utilizes:

- `build_app`: To easily build and sign your app (via `gym`).
- `pilot`: Makes it easier to manage your app on Apple’s `TestFlight`.

``` ruby
lane :beta do
  build_app(scheme: "YourScheme")
  pilot
end
```

To run Fastlane:
``` bash
fastlane beta
```

### Attention
Before proceeding, you need to have the `ipa` or `pkg` file generated.

### The Second Way - App Store Connect
By creating a `release` `lane` inside the `Fastfile`. It utilizes:

- `gym`: To build and package iOS apps for you.
- `deliver`: To upload screenshots, metadata, and binaries to App Store Connect.

``` ruby
lane :release do
  gym # Builds the app
  deliver # Uploads the app to App Store Connect
end
``` 

To run Fastlane:
``` bash
fastlane release
```

### If you haven't installed Fastlane yet, here are the steps:

Outline of the Process
- **Install Fastlane**: You can do so using `RubyGems`, which is the `Ruby` package manager:
``` bash
brew install fastlane
```
- **Navigate to Project Directory**: Open `Terminal` and navigate to the directory containing your `Xcode` project.
- **Initialize Fastlane (Optional)**: If you haven't initialized `Fastlane` in your project yet, you can do so by running:
``` bash
fastlane init
```