+++
title = 'Integration testing for SwiftUI views'
date = 2024-05-10T06:20:50+03:00
tags = ["Integration testing", "SwiftUI"]
draft = false
+++

### Introduction 
I have been looking for information about implementation details of integration testing. 
I found a lot of information, but it was theoretical and all information looked the same. 
I did not find a meaningful example, so I tried to come up with my own definition and sample.

> Integration testing means testing the behavior between modules or views to ensure they work as expected after user actions.

### There are two ways of conducting integration testing:
- The first is by unit tests, where you try to test the flowing data between view models.
- The second is UI tests, where you try to test if the UI items exist and navigation works correctly.

I will focus on testing the flowing data between view models.

### Sample: List View and Detail View

When you `tap` on a `row` in the `list`, you expect that:
- The `selected item` will `pass` to the `detail view`.
- The `detail view` will `receive` this `item`.
- The `selected item` will be `equal` to the `passed item`.

``` swift 
import XCTest
@testable import IntegrationTesting

final class IntegrationTestingTests: XCTestCase {

    func testItemSelection() {
        let viewModel = MockItemListViewModel(service: MockItemService())
        let selectedIndex = 1
        let itemListView = ItemListView(viewModel: viewModel,
                                        didSelectItem: {
            let itemDetailView = ItemDetailView(selectedItem: $0)
            XCTAssertEqual(itemDetailView.selectedItem?.name, viewModel.items[selectedIndex].name)
        })
        itemListView.didSelectItem(viewModel.items[selectedIndex])
    }

}
```

### Helpers
``` swift 
import Combine

final class MockItemListViewModel: ItemListViewModel {
    var selectedItem: Item? = nil
}

class ItemListViewModel: ObservableObject {
    @Published var items: [Item] = []
    private var cancellables: Set<AnyCancellable> = []
    private let service: ItemService

    init(service: ItemService) {
        self.service = service
        fetchItems()
    }

    func fetchItems() {
        service.fetchItems()
            .sink { completion in
                // Handle error or completion if necessary
            } receiveValue: { [weak self] items in
                self?.items = items
            }
            .store(in: &cancellables)
    }
}
``` 

``` swift 
import Combine

protocol ItemService {
    func fetchItems() -> AnyPublisher<[Item], Error>
}

final class MockItemService: ItemService {
    func fetchItems() -> AnyPublisher<[Item], Error> {        
        return Just([Item(name: "Item 1"), Item(name: "Item 2")])
            .setFailureType(to: Error.self)
            .eraseToAnyPublisher()
    }
}
```

``` swift 
import SwiftUI

struct ItemListView: View {
    @ObservedObject var viewModel: ItemListViewModel
    var didSelectItem: (Item) -> Void 

    var body: some View {
        List(viewModel.items) { item in
            Button(action: {
                didSelectItem(item) 
            }) {
                Text(item.name)
            }
        }.accessibilityIdentifier("ItemListView")
    }
}
```
``` swift 
import SwiftUI

struct ItemDetailView: View {
    var selectedItem: Item?

    var body: some View {
        if let item = selectedItem {
            Text("Item Detail: \(item.name)")
        } else {
            Text("No item selected")
        }
    }
}
```

#### Thank you for reading! 😊
