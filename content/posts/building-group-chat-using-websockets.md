+++
title = 'Building Group Chat using WebSockets'
date = 2024-05-02T15:15:14+03:00
tags = ["iOS", "Chat", "app", "WebSockets", "SwiftUI"]
draft = false
+++

### Introduction
I never had a chance to work with WebSockets, so I decided to take a look and create a group chat. Here's what I discovered:
- To be able to send and receive messages, you need to create an interface for communication between a server and your application. In my case, I chose `sendMessage` and `receiveMessage` methods.
- For the server-side, I chose `Node.js`.
- For the iOS application, I chose the [Socket.IO](https://github.com/socketio/socket.io-client-swift) library.

### Implementation
Let’s dive deeper into the implementation.

#### First step
The first step would be to create a `server.js` file to be able to handle incoming events.
``` js
const express = require('express');
const app = express();
const server = require('http').Server(app);
const io = require('socket.io')(server);
const { randomUUID } = require('crypto');

const users = new Map();

io.on('connection', (socket) => {
  let username = socket.handshake.auth.username;

  console.log('a user connected');

  users.set(socket.id, username);

  io.emit('receiveNewUser', username, Object.fromEntries(users));

  socket.on('sendMessage', (message) => {
    const username = users.get(socket.id);
    io.emit('receiveMessage', randomUUID(), username, message);
  });

  socket.on('disconnect', () => {
    console.log('user disconnected');
    users.delete(socket.id);
  });
});

server.listen(3000, () => {
  console.log('listening on *:3000');
});
```

#### Second step
The next step would be creating a `ChatService` that will be responsible for `connect`, `disconnect`, `send`, and `receive` data.
``` swift 
import SocketIO

final class ChatService {

    private var manager: SocketManager!
    private var socket: SocketIOClient!
    private var username: String!

    init() {
        manager = SocketManager(socketURL: URL(string: "http://localhost:3000")!)
        socket = manager.defaultSocket
    }

    func connect(username: String) {
        self.username = username
        socket.connect(withPayload: ["username": username])
    }

    func disconnect() {
        socket.disconnect()
    }

    func sendMessage(_ message: String) {
        socket.emit("sendMessage", message)
    }

    func sendUsername(_ username: String) {
        socket.emit("sendUsername", username)
    }

    func receiveMessage(_ completion: @escaping (String, String, UUID) -> Void) {
        socket.on("receiveMessage") { data, _ in
            if let text = data[2] as? String,
               let id = data[0] as? String,
               let username = data[1] as? String {
                completion(username, text, UUID.init(uuidString: id) ?? UUID())
            }
        }
    }

    func receiveNewUser(_ completion: @escaping (String, [String:String]) -> Void) {
        socket.on("receiveNewUser") { data, _ in
            if let username = data[0] as? String,
               let users = data[1] as? [String:String] {
                completion(username, users)
            }
        }
    }
    
}
```

#### Third step
The next step would be creating a `ViewModel` communicating with the `ChatService`.
``` swift
import Foundation

final class ViewModel: ObservableObject {
    private let chatService: ChatService = ChatService()
    @Published var message: String = ""
    @Published var messages: [Message] = []
    @Published var username: String = ""
    @Published var users: [String:String] = [:]
    @Published var newUser: String = ""
    @Published var showUsernamePrompt: Bool = true
    @Published var isShowingNewUserAlert = false
}

extension ViewModel {

    func connect() {
        chatService.connect(username: username)
        chatService.receiveMessage { username, text, id in
            self.receiveMessage(username: username, text: text, id: id)
        }
        chatService.receiveNewUser { username, users in
            self.receiveNewUser(username: username, users: users)
        }
        showUsernamePrompt = false
    }

    func sendMessage() {
        chatService.sendMessage(message)
        message = ""
    }

    func receiveMessage(username: String, text: String, id: UUID) {
        messages.append(Message(username: username, text: text, id: id))
    }

    func receiveNewUser(username: String, users: [String:String]) {
        self.users = users
        self.newUser = username

        self.isShowingNewUserAlert = self.username != username
    }

    func disconnect() {
        chatService.disconnect()
        message = ""
        messages = []
        username = ""
        users = [:]
        newUser = ""
        showUsernamePrompt = true
        isShowingNewUserAlert = false
    }

}
```

#### Fourth step
The last step would be creating `UI` and connecting it with the `ViewModel`.
``` swift
import SwiftUI

struct ChatView: View {

    @StateObject private var viewModel = ViewModel()

    var body: some View {
        NavigationView {
            VStack {
                if viewModel.showUsernamePrompt {
                    HStack {
                        TextField("Enter your username", text: $viewModel.username)
                            .textFieldStyle(RoundedBorderTextFieldStyle())

                        Button(action: viewModel.connect) {
                            Text("Connect")
                        }
                    }
                    .padding()
                } else {
                    List {
                        ForEach(viewModel.messages, id: \.self) { message in
                            HStack {
                                if message.username == viewModel.username {
                                    Text("Me:")
                                        .font(.subheadline)
                                        .foregroundColor(.blue)
                                } else {
                                    Text("\(message.username):")
                                        .font(.subheadline)
                                        .foregroundColor(.green)
                                }

                                Text(message.text)
                            }
                        }
                    }

                    HStack {
                        TextField("Enter a message", text: $viewModel.message)
                            .textFieldStyle(RoundedBorderTextFieldStyle())

                        HStack {
                        Button(action: viewModel.sendMessage) {
                                Text("Send")
                            }
                            Button(action: viewModel.disconnect) {
                                Text("Disconnect")
                            }
                        }
                    }
                    .padding()
                }
            }
            .navigationBarTitle("Group Chat \(viewModel.users.count > 0 ? "(\(viewModel.users.count) connected)" : "")")
            .navigationBarTitleDisplayMode(.inline)
            .alert("\(viewModel.newUser) just joined the chat!",
                   isPresented: $viewModel.isShowingNewUserAlert) {
                Button("OK", role: .cancel) {
                    viewModel.isShowingNewUserAlert = false
                }
            }
        }
    }

}

#Preview {
    ChatView()
}
```

#### Thank you for reading! 😊
