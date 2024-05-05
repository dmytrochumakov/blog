+++
title = 'Building Video Streaming iOS App'
date = 2024-05-03T07:34:56+03:00
tags = ["Building", "Video", "Streaming", "iOS", "app", "SwiftUI"]
draft = false
+++

### Introduction
I was looking for a way to add a video player to my iOS app that could be able to play remote videos.

### Caveats  
#### Problem
I found that you can't open `Vimeo` or `Youtube` videos because of `AVFoundationErrorDomain Code=-11850 "Operation Stopped" UserInfo={NSLocalizedFailureReason=The server is not correctly configured Domain=NSOSStatusErrorDomain Code=-12939` error. 
I don’t know exactly what this means, but I'm speculating it's related to some protection.

#### Solution
My solution was to find another video that is not related to those platforms.

### Implementation
[`AVKit`](https://developer.apple.com/documentation/avkit) has a built-in video player called [`VideoPlayer`](https://developer.apple.com/documentation/avkit/videoplayer). 
All you need to play a video is to pass [`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer/) with `videoURL`.
```swift
@ViewBuilder
var fullScreenVideoPlayer: some View {
    let avPlayer = AVPlayer(url: videoURL)
    VideoPlayer(player: avPlayer)
        .edgesIgnoringSafeArea(.all)
        .onAppear {
            avPlayer.play()
        }
}
```

I will dive a little deeper with widgets in the next chapters.

#### Thank you for reading! 😊
