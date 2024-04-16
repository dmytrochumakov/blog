---
title: 'Archiving Xcode project using the CLI'
date: 2024-04-16T13:46:03+03:00
tags: ["tools", "CLI", "archiving"]
draft: false
cover:
  image: "images/c_6.png"
---

### Introduction
When you are working on different projects  sometimes  you need to use different  IDE’s. You need to find a way to archive a project in the fastest way. 

### One of such ways is by using the `xcodebuild archive` command
#### Basic outline of the process
- **Open Terminal**: Open the Terminal application on your Mac.
- **Navigate to Project Directory**: Use the cd command to navigate to the directory containing your Xcode project.
- **Run** `xcodebuild archive`: Once you're in the project directory, you can run `xcodebuild archive` with the appropriate parameters to build your project. 

#### Example:
``` bash
xcodebuild archive -scheme YourSchemeName -archivePath ~/Desktop/YourAppName.xcarchive
```

### Another way is by integrating [fastlane](https://fastlane.tools/) into your workflow:
#### Outline of the process
**Install Fastlane**: If you haven't already installed Fastlane, you can do so using RubyGems, which is the Ruby package manager:

``` bash
brew install fastlane
```

**Navigate to Project Directory**: Open Terminal and navigate to the directory containing your Xcode project.
Initialize Fastlane (Optional): If you haven't initialized Fastlane in your project yet, you can do so by running:

```
fastlane init
```

**Create a lane for archiving**: Open your `Fastfile` located in the `fastlane` directory of your project, and add a new lane for archiving:

``` ruby
lane :archive do
  gym(
    scheme: "YourSchemeName",
    output_directory: "/path/to/your/archive/directory",
    output_name: "YourAppName"
  )
end
```

**Run the archive lane**: Once you've defined the archive lane, you can run it using the following command:

``` bash
fastlane archive
```