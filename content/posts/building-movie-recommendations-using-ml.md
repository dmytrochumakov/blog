+++
title = 'Building movie recommendations using ML'
date = 2024-05-01T10:29:44+03:00
tags = ["building", "movie", "recommendations", "ML", "SwiftUI"]
draft = false
+++

### Introduction
I was wondering about how to create movie recommendations, so I decided to take a closer look and find out more about this topic. This is what I found:

### First step:
You need to create a JSON file with the data that you will use to train the model and define the parameters for training the model. 

``` json
[
  {
    "title": "Avatar",
    "year": "2009",
    "rated": "PG-13",
    "released": "18 Dec 2009",
    "runtime": "162 min",
    "genre": "Action, Adventure, Fantasy",
    "director": "James Cameron",
    "writer": "James Cameron",
    "actors": "Sam Worthington, Zoe Saldana, Sigourney Weaver, Stephen Lang",
    "plot": "A paraplegic marine dispatched to the moon Pandora on a unique mission becomes torn between following his orders and protecting the world he feels is his home.",
    "language": "English, Spanish",
    "country": "USA, UK",
    "awards": "Won 3 Oscars. Another 80 wins & 121 nominations.",
    "poster": "https://ia.media-imdb.com/images/M/MV5BMTYwOTEwNjAzMl5BMl5BanBnXkFtZTcwODc5MTUwMw@@._V1_SX300.jpg",
    "metascore": "83",
    "imdbrating": "7.9",
    "imdbvotes": "890,617",
    "imdbid": "tt0499549",
    "type": "movie",
    "response": "True",
    "keywords": ["alien", "avatar", "fantasy world", "soldier", "battle"]
  },
]
```

When you have created the data, you can proceed to the next step.

### Creating a model with parameters 
Creating a model with parameters is essential for training. As an example, I chose the following parameters to create more realistic recommendations: `director`, `actors`, `language`, `country`, `metascore`, `IMDb rating`, and `IMDb votes`.

``` swift 
struct Movie: Decodable {
  var id: String {
      return imdbid
  }
  let title: String
  let keywords: [String]
  let director: String
  let actors: String
  let language: String
  let country: String
  let poster: String
  let metascore: String
  let imdbrating: String
  let imdbvotes: String
  let imdbid: String
}

extension Movie: Identifiable, TextImageProviding {
  var url: URL {
    return URL(string: poster)!
  }
}
```

When you have created the model, you can proceed to the next step: 

### Creating a Recommendations service
In this step, it's important to specify the ML model you'll utilize. 
I chose the `MLLinearRegressor` model because linear regression computes an output value for a given input value. 
I selected 'favorite' as the target column for this model to create predictions based on the films I like.

``` swift
import Foundation
import TabularData

#if canImport(CreateML)
import CreateML
#endif

final class RecommendationService {
  private let queue = DispatchQueue(label: "com.recommendation-service.queue", qos: .userInitiated)

  func computeRecommendations(basedOn items: [FavoriteWrapper<Movie>]) async throws -> [Movie] {
    return try await withCheckedThrowingContinuation { continuation in
      queue.async {
        #if targetEnvironment(simulator)
        continuation.resume(throwing: NSError(domain: "Simulator Not Supported", code: -1))
        #else
        let trainingData = items.filter {
          $0.isFavorite != nil
        }

        let trainingDataFrame = self.dataFrame(for: trainingData)

        let testData = items
        let testDataFrame = self.dataFrame(for: testData)

        do {
          let regressor = try MLLinearRegressor(trainingData: trainingDataFrame, targetColumn: "favorite")

          let predictionsColumn = (try regressor.predictions(from: testDataFrame)).compactMap { value in
            value as? Double
          }

          let sorted = zip(testData, predictionsColumn)
            .sorted { lhs, rhs -> Bool in
              lhs.1 > rhs.1
            }
            .filter {
              $0.1 > 0
            }
            .prefix(10)

          print(sorted.map(\.1))

          let result = sorted.map(\.0.model)

          continuation.resume(returning: result)
        } catch {
          continuation.resume(throwing: error)
        }
        #endif
      }
    }
  }

  private func dataFrame(for data: [FavoriteWrapper<Movie>]) -> DataFrame {
    var dataFrame = DataFrame()

    dataFrame.append(
      column: Column(name: "keywords", contents: data.flatMap(\.model.keywords).joined(separator: ", "))
    )

    dataFrame.append(
      column: Column(name: "director", contents: data.map(\.model.director))
    )

    dataFrame.append(
      column: Column(name: "actors", contents: data.map(\.model.actors))
    )

    dataFrame.append(
      column: Column(name: "language", contents: data.map(\.model.language))
    )

    dataFrame.append(
      column: Column(name: "country", contents: data.map(\.model.country))
    )

    dataFrame.append(
      column: Column<Int>(
        name: "metascore",
        contents: data.map {
          return Int($0.model.metascore)
        }
      )
    )

    dataFrame.append(
      column: Column<Double>(
        name: "imdbrating",
        contents: data.map {
          return Double($0.model.imdbrating)
        }
      )
    )

    dataFrame.append(
      column: Column(name: "imdbvotes", contents: data.map(\.model.imdbvotes))
    )

    dataFrame.append(
      column: Column<Int>(
        name: "favorite",
        contents: data.map {
          if let isFavorite = $0.isFavorite {
            return isFavorite ? 1 : -1
          } else {
            return 0
          }
        }
      )
    )

    return dataFrame
  }
}
```

