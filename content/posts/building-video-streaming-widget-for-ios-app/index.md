+++
title = 'Building Video Streaming Widget for iOS App'
date = 2024-05-05T07:20:29+03:00
tags = ["Building", "Video", "Streaming", "Widget", "iOS", "app", "SwiftUI"]
draft = false
+++

### Introduction
I was exploring the idea of creating a YouTube-like widget for the lock screen on iOS devices. 
![alt image](images/0.png#center)
It wasn't easy because most articles on the Internet discussed general implementations, such as for a coffee shop or a to-do list. 
Even when I found some similar versions, the project wouldn't compile. 
I made the decision to approach it my way, so here's what I found out:

### Caveats
- After being stuck for two or more hours without understanding `why, after tapping on a button, I wasn't able to receive a callback from it and the widget always opened the main iOS app`, I realized that I forgot to add `AppIntent` - without it, you can't handle actions for iOS 17.
``` swift
import AppIntents

struct ButtonIntent: AppIntent {
    
    static let title: LocalizedStringResource = "ButtonIntent"

    @Parameter(title: "id")
    var id: String

    func perform() async throws -> some IntentResult {
        if id == Command.playPause.rawValue {
            DataModel.shared.isPlaying.toggle()
        }
        return .result()
    }

}
```
- Another crucial point is not to forget to add an explicit `init`. If you don't implement it explicitly, it will not work.
``` swift
import AppIntents

struct ButtonIntent: AppIntent {

    static let title: LocalizedStringResource = "ButtonIntent"

    @Parameter(title: "id")
    var id: String

    init(id: String) {
        self.id = id
    }

    init() {}

    func perform() async throws -> some IntentResult {
        if id == Command.playPause.rawValue {
            DataModel.shared.isPlaying.toggle()
        }
        return .result()
    }

}
```
- Lastly, I attempted to add a `Slider`, but I found that it's not supported by the widget. My solution was to choose a `ProgressView` instead.

### Implementation
``` swift
struct YouTubeLockScreenWidget: View {

    var body: some View {
        VStack {
            Spacer()
            ProgressView(value: DataModel.shared.currentTime, total: DataModel.shared.totalTime)
                .progressViewStyle(.linear)
            Spacer()
            HStack {
                ForEach(Command.allCases) { command in
                    Button(intent: ButtonIntent(id: command.id)) {
                        Image(systemName: imageSystemName(isPlaying: DataModel.shared.isPlaying, command: command))
                    }
                }
            }
        }
    }

}
```

``` swift
final class DataModel {
    static let shared = DataModel()
    var isPlaying: Bool = false
    var currentTime: TimeInterval = 34
    var totalTime: TimeInterval = 304
}

enum Command: String, CaseIterable {
    case previous
    case playPause
    case next
}

extension Command: Identifiable {
    var id: String {
        rawValue
    }
}

func imageSystemName(isPlaying: Bool, command: Command) -> String {
    switch command {
    case .playPause:
        if isPlaying {
            return "pause.fill"
        } else {
            return "play.fill"
        }
    case .next:
        return "forward.fill"
    case .previous:
        return "backward.fill"
    }
}
```

I had not replaced default generated code when I was adding widget to the project. 
I just added `YouTubeLockScreenWidget` to generated `VideoStreamingWidgetEntryView`.
``` swift 
struct VideoStreamingWidgetEntryView : View {
    var entry: Provider.Entry

    var body: some View {
        YouTubeLockScreenWidget()
    }
}
```

#### Thank you for reading! 😊
