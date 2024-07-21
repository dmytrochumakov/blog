+++
title = 'Implementing ChatGPT in an iOS App'
date = 2024-07-21T07:25:28+03:00
tags = ["Implementing", "ChatGPT", "iOS", "App", "Combine", "SwiftUI", "Swift"]
draft = false
+++

### Introduction 
I haven't had the opportunity to build a chatbot before. This topic was trending some time ago, and I always wanted to implement it myself. In this article, I will focus on the steps you need to know to successfully build and run a chatbot application.

### First Step
The first step is to add the [OpenAI](https://github.com/MacPaw/OpenAI) dependency to your project:
```swift 
.package(url: "https://github.com/MacPaw/OpenAI.git", branch: "main")
```

```swift
dependencies: [
    .byNameItem(
        name: "OpenAI",
        condition: .when(platforms: [
            .iOS,
        ])
    ),
],
```

### Second Step
The second step is to [generate an OpenAI key](https://platform.openai.com/api-keys) and replace it inside your project.

### Third Step
The third step is to create `ChatGPTService` and add the `getAssistantResponse` method that will be receiving responses from the GPT model:
```swift 
import Foundation
import OpenAI

final class ChatGPTService {
    private let client: OpenAI

    init() {
        client = OpenAI(apiToken: "YOUR_TOKEN")
    }

    func getAssistantResponse(_ messages: [Message], _ completion: @escaping (Message?) -> Void) {
        let query = ChatQuery(
            messages: messages.map {
                .init(role: .user, content: $0.content)!
            },
            model: .gpt3_5Turbo
        )

        client.chats(query: query) { result in
            switch result {
            case let .success(success):
                guard let choice = success.choices.first,
                      let message = choice.message.content?.string
                else { completion(nil); return }
                DispatchQueue.main.async {
                    completion(Message(content: message, isUser: false))
                }
            case let .failure(error):
                print(error)
                completion(nil)
            }
        }
    }
}
```

### Fourth Step 
The fourth step is to add `ChatService` that will be responsible for adding, sending messages, and redrawing the UI:
```swift 
import Foundation

struct Message: Identifiable {
    var id: UUID = .init()
    var content: String
    var isUser: Bool
}

final class ChatService: ObservableObject {
    @Published private(set) var messages: [Message] = []
    private let chatGPTService: ChatGPTService = .init()

    func sendMessage(_ message: Message) {
        messages.append(message)
        chatGPTService.getAssistantResponse(messages) { [unowned self] message in
            if let message = message {
                messages.append(message)
            }
        }
    }
}
```

### Fifth Step 
The fifth step is to connect the `UI` and `ChatService`:
```swift 
import SwiftUI

struct ChatView: View {
    @StateObject private var chatService: ChatService = .init()
    @State private var message: String = ""

    var body: some View {
        VStack {
            ScrollView {
                ForEach(chatService.messages) { message in
                    Text(message.content)
                }
            }
            HStack {
                TextField("Message", text: $message)
                Button("Send") {
                    chatService.sendMessage(Message(content: message, isUser: true))
                }
            }
        }
    }
}
```

### Caveats 
Be aware of API quota limits; you can exceed your limits very quickly.

### Resources
https://www.youtube.com/watch?v=fkg3UzopiHY&ab_channel=JaredDavidson

#### Thank you for reading! 😊
