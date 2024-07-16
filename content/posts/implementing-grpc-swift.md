+++
title = 'Implementing gRPC Swift'
date = 2024-07-16T07:16:41+03:00
tags = ["Implementing", "gRPC", "Swift"]
draft = false
+++

### Introduction 
I've never had the chance to use this technology before. I've always wondered how gRPC operates. In this article, I will explore what gRPC is, how to install and use it, and when to use gRPC.

### What is gRPC?
[gRPC](https://blog.postman.com/what-is-grpc/) is an open-source, high-performance framework that facilitates efficient communication in distributed systems. gRPC is an implementation of the RPC (Remote Procedure Call) protocol, which enables services to call functions on other machines as if they were local software methods. gRPC was developed by Google in 2015, and it includes several features that enhance the way remote procedure calls are made. For instance, its use of Protocol Buffers (Protobuf) supports strongly typed service contracts, data serialization, and code generation in a variety of programming languages. It also uses HTTP/2 as its transport protocol, which facilitates bi-directional streaming and reduces latency.

### Installation Process
#### Prerequisites
##### Swift Version
gRPC requires Swift 5.8 or higher.

##### Install Protocol Buffers
Install the `protoc` compiler that is used to generate gRPC service code. The simplest way to do this is to download pre-compiled binaries for your platform (`protoc-<version>-<platform>.zip`) from here: [https://github.com/google/protobuf/releases](https://github.com/google/protobuf/releases).

* Unzip this file.
* Update the environment variable `PATH` to include the path to the `protoc` binary file.

##### Download the Example
You'll need a local copy of the example code to work through this quickstart. Download the example code from our GitHub repository (the following command clones the entire repository, but you just need the examples for this quickstart and other tutorials):

```sh
# Clone the repository at the latest release to get the example code (replacing x.y.z with the latest release, for example 1.13.0):
git clone -b x.y.z https://github.com/grpc/grpc-swift
# Navigate to the repository
cd grpc-swift/
```

### Implementation Process
#### Run a gRPC Application
From the `grpc-swift` directory:

1. Compile and run the server:

   ```sh
   swift run HelloWorldServer
   ```

2. In another terminal, compile and run the client:

   ```sh
   swift run HelloWorldClient
   Greeter received: Hello stranger!
   ```

Congratulations! You've just run a client-server application with gRPC.

#### Update a gRPC Service
Now let's look at how to update the application with an extra method on the server for the client to call. Our gRPC service is defined using protocol buffers; you can find out lots more about how to define a service in a `.proto` file in [What is gRPC?](https://grpc.io/docs/guides/). For now, all you need to know is that both the server and the client "stub" have a `SayHello` RPC method that takes a `HelloRequest` parameter from the client and returns a `HelloReply` from the server, and that this method is defined like this:

```proto
// The greeting service definition.
service Greeter {
  // Sends a greeting.
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings.
message HelloReply {
  string message = 1;
}
```

Let's update this so that the `Greeter` service has two methods. Edit `Protos/upstream/grpc/examples/helloworld.proto` and update it with a new `SayHelloAgain` method, with the same request and response types:

```proto
// The greeting service definition.
service Greeter {
  // Sends a greeting.
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  // Sends another greeting.
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings.
message HelloReply {
  string message = 1;
}
```

(Don't forget to save the file!)

#### Update and Run the Application
We need to regenerate `Sources/Examples/v1/HelloWorld/Model/helloworld.grpc.swift`, which contains our generated gRPC client and server classes. From the `grpc-swift` directory run:

```sh
Protos/generate.sh
```

This also regenerates classes for populating, serializing, and retrieving our request and response types. However, we still need to implement and call the new method in the human-written parts of our example application.

#### Update the Server
In the same directory, open `Sources/Examples/v1/HelloWorld/Server/GreeterProvider.swift`. Implement the new method like this:

```swift
final class GreeterProvider: Helloworld_GreeterAsyncProvider {
  let interceptors: Helloworld_GreeterServerInterceptorFactoryProtocol? = nil

  func sayHello(
    request: Helloworld_HelloRequest,
    context: GRPCAsyncServerCallContext
  ) async throws -> Helloworld_HelloReply {
    let recipient = request.name.isEmpty ? "stranger" : request.name
    return Helloworld_HelloReply.with {
      $0.message = "Hello \(recipient)!"
    }
  }

  func sayHelloAgain(
    request: Helloworld_HelloRequest,
    context: GRPCAsyncServerCallContext
  ) async throws -> Helloworld_HelloReply {
    let recipient = request.name.isEmpty ? "stranger" : request.name
    return Helloworld_HelloReply.with {
      $0.message = "Hello again \(recipient)!"
    }
  }
}
```

#### Update the Client
In the same directory, open `Sources/Examples/v1/HelloWorld/Client/HelloWorldClient.swift`. Call the new method like this:

```swift
func run() async throws {
  // Setup an `EventLoopGroup` for the connection to run on.
  //
  // See: https://github.com/apple/swift-nio#eventloops-and-eventloopgroups
  let group = MultiThreadedEventLoopGroup(numberOfThreads: 1)

  // Make sure the group is shutdown when we're done with it.
  defer {
    try! group.syncShutdownGracefully()
  }

  // Configure the channel, we're not using TLS so the connection is `insecure`.
  let channel = try GRPCChannelPool.with(
    target: .host("localhost", port: self.port),
    transportSecurity: .plaintext,
    eventLoopGroup: group
  )

  // Close the connection when we're done with it.
  defer {
    try! channel.close().wait()
  }

  // Provide the connection to the generated client.
  let greeter = Helloworld_GreeterAsyncClient(channel: channel)

  // Form the request with the name, if one was provided.
  let request = Helloworld_HelloRequest.with {
    $0.name = self.name ?? ""
  }

  do {
    let greeting = try await greeter.sayHello(request)
    print("Greeter received: \(greeting.message)")
  } catch {
    print("Greeter failed: \(error)")
  }

  do {
    let greetingAgain = try await greeter.sayHelloAgain(request)
    print("Greeter received: \(greetingAgain.message)")
  } catch {
    print("Greeter failed: \(error)")
  }
}
```

#### Run!
Just like we did before, from the top-level `grpc-swift` directory:

1. Compile and run the server:

   ```sh
   swift run HelloWorldServer
   ```

2. In another terminal, compile and run the client:

   ```sh
   swift run HelloWorldClient
   Greeter received: Hello stranger!
   Greeter received: Hello again stranger!
   ```

### When to Use gRPC
gRPC was designed to support highly efficient, language-agnostic communication in distributed systems. It is therefore better suited than REST for microservice-based architectures, in which individual services may be developed in different programming languages and may face varying workloads. Additionally, gRPC’s use of Protobuf for binary data serialization makes it the better choice for applications that demand low latency and high throughput, while its support for different streaming patterns makes it ideal for real-time chat and video applications.

### Resources
* [gRPC vs. REST](https://blog.postman.com/grpc-vs-rest/)
* [gRPC Swift Quick Start](https://github.com/grpc/grpc-swift/blob/main/docs/quick-start.md?plain=1#install-protocol-buffers)


#### Thank you for reading! 😊
