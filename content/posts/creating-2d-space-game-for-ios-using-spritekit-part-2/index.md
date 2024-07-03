+++
title = 'Creating a 2D Space Game for iOS Using SpriteKit - Part 2'
date = 2024-07-03T06:39:04+03:00
tags = ["Creating", "2D", "Game", "iOS", "SpriteKit", "Swift"]
draft = false
+++

### Introduction 
In the [previous chapter](https://dmytros.blog/posts/creating-2d-space-game-for-ios-using-spritekit-part-1/), I started talking about the video game creation process, from project setup to adding the background. Now, I'm going to add the player and physics to it.

You can [download the project here](https://github.com/dmytrochumakov/2d-space-game-ios/archive/refs/heads/main.zip).

### First Step
The first step is to initialize `player` using [`SKSpriteNode`](https://developer.apple.com/documentation/spritekit/skspritenode), set up `player.position`, and add `player` as a child node.
> **SKSpriteNode** - is an onscreen graphical element that can be initialized from an image or a solid color.

```swift 
self.player = SKSpriteNode(imageNamed: "shuttle")
player.position = CGPoint(x: self.frame.size.width / 2, y: player.size.height / 2 + 20)

self.addChild(player)
```

### Second Step
The second step is to remove Earth's gravity effect from the physics world, because the player will be looking into the screen from a top-down perspective and gravity effects make no difference for it.
```swift 
self.physicsWorld.gravity = CGVector(dx: 0, dy: 0)
```

### Third Step
The third step is to add `contactDelegate` to be able to respond when physics bodies come into contact.
> [SKPhysicsContactDelegate](https://developer.apple.com/documentation/spritekit/skphysicscontactdelegate) - An object that implements the SKPhysicsContactDelegate protocol can respond when two physics bodies with overlapping `contactTestBitMask` values are in contact with each other in a physics world. You can use the contact delegate to play a sound or execute game logic, such as increasing a player’s score when a contact event occurs. 

```swift
self.physicsWorld.contactDelegate = self
``` 

```swift 
private let alienCategory: UInt32 = 0x1 << 1
private let photonTorpedoCategory: UInt32 = 0x1 << 0

alien.physicsBody?.categoryBitMask = alienCategory
alien.physicsBody?.contactTestBitMask = photonTorpedoCategory
alien.physicsBody?.collisionBitMask = 0

torpedoNode.physicsBody?.categoryBitMask = photonTorpedoCategory
torpedoNode.physicsBody?.contactTestBitMask = alienCategory
torpedoNode.physicsBody?.collisionBitMask = 0
torpedoNode.physicsBody?.usesPreciseCollisionDetection = true
```

```swift
// MARK: - SKPhysicsContactDelegate
extension GameScene: SKPhysicsContactDelegate {

    func didBegin(_ contact: SKPhysicsContact) {
        var firstBody: SKPhysicsBody
        var secondBody: SKPhysicsBody

        if contact.bodyA.categoryBitMask < contact.bodyB.categoryBitMask {
            firstBody = contact.bodyA
            secondBody = contact.bodyB
        } else {
            firstBody = contact.bodyB
            secondBody = contact.bodyA
        }

        if (firstBody.categoryBitMask & photonTorpedoCategory) != 0 && (secondBody.categoryBitMask & alienCategory) != 0 {
            torpedoDidCollideWithAlien(torpedoNode: firstBody.node as! SKSpriteNode, alienNode: secondBody.node as! SKSpriteNode)
        }
    }

}
```

> `didBegin(_:)` is an instance method called when two bodies first contact each other.

### Additionally
We need to add `scoreLabel` to track the player's score:

```swift
self.scoreLabel = SKLabelNode(text: "Score: 0")
scoreLabel.position = CGPoint(x: self.frame.size.width / 2, y: self.frame.size.height - 50)
scoreLabel.fontName = "AmericanTypewriter-Bold"
scoreLabel.fontSize = 36
scoreLabel.fontColor = UIColor.white
score = 0

self.addChild(scoreLabel)
```

`gameTimer` to add an alien every 0.75 milliseconds:

```swift
self.gameTimer = Timer.scheduledTimer(timeInterval: 0.75, target: self, selector: #selector(addAlien), userInfo: nil, repeats: true)
```

`motionManager` to change the user's position using the accelerometer:

```swift
self.motionManager = CMMotionManager()
motionManager.accelerometerUpdateInterval = 0.1
motionManager.startAccelerometerUpdates(to: OperationQueue.current!) { (data: CMAccelerometerData?, error: Error?) in
    if let accelerometerData = data {
        let acceleration = accelerometerData.acceleration
        self.xAcceleration = CGFloat(acceleration.x) * 0.75 + self.xAcceleration * 0.25
    }
}
```

`touchesEnded` method to fire a torpedo:

```swift
override func touchesEnded(_ touches: Set<UITouch>, with event: UIEvent?) {
    fireTorpedo()
}
```

`didSimulatePhysics` method to change the player's position:

```swift
override func didSimulatePhysics() {
    player.position.x += xAcceleration * 50

    if player.position.x < 0 {
        player.position = CGPoint(x: self.frame.size.width, y: player.position.y)
    } else if player.position.x > self.frame.size.width {
        player.position = CGPoint(x: 0, y: player.position.y)
    }
}
```

### Summarizing
It was an amazing experience. I was surprised that the entire game was less than 200 lines of code. I tried my best to create a step-by-step instruction of the game creation process. I hope this article will be helpful for you.

#### Thank you for reading! 😊
