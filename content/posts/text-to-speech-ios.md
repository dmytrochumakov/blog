+++
title = 'Text To Speech iOS'
date = 2024-05-24T07:52:07+03:00
tags = ["Text To Speech", "iOS"]
draft = false
+++

### Introduction
I was eager to learn how converting Text To Speech works in iOS. Here is what I discovered:

### First Step
The first step is to add [`AVSpeechSynthesizer`](https://developer.apple.com/documentation/avfaudio/avspeechsynthesizer/), an object that produces synthesized speech from text utterances.
```swift
@State private var speechSynthesizer = AVSpeechSynthesizer()
```

### Second Step
The second step is to add [`AVSpeechUtterance`](https://developer.apple.com/documentation/avfaudio/avspeechutterance), an object that encapsulates the text for speech synthesis.
```swift
private var utterance: AVSpeechUtterance {
    let inputMessage = "Hello world!"
    let utterance = AVSpeechUtterance(string: inputMessage)
    utterance.voice = AVSpeechSynthesisVoice(language: "en-US")
    return utterance
}
```

#### Optional
You can configure `pitch`, `rate`, and `voice` parameters.

### Third Step
The third step is to add a `speak` method that actually allows you to convert Text To Speech.
```swift
speechSynthesizer.speak(utterance)
```

#### Complete Example
```swift
import SwiftUI
import AVFoundation

struct ContentView: View {

    @State private var speechSynthesizer = AVSpeechSynthesizer()

    private var utterance: AVSpeechUtterance {
        let inputMessage = "Hello world!"
        let utterance = AVSpeechUtterance(string: inputMessage)
        utterance.voice = AVSpeechSynthesisVoice(language: "en-US")
        return utterance
    }

    var body: some View {
        VStack {
            Button("Speak") {
                speechSynthesizer.speak(utterance)
            }
        }
        .padding()
    }

}

#Preview {
    ContentView()
}
```

#### Thank you for reading! 😊
