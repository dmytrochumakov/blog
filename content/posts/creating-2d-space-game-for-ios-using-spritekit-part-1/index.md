+++
title = 'Creating a 2D Space Game for iOS Using SpriteKit - Part 1'
date = 2024-06-28T07:18:59+03:00
tags = ["Creating", "2D", "Game", "iOS", "SpriteKit", "Swift"]
draft = false
+++

### Introduction
I have never tried creating a game before; it feels like magic to me. I know that games have an enormous amount of underlying layers of abstractions and tools such as game engines, rendering, and so on. I have always been eager to learn at least 1% of the game creation process. In this article, I'm going to explore step-by-step instructions for creating a game for the iOS platform using SpriteKit.

### First Step
The first step is to create an Xcode project using the iOS game template and SpriteKit Game Technology.
![alt image](images/0.png#center)
![alt image](images/1.png#center)

### Second Step
The second step is to add resources like images and sounds. You can find assets inside the Assets folder [Download Project](https://github.com/dmytrochumakov/2d-space-game-ios/archive/refs/heads/main.zip).

### Third Step
The third step is to override the `didMove(to:)` method - it tells you when the scene is presented by a view. This method is similar to `viewDidLoad` for `UIViewController`. We will be implementing logic inside it.
``` swift
override func didMove(to view: SKView) { }
```

### Creating and Adding Starfield Background
The next step is to create and add a starfield using [`SKEmitterNode`](https://developer.apple.com/documentation/spritekit/skemitternode) as a child to `SKScene`. After that, you will be able to see the starfield in the background.

![alt image](images/6.gif#center)

```swift
self.starfield = SKEmitterNode(fileNamed: "Starfield")
starfield.position = CGPoint(x: self.frame.size.width / 2, y: self.frame.size.height)
starfield.advanceSimulationTime(10)

self.addChild(starfield)

starfield.zPosition = -1
```

#### Let's Dive a Little Deeper
##### Custom Particles Creation
If you were wondering how to create custom particles similar to the starfield background, you need to:
- Create a new SpriteKit Particle File:
![alt image](images/3.png#center)
- Choose a particle template:
![alt image](images/4.png#center)
- Select the created file -> Open the Inspectors side menu and configure it with settings that you like.
![alt image](images/5.png#center)

##### SKEmitterNode
Initialization of `SKEmitterNode(fileNamed: "Starfield")` helps create a starfield background.

> `SKEmitterNode` is a node that automatically creates and renders small particle sprites. Particles are privately owned by SpriteKit—your game cannot access the generated sprites. For example, you cannot add physics shapes to particles. Emitter nodes are often used to create smoke, fire, sparks, and other particle effects.

##### advanceSimulationTime
`advanceSimulationTime(10)` - Advances the emitter particle simulation. In other words, you do not need to wait until the particles reach the bottom of the screen.
![alt image](images/7.gif#center)

##### addChild
`self.addChild(starfield)` - Adds a node to the end of the receiver’s list of child nodes. After that, you will be able to see the starfield background.

##### zPosition
`zPosition = -1` - Moves the starfield to the back of the screen.

### Resources
Enormous appreciation for [Brian Advent](https://youtu.be/cJy61bOqQpg?si=Tei9yLmjD551q2Zx) for his comprehensive video. Without it, I would still be surfing the internet and collecting pieces of the puzzle.

### Download Materials
[Download Project](https://github.com/dmytrochumakov/2d-space-game-ios/archive/refs/heads/main.zip)

#### Thank you for reading! 😊
