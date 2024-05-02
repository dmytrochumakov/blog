+++
title = 'Exploring visionOS'
date = 2024-05-02T08:45:39+03:00
tags = ["visionOS", "app", "3d", "SwiftUI", "RealityKit", "Reality Composer Pro"]
draft = false
+++

### Introduction
I find myself fascinated by the idea of creating an app for `visionOS` where I could possibly display 3D AirPods that I like.
![alt image](images/0.gif#center)
Here are a few steps on how you can do the same:

### First step
The first step that you need to do is to create a `visionOS` project.

### Second step
The next step would be adding a 3D object to `Reality Composer Pro` and exporting it as a `.usdz` file. You can download free 3D objects [here](https://sketchfab.com/feed). All you need to do to download content is to register on this site.

### Third step 
The final step would be adding code to display the 3D object. To do that, we need to add [Model3D](https://developer.apple.com/documentation/realitykit/model3d/). It helps asynchronously load and display a 3D model.

``` swift 
import SwiftUI
import RealityKit
import RealityKitContent

struct AirPodsMaxAnimation: View {
    var body: some View {
        NavigationStack {
            VStack {
                Model3D(named: "Airpods_Max_Pink") { model in
                     model
                         .resizable()
                         .aspectRatio(contentMode: .fit)
                         .scaleEffect(0.5)
                         .phaseAnimator([false, true]) { AirPodsMax, threeDYRotate in
                             AirPodsMax
                                 .rotation3DEffect(.degrees(threeDYRotate ? 0 : -360 * 5), axis: (x: 0, y: 1, z: 0))
                         } animation: { threeDYRotate in
                                 .linear(duration: 25).repeatForever(autoreverses: false)
                         }
                 } placeholder: {
                     ProgressView()
                 }
            }
            .navigationTitle("Airpods Max Pink")            
        }
    }
}

#Preview(windowStyle: .automatic) {
    AirPodsMaxAnimation()
}
```

#### Thank you for reading! 😊
