+++
title = 'Animation - UIKit'
date = 2024-05-21T06:31:02+03:00
tags = ["Animation", "UIKit", "Core Animation", "CAShapeLayer", "CABasicAnimation", "CAAction"]
draft = false
+++

### Introduction
I was curious about creating animations in UIKit. I wanted to animate different properties such as `color` and `path`.
![alt image](images/0.gif#center)

Here is what I found:

It’s impossible to create complex animations only by using the [block-based animation API](https://developer.apple.com/documentation/uikit/uiview#1654491). To do that, you need the [`Core Animation`](https://developer.apple.com/documentation/quartzcore) API and [`CAPropertyAnimation`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/) with its various subclasses.

Complex animation in UIKit is based on a few key components:
- [`CAShapeLayer`](https://developer.apple.com/documentation/quartzcore/cashapelayer) - provides extensive customization options: `path`, `stroke`, `fill`, `shadow`
- [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) - helps animate `color` or change the `path`

### Implementation
#### First Step
The first step is to create a shape layer that will draw an arrow using [`CAShapeLayer`](https://developer.apple.com/documentation/quartzcore/cashapelayer).
```swift
private lazy var arrowShapeLayer: CAShapeLayer = {
    let arrowShapeLayer = CAShapeLayer()
    arrowShapeLayer.strokeColor = direction.arrowColour.cgColor
    arrowShapeLayer.lineWidth = ArrowView.arrowLineWidth
    arrowShapeLayer.lineCap = .round
    arrowShapeLayer.fillColor = UIColor.clear.cgColor
    return arrowShapeLayer
}()
```

#### Second Step
The second step is to animate the arrow direction by changing the shape layer's path and stroke color.
```swift
var direction: Direction = .up {
    didSet {
        guard oldValue != direction else { return }
        let pathAnimation = CABasicAnimation(keyPath: "path")
        pathAnimation.fromValue = arrowShapeLayer.presentation()?.path
        pathAnimation.duration = 0.5
        arrowShapeLayer.add(pathAnimation, forKey: "pathAnimation")
        
        let strokeColourAnimation = CABasicAnimation(keyPath: "strokeColor")
        strokeColourAnimation.fromValue = arrowShapeLayer.presentation()?.strokeColor
        strokeColourAnimation.duration = 0.5
        arrowShapeLayer.add(strokeColourAnimation, forKey: "strokeColourAnimation")
        
        arrowShapeLayer.path = direction.arrowPath(in: bounds).cgPath
        arrowShapeLayer.strokeColor = direction.arrowColour.cgColor
    }
}
```

#### Third Step
The third step is to synchronize Core Animation with UIKit animation by requesting the [`CAAction`](https://developer.apple.com/documentation/quartzcore/caaction) property.
```swift
var direction: Direction = .up {
    didSet {
        guard oldValue != direction else { return }
        if let backgroundColourAnimation = action(for: layer, forKey: "backgroundColor") as? CABasicAnimation {
            let pathAnimation = backgroundColourAnimation.copy(forKeyPath: "path")
            pathAnimation.fromValue = arrowShapeLayer.presentation()?.path
            arrowShapeLayer.add(pathAnimation, forKey: "pathAnimation")
            
            let strokeColourAnimation = backgroundColourAnimation.copy(forKeyPath: "strokeColor")
            strokeColourAnimation.fromValue = arrowShapeLayer.presentation()?.strokeColor
            arrowShapeLayer.add(strokeColourAnimation, forKey: "strokeColourAnimation")
        }
        
        arrowShapeLayer.path = direction.arrowPath(in: bounds).cgPath
        arrowShapeLayer.strokeColor = direction.arrowColour.cgColor
    }
}
```

### Resources
If you are curious, I would recommend you read a more detailed explanation from [Darjeeling Steve's blog](https://darjeelingsteve.com/articles/Synchronising-CALayer-and-UIKit-Animations.html). I found this resource incredible and full of comprehensive information.


#### Thank you for reading! 😊
