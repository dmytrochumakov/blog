+++
title = 'Combine practical usage examples'
date = 2024-06-22T07:13:00+03:00
tags = ["Combine", "Swift"]
draft = false
+++

### Introduction
When working in a large codebase with a significant number of async events, I often found myself in situations where I couldn't combine events effectively. This resulted in optimization problems and inefficient consumption of OS resources.

The codebase contained `closures` and `async/await`, so it wasn't possible to use operators like `merge` or `combineLatest`. After discovering this limitation, I decided to add new methods using [`Combine`](https://developer.apple.com/documentation/combine).

I will be demonstrating this with a simple `NetworkService` responsible only for executing and validating requests using Combine. Let's dive into the implementation.

### First Step
The first step is to create the `NetworkService` with `request(_ endpoint: Endpoint)` method.

```swift
func request<T: Decodable>(_ endpoint: Endpoint) -> AnyPublisher<T, Error> {}
```

#### Quick Explanation of What AnyPublisher Is
The [AnyPublisher](https://developer.apple.com/documentation/combine/anypublisher) returns a publisher from a method without exposing the specific type of publisher you are using internally. It helps hide implementation details.

### Second Step
The second step is to add a few helpers such as `Endpoint` and `NetworkError`.

```swift
struct Endpoint {
    let url: String
    let headers: [String: String]?
    let body: Data?
    let httpMethod: HTTPMethod

    func urlRequest(with url: URL) -> URLRequest {
        var urlRequest = URLRequest(url: url)
        urlRequest.httpMethod = httpMethod.rawValue
        urlRequest.allHTTPHeaderFields = headers ?? [:]
        urlRequest.httpBody = body
        return urlRequest
    }

    enum HTTPMethod: String {
        case GET
        case POST
        case PUT
        case DELETE
    }
}
```

```swift
enum NetworkError: Error {
    case invalidURL
    case invalidResponseType
    case jsonDecoderError(_ error: Error)
}
```

### Third Step
The third step is to implement the `request` method.

```swift
func request<T: Decodable>(_ endpoint: Endpoint) -> AnyPublisher<T, Error> {
    guard let url = URL(string: endpoint.url) else {
        return Fail<T, Error>(error: NetworkError.invalidURL).eraseToAnyPublisher()
    }
    return URLSession.shared
        .dataTaskPublisher(for: endpoint.urlRequest(with: url))
        .tryMap { output in
            guard output.response is HTTPURLResponse else {
                throw NetworkError.invalidResponseType
            }
            return output.data
        }
        .decode(type: T.self, decoder: JSONDecoder())
        .mapError { error in
            NetworkError.jsonDecoderError(error)
        }
        .eraseToAnyPublisher()
}
```

### Fourth Step
The fourth step is to test if `NetworkService` works as expected.

```swift
import Combine
import Inject
import SwiftUI

public struct ContentView: View {
    @ObserveInjection var inject

    private let networkService: NetworkService = .init()
    @State private var cancellables = Set<AnyCancellable>()
    @State private var post: Post?

    public init() {}

    public var body: some View {
        VStack {
            Button("Fetch Data") {
                networkService
                    .request(Endpoint(url: "https://jsonplaceholder.typicode.com/posts/1", httpMethod: .GET))
                    .sink(receiveCompletion: { completion in
                        switch completion {
                        case .finished:
                            break
                        case let .failure(error):
                            print(error)
                        }
                    }, receiveValue: { (post: Post) in
                        self.post = post
                    })
                    .store(in: &cancellables)
            }
            Text(post?.title ?? "")
        }
        .enableInjection()
    }
}
```

### Summary
Combine is best suited for handling multiple async events by using event-processing operators. Before integrating it into your codebase, make sure to weigh all the pros and cons.

#### Thank you for reading! 😊
