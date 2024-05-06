+++
title = 'Building Dynamic Island for Video Streaming App'
date = 2024-05-06T07:35:52+03:00
tags = ["Building", "Dynamic Island", "Video", "Streaming", "iOS", "app", "SwiftUI"]
draft = false
+++

### Introduction
I was curious about how to add Dynamic Island and implement it into a Video Streaming App. 
![alt image](images/0.png#center)
Here are a few steps on how you can achieve this.

### Caveats
- Debugging Dynamic Island can be a bit tricky; it only works when the main app is running. If you try to run it separately, you will encounter the error `SendProcessControlEvent:toPid: encountered an error: Error Domain=com.apple.dt.deviceprocesscontrolservice Code=8 "Failed to show Widget".` The solution is to configure live activities and run them through the main app.
- Be aware that when you add a widget to the project, in some cases, it adds all main target files to `Compile Sources`.

### Implementation
Dynamic Islands are divided into different sizes: `minimal`, `compactTrailing`, `compactLeading`, and `expanded`.
Before proceeding, you need to add `LiveActivityManager` to be able to display Dynamic Islands.

``` swift 
import Foundation
import ActivityKit

struct VideoStreamingWidgetActivityAttributes: ActivityAttributes {
    struct ContentState: Codable, Hashable {
        var isPlaying: String = "0"
    }
}

final class LiveActivityManager {

    @discardableResult
    static func startActivity(isPlaying: String) throws -> String {

        var activity: Activity<VideoStreamingWidgetActivityAttributes>?
        let initialState = VideoStreamingWidgetActivityAttributes.ContentState(isPlaying: isPlaying)

        do {
            activity = try Activity.request(attributes: VideoStreamingWidgetActivityAttributes(), contentState: initialState, pushType: nil)

            guard let id = activity?.id else { throw
                LiveActivityErrorType.failedToGetID }
            return id
        } catch {
            throw error
        }
    }

}

enum LiveActivityErrorType: Error {
    case failedToGetID
}
```

#### UI
#### compactTrailing
``` swift
compactTrailing: {
                Text("0:33")
                    .foregroundColor(.red)
                    .padding(.trailing, 8)
                } 
```
#### compactLeading
``` swift
compactLeading: {
                Image(systemName: "waveform")
                    .resizable()
                    .aspectRatio(contentMode: .fit)
                    .foregroundColor(.red)
                    .padding(.leading, 8)
                } 
```
#### expanded
``` swift
  DynamicIsland {
                DynamicIslandExpandedRegion(.center) {
                    HStack {
                        Text("0:33")
                            .foregroundStyle(.gray)
                            .frame(height: 4)
                        ProgressView(value: 33, total: 344)
                            .progressViewStyle(.linear)
                        Text("-2:33")
                            .foregroundStyle(.gray)
                            .frame(height: 4)
                    }
                }
                DynamicIslandExpandedRegion(.bottom) {
                        HStack(spacing: 24) {
                            ForEach(Command.allCases) { command in
                                Button(intent: ButtonIntent(id: command.id)) {
                                    Image(systemName: imageSystemName(isPlaying: true, command: command))
                                }
                            }
                        }
                }
            } 
```
#### Helpers
``` swift 
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

        }
        return .result()
    }

}
```

#### Thank you for reading! 😊
