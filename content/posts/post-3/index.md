+++
title = 'Testing Xcode project using the CLI'
date = 2024-04-12T03:20:34+03:00
tags = ["tools", "CLI", "testing"]
draft = false
+++

### Introduction
When you are working on different projects  sometimes  you need to use different  IDE’s. You need to find a way to test a project in the fastest way. 

### One of such ways is by using the `xcodebuild` command
#### Basic outline of the process
- **Open Terminal**: Open the Terminal application on your Mac.
- **Navigate to Project Directory**: Use the cd command to navigate to the directory containing your Xcode project.
- **Run** `xcodebuild`: Once you're in the project directory, you can run `xcodebuild` with the appropriate parameters to build your project. 

#### Example:
``` bash
xcodebuild -project YourProject.xcodeproj -scheme YourSchemeName test
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

**Define a Lane for Testing**: Open your Fastfile and define a lane for running tests. Here's a basic example:

``` ruby
lane :run_tests do
  scan(scheme: "YourSchemeName")
end
```

**Run Tests Using Fastlane**: You can now run your tests using the lane you defined. In the terminal, navigate to your project directory and run:

``` bash
fastlane run_tests
```


### Testing a project through a project generation tools
If you are testing a project through a project generation tool like [tuist](https://github.com/tuist/tuist) you do not need anything that was mentioned above because it already has build-in commands: 
```
tuist test YourSchemeName
```