Once you've finished with the ML part, proceed to the next step: 

### Creating a ViewModel to assemble all components
At this stage, you handle user input and recompute recommendations based on user input.

``` swift 
@MainActor
final class MainViewModel: ObservableObject {
  private var allMovies: [FavoriteWrapper<Movie>] = []
  @Published private(set) var movies: [Movie] = []
  @Published private(set) var recommendations: [Movie] = []

  private let recommendationService: RecommendationService
  private var recommendationsTask: Task<Void, Never>?

  init(recommendationService: RecommendationService = RecommendationService()) {
    self.recommendationService = recommendationService
  }

  func loadAllMovies() async {
    guard let url = Bundle.main.url(forResource: "movies", withExtension: "json") else {
      return
    }

    do {
      let data = try Data(contentsOf: url)
      allMovies = (try JSONDecoder().decode([Movie].self, from: data)).shuffled().map {
        FavoriteWrapper(model: $0)
      }
      movies = allMovies.map(\.model)
    } catch {
      print(error.localizedDescription)
    }
  }

  func didRemove(_ item: Movie, isLiked: Bool) {
    movies.removeAll { $0.id == item.id }
    if let index = allMovies.firstIndex(where: { $0.model.id == item.id }) {
      allMovies[index] = FavoriteWrapper(model: item, isFavorite: isLiked)
    }

    recommendationsTask?.cancel()
    recommendationsTask = Task {
      do {
        let result = try await recommendationService.computeRecommendations(basedOn: allMovies)
        if !Task.isCancelled {
          recommendations = result
        }
      } catch {
        print(error.localizedDescription)
      }
    }
  }

  func resetUserChoices() {
    movies = allMovies.map(\.model)
    recommendations = []
  }
}
```

### The final step 
The final step is to create the UI and connect it with the ViewModel.

``` swift
import SwiftUI

struct ContentView: View {
  @StateObject private var viewModel: MainViewModel

  init() {
    _viewModel = StateObject(wrappedValue: MainViewModel())
  }

  var body: some View {
    NavigationView {
      ScrollView {
        VStack(alignment: .leading) {
          SectionTitleView(text: "Swipe to Like or Dislike")

          if viewModel.movies.isEmpty {
            HStack {
              Spacer()

              VStack {
                Text("All Done!")
                  .multilineTextAlignment(.center)
                  .font(.callout)
                  .foregroundColor(.secondary)
                Button("Try Again") {
                  withAnimation {
                    viewModel.resetUserChoices()
                  }
                }
                .font(.headline)
                .buttonStyle(.borderedProminent)
              }

              Spacer()
            }
            .padding(.horizontal)
            .padding(.vertical, 32)
          } else {
            CardsStackView(models: viewModel.movies) { item, isLiked in
              withAnimation(.spring()) {
                viewModel.didRemove(item, isLiked: isLiked)
              }
            }
            .zIndex(1)
          }

          RecommendationsView(recommendations: viewModel.recommendations)
        }
      }
      .navigationTitle("Tmovie´inder!")
      .task {
        await viewModel.loadAllMovies()
      }
    }
    .navigationViewStyle(.stack)
  }
}
```

### Resources
- 🔗 [GitHub project](https://github.com/dmytrochumakov/movie-recommendations-using-ML)
- 📥 [Download materials](https://github.com/dmytrochumakov/movie-recommendations-using-ML/archive/refs/heads/main.zip)

#### Thank you for reading! 😊
