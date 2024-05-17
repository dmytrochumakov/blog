+++
title = 'Animation - SwiftUI'
date = 2024-05-17T07:23:46+03:00
tags = ["Animation", "SwiftUI"]
draft = false
+++

### Introduction
I was eager to learn about creating complex animations in SwiftUI. The few questions that were on my mind included what types of animations exist and what I can animate. Here is what I found:

### Types of Animation
SwiftUI has `explicit` and `implicit` animation types.
- **Implicit Animation:** This is specified with the `.animation()` modifier. SwiftUI will animate changes in old and new values.
    ```swift
    struct ImplicitAnimation: View {
        @State private var half = false
        @State private var dim = false

        var body: some View {
            Image("tower")
                .scaleEffect(half ? 0.5 : 1.0)
                .opacity(dim ? 0.2 : 1.0)
                .animation(.easeInOut(duration: 1.0))
                .onTapGesture {
                    self.dim.toggle()
                    self.half.toggle()
                }
        }
    }
    ```
    ![Implicit Animation](images/implicit.gif#center)

- **Explicit Animation:** This is specified with the `withAnimation` closure. Only those parameters that depend on a value changed inside the `withAnimation` closure will be animated.
    ```swift
    struct ExplicitAnimation: View {
        @State private var half = false
        @State private var dim = false

        var body: some View {
            Image("tower")
                .scaleEffect(half ? 0.5 : 1.0)
                .opacity(dim ? 0.5 : 1.0)
                .onTapGesture {
                    self.half.toggle()
                    withAnimation(.easeInOut(duration: 1.0)) {
                        self.dim.toggle()
                    }
                }
        }
    }
    ```
    ![Explicit Animation](images/explicit.gif#center)

### What is Possible to Animate
- You can animate single parameters such as `size`, `offset`, `color`, `scale`, etc.
- You can conform to the [`Animatable`](https://developer.apple.com/documentation/swiftui/animatable) protocol and describe how to animate a property of a view.
- You can also animate multiple parameters with [`AnimatablePair`](https://developer.apple.com/documentation/swiftui/animatablepair).

### Resources
An invaluable resource is [The SwiftUI Lab](https://swiftui-lab.com/), which has more than 5 posts only about animation in SwiftUI.


#### Thank you for reading! 😊
