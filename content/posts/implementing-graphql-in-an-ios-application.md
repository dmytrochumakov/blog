+++
title = 'Implementing GraphQL in an iOS application'
date = 2024-05-15T07:13:39+03:00
tags = ["Implementing", "GraphQL", "iOS", "application"]
draft = false
+++

### Introduction 
I previously never had a chance to work with GraphQL. I was excited to learn when to apply this technology, what tools I can use, and how I can implement it. Here’s what I found:

For testing, I used the [Star Wars GraphQL API](https://studio.apollographql.com/public/star-wars-swapi/variant/current/home) with `AllFilmsQuery`:

```graphql
query AllFilmsQuery {
  allFilms {
    films {
      title
      director
      created
      producers
      releaseDate
    }
  }
}
```

I requested `allFilms` with `title`, `director`, `created`, `producers`, and `releaseDate` information.

### When to Apply This Technology
- The best way to use GraphQL is when you have multiple platform applications such as web, mobile, and TV, and each client needs to request the specific data they require. 
- It's also beneficial when you need to fetch complex, nested, or related data from multiple sources.

### What Tools to Use
- [GraphiQL Live Demo](https://graphql.github.io/swapi-graphql) - A graphical interactive in-browser GraphQL IDE.
- [Apollo iOS Docs](https://www.apollographql.com/docs/ios/) - Apollo iOS is an open-source GraphQL client for native client applications, written in Swift.
- [Apollo iOS Code Generation CLI](https://www.apollographql.com/docs/ios/code-generation/codegen-cli/) - A CLI to generate boilerplate code.

### Caveats
Before diving into implementation, I would like to highlight a few nuances.

You need to add [`NetworkInterceptorProvider` and `AuthorizationInterceptor`](https://www.apollographql.com/docs/ios/tutorial/tutorial-authenticate-operations/#create-the-authorizationinterceptor) to authenticate your operations. Without them, you won't be able to access the data on your server.

### How to Implement It
#### Client 
``` swift 
class NetworkInterceptorProvider: DefaultInterceptorProvider {

    override func interceptors<Operation>(for operation: Operation) -> [ApolloInterceptor] where Operation : GraphQLOperation {
        var interceptors = super.interceptors(for: operation)
        interceptors.insert(AuthorizationInterceptor(), at: 0)
        return interceptors
    }

}
```

``` swift 
class AuthorizationInterceptor: ApolloInterceptor {

    let id: String = UUID().uuidString

    func interceptAsync<Operation>(
        chain: RequestChain,
        request: HTTPRequest<Operation>,
        response: HTTPResponse<Operation>?,
        completion: @escaping (Result<GraphQLResult<Operation.Data>, Error>) -> Void
    ) where Operation : GraphQLOperation {
        chain.proceedAsync(request: request,
                           response: response,
                           completion: completion)
    }

}
``` 

``` swift 
private let apollo: ApolloClient = {
    let client = URLSessionClient()
    let cache = InMemoryNormalizedCache()
    let store = ApolloStore(cache: cache)
    let provider = NetworkInterceptorProvider(client: client, store: store)
    let url = URL(string: "https://swapi-graphql.netlify.app/.netlify/functions/index")!
    let transport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)
    return ApolloClient(networkTransport: transport, store: store)
}()
```

#### UI
``` swift 
import SwiftUI
import Apollo
import StarWarsAPI

struct ContentView: View {

    @State private var list: [AllFilmsQuery.Data.AllFilms.Film?] = []

    var body: some View {
        VStack {
            ForEach(list, id: \.self) { row in
                Text(row?.title ?? "")
            }
        }.onAppear(perform: {
            apollo.fetch(query: AllFilmsQuery()) { result in
                switch result {
                case .success(let response):
                    self.list = response.data?.allFilms?.films ?? []
                case .failure(let error):
                    print(error)
                }
            }
        })
        .padding()
    }
    
}

#Preview {
    ContentView()
}
```

#### References
- You can find more detailed information in the amazing post [Unleashing the Power of GraphQL in Your iOS App](https://vincenzopascarella.medium.com/unleashing-the-power-of-graphql-in-your-ios-app-67e0f768cb14) where you can find step-by-step instructions on how to use Apollo iOS and GraphQL. 
- I also recommend watching the video [“Adopting GraphQL" by Carola Nitz](https://www.youtube.com/watch?v=ndBJ0Gi7RpI&ab_channel=Swiftable), hosted by [Swiftable](https://www.youtube.com/@swiftable).

#### Thank you for reading! 😊
