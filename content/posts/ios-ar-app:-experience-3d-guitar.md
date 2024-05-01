+++
title = 'iOS AR App: Experience 3D Guitar'
date = 2024-05-01T15:57:54+03:00
tags = ["ios", "ar", "app", "3d"]
draft = false
+++

### Introduction
I was searching for an AR implementation of a 3D guitar inside an iOS app. Here's what I discovered:

### First Step
The first step is not related to building the app. 
Before that you need to create a project using the `Reality Composer Pro` app (you can find it through `Spotlight search`). 

### Second Step
After that, you need to visit https://developer.apple.com/augmented-reality/quick-look/ and download one of the `USDZ` files. 
In my case, I chose the [3D guitar](https://developer.apple.com/augmented-reality/quick-look/models/stratocaster/fender_stratocaster.usdz). 

### Third Step
Now, you can start diving into AR implementation inside the iOS project:
- You need to add the `Privacy - Camera Usage Description` key to be able to use the camera
- Put `your_file_name.usdz` into the iOS project
- Create an `ARViewRepresentable` 
``` swift
struct ARViewRepresentable: UIViewRepresentable {

    func makeUIView(context: Context) -> some ARView {
        let arView = ARView(frame: .zero)       
        return arView
    }

    func updateUIView(_ uiView: UIViewType, context: Context) {

    }
    
}
```
- Load, and anchor the 3D model
``` swift 
    func makeUIView(context: Context) -> some ARView {
        let arView = ARView(frame: .zero)
        // Load 3D model
        guard let guitarModelURL = Bundle.main.url(forResource: "fender_stratocaster", withExtension: "usdz") else {
            fatalError("Failed to load model file.")
        }

        let guitarModel = try! Entity.load(contentsOf: guitarModelURL)
        // Anchor 3D model
        let anchorEntity = AnchorEntity(.plane(.horizontal, classification: .any, minimumBounds: .zero))
        anchorEntity.addChild(guitarModel)        

        return arView
    }
```
- and finally add it to `arView.scene`
``` swift
   func makeUIView(context: Context) -> some ARView {
        let arView = ARView(frame: .zero)
        // Load 3D model
        guard let guitarModelURL = Bundle.main.url(forResource: "fender_stratocaster", withExtension: "usdz") else {
            fatalError("Failed to load model file.")
        }

        let guitarModel = try! Entity.load(contentsOf: guitarModelURL)
        // Anchor 3D model
        let anchorEntity = AnchorEntity(.plane(.horizontal, classification: .any, minimumBounds: .zero))
        anchorEntity.addChild(guitarModel)

        // Add anchor to scene
        arView.scene.addAnchor(anchorEntity)

        return arView
    }
```

That's it! Play and enjoy 😊

#### Thank you for reading! 😊
