+++
title = 'Implementing In-App Purchases to SwiftUI app using StoreKit 2'
date = 2024-05-12T07:57:51+03:00
tags = ["Implementing", "non-consumable", "In-App Purchases", "SwiftUI", "app", "StoreKit", "StoreKit 2"]
draft = false
+++

### Introduction
I was wondering how to add in-app purchases to my app. I chose [non-consumable in-app purchase](https://support.apple.com/en-us/108813#:~:text=What%20is%20a%20non%2Dconsumable%20in%2Dapp%20purchase%3F) because you can pay one time for this item. Here are a few steps on how I did it.

![alt image](images/0.gif#center)

### First Step
[Set up In-App Purchases](https://learn.buildfire.com/en/articles/2534672-how-to-create-a-single-in-app-purchase-for-ios) for your app in [App Store Connect](https://appstoreconnect.apple.com/login) account or add a `.storekit` configuration file and start from there. If you've already set up In-App Purchases in your account, you can sync the `StoreKit config` with that data.

### Caveats
Be aware that if you choose to set up the `StoreKit configuration` file first, you will not find that file in the `Xcode 15.3.0 iOS template`. Instead, switch to `macOS` and search for it there.

### Second Step
`fetchProducts` by identifiers to retrieve data and by using an [`SKProductsRequestDelegate`](https://developer.apple.com/documentation/storekit/skproductsrequestdelegate) to receive and display products.

``` swift 
func fetchProducts() {
    let productIDs: Set<String> = ["com.remove.ads.nonconsumable"]

    let request = SKProductsRequest(productIdentifiers: productIDs)
    request.delegate = self
    request.start()
}
```
``` swift 
// MARK: - SKProductsRequestDelegate
extension ViewModel: SKProductsRequestDelegate {

    func productsRequest(_ request: SKProductsRequest, didReceive response: SKProductsResponse) {
        DispatchQueue.main.async {
            self.products = response.products.map { product in
                Product(id: product.productIdentifier, title: product.localizedTitle, price: product.price.doubleValue)
            }

            for product in response.products {
                self.productsMap[product.productIdentifier] = product
            }
        }
    }

}
```

### Third Step
Add a `purchaseProduct` method and connect the `view model` with the `UI`.
``` swift 
func purchaseProduct(product: Product) {
    guard SKPaymentQueue.canMakePayments() else {
        errorMessage = "In-app purchases are disabled on this device."
        return
    }

    guard let skProduct = productsMap[product.id] else {
        errorMessage = "Product information not available."
        return
    }

    let payment = SKPayment(product: skProduct)
    SKPaymentQueue.default().add(payment)
}
```
#### UI
``` swift  
var body: some View {
    ZStack(alignment: .top) {
        VStack(spacing: 10) {
            Text("With StoreKit 2")
                .padding()
            ForEach(viewModel.products) { product in
                Button {                    
                    viewModel.purchaseProduct(product: product)                        
                } label: {
                    Text(product.title)
                }
            }
        }
        .padding()
    }
    .onAppear(perform: {
        viewModel.fetchProducts()
    })
}
```

#### ViewModel
``` swift
final class ViewModel: NSObject, ObservableObject {

    @Published var products: [Product] = []
    private var productsMap: [String: SKProduct] = [:]

    @Published var errorMessage: String?

    func fetchProducts() {
        let productIDs: Set<String> = ["com.remove.ads.nonconsumable"]

        let request = SKProductsRequest(productIdentifiers: productIDs)
        request.delegate = self
        request.start()
    }

    func purchaseProduct(product: Product) {
        guard SKPaymentQueue.canMakePayments() else {
            errorMessage = "In-app purchases are disabled on this device."
            return
        }

        guard let skProduct = productsMap[product.id] else {
            errorMessage = "Product information not available."
            return
        }

        let payment = SKPayment(product: skProduct)
        SKPaymentQueue.default().add(payment)
    }

}

// MARK: - SKProductsRequestDelegate
extension ViewModel: SKProductsRequestDelegate {

    func productsRequest(_ request: SKProductsRequest, didReceive response: SKProductsResponse) {
        DispatchQueue.main.async {
            self.products = response.products.map { product in
                Product(id: product.productIdentifier, title: product.localizedTitle, price: product.price.doubleValue)
            }

            for product in response.products {
                self.productsMap[product.productIdentifier] = product
            }
        }
    }

}
```

#### Helpers
``` swift
struct Product: Identifiable {
    let id: String
    let title: String
    let price: Double
}
```

#### Thank you for reading! 😊
