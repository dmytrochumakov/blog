+++
title = 'Building Xcode project using Github Actions'
date = 2024-04-23T08:29:30+03:00
tags = ["project", "building", "Github Actions", "CI"]
draft = false
+++

### Introduction
If you're wondering how to build an Xcode project using GitHub Actions, here are a few steps:

- First, you need to create a `.github/workflows` folder with a `CI.yml` file inside your project directory.
- Next, you need to add configuration to the `CI.yml` file.

```yml
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: macos-14

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up Xcode version
      run: sudo xcode-select -s /Applications/Xcode_15.3.app/Contents/Developer

    - name: Install xcpretty
      run: gem install xcpretty

    - name: Build project
      run: xcodebuild -project /Users/runner/work/YourProjectName/YourProjectName/YourProjectName/YourProjectName.xcodeproj -scheme YourSchemeName -sdk iphonesimulator -destination 'platform=iOS Simulator,name=iPhone 15 Pro' clean build | xcpretty
```

### Caveats
If you don't specify the path to the Xcode project, you will receive an error like this: `xcodebuild: error: ‘YourProjectName.xcodeproj' does not exist`.

You can debug project directory by adding this line to your config:
``` yml
    - name: Debug Directory Contents
      run: ls -la /Users/runner/work/YourProjectName/YourProjectName
```

Thank you for reading!