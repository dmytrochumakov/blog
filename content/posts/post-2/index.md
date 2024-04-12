+++
title = 'Building Xcode project using the CLI'
date = 2024-04-05T04:59:38+03:00
tags = ["tools", "CLI"]
draft = false
+++

### Introduction
When you are working on different projects  sometimes  you need to use different  IDE’s. You need to find a way to build a project in the fastest way. 

### One of such ways is by using the `xcodebuild` command
#### Basic outline of the process
- **Open Terminal**: Open the Terminal application on your Mac.
- **Navigate to Project Directory**: Use the cd command to navigate to the directory containing your Xcode project.
- **Run** `xcodebuild`: Once you're in the project directory, you can run `xcodebuild` with the appropriate parameters to build your project. 

#### Example:
``` 
xcodebuild -project YourProjectName.xcodeproj -scheme YourSchemeName
```

### Another way is by integrating [fastlane](https://fastlane.tools/) into your workflow:
#### Outline of the process
**Install Fastlane**: If you haven't already installed Fastlane, you can do so using RubyGems, which is the Ruby package manager:

```
brew install fastlane
```

**Navigate to Project Directory**: Open Terminal and navigate to the directory containing your Xcode project.
Initialize Fastlane (Optional): If you haven't initialized Fastlane in your project yet, you can do so by running:

```
fastlane init
```

**Build with Fastlane**: Once Fastlane is set up, you can use it to build your Xcode project. Fastlane provides a lane named build_app for building your app. You can run this lane with the following command:
```
fastlane build_app
```

### Building a project through a project generation tools
If you are building a project through a project generation tool like [tuist](https://github.com/tuist/tuist) you do not need anything that was mentioned above because it already has build-in commands `tuist build`.
