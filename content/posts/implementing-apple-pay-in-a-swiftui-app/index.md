+++
title = 'Implementing Apple Pay in a SwiftUI app'
date = 2024-05-11T07:25:24+03:00
tags = ["Implementing", "Apple Pay", "SwiftUI", "app", "MVVM"]
draft = false
+++

### Introduction
Sometime ago, I was working on a marketplace app, and I needed to add Apple Pay to make purchases more easily. Here are a few steps on how I did it:
![alt image](images/0.gif#center)

### First Step
- You need to add [Apple Pay](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/apple_pay/offering_apple_pay_in_your_app) capability to your project. 
- You will need to `Register a Merchant ID`. I will skip this step; you can find info by following this link [Setting up Apple Pay](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/apple_pay/setting_up_apple_pay).

### Second Step
- You will need to `import PassKit` and create [`PKPaymentRequest`](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/pkpaymentrequest) to interact with [`PKPaymentAuthorizationController`](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/pkpaymentauthorizationcontroller) and [`PKPaymentAuthorizationControllerDelegate`](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/pkpaymentauthorizationcontrollerdelegate).

``` swift 
  func initiateApplePay() {
        // Create payment request
        let paymentRequest = PKPaymentRequest()
        paymentRequest.merchantIdentifier = "your_merchant_identifier"
        paymentRequest.countryCode = "US"
        paymentRequest.currencyCode = "USD"
        paymentRequest.supportedNetworks = [.visa, .masterCard, .amex]
        paymentRequest.merchantCapabilities = .threeDSecure

        // Add payment items from cart
        for item in cartItems {
            let paymentItem = PKPaymentSummaryItem(label: item.name, amount: item.price)
            paymentRequest.paymentSummaryItems.append(paymentItem)
        }

        // Add total amount
        let totalItem = PKPaymentSummaryItem(label: "Total", amount: totalAmount)
        paymentRequest.paymentSummaryItems.append(totalItem)

        // Present Apple Pay sheet
        let paymentController = PKPaymentAuthorizationController(paymentRequest: paymentRequest)
        paymentController.delegate = self
        paymentController.present(completion: nil)
    }
```

### Third Step
Add UI and connect it with the view model.
#### UI
``` swift
import SwiftUI

struct ContentView: View {
    @StateObject private var viewModel = MarketplaceViewModel()

    var body: some View {
        VStack {
            List(viewModel.products) { product in
                HStack {
                    Text(product.name)
                    Spacer()
                    Text(product.price.stringValue + "$")
                    Button(viewModel.inCart(product: product) ? "" : "Add to cart") {
                        viewModel.addToCart(product: product)
                    }
                }
            }
            Text("Total: \(viewModel.totalAmount)")
            Button("Pay with Apple Pay") {
                viewModel.initiateApplePay()
            }
            .padding()
        }
    }
}

#Preview {
    ContentView()
}
```

#### ViewModel
``` swift 
final class MarketplaceViewModel: NSObject, ObservableObject {

    private var cartItems: [Product] = []
    @Published private(set) var totalAmount: NSDecimalNumber = 0.0
    @Published private(set) var products: [Product] = []

    func fetchProducts() {
        Task {
            self.products = await ProductService.getProducts()
        }
    }

    func inCart(product: Product) -> Bool {
        cartItems.contains(product)
    }

    override init() {
        super.init()
        fetchProducts()
    }

    private func calculateTotalAmount() {
        totalAmount = cartItems.reduce(0) { $0.adding($1.price) }
    }

    func addToCart(product: Product) {
        cartItems.append(product)
        calculateTotalAmount()
    }

    func initiateApplePay() {
        // Create payment request
        let paymentRequest = PKPaymentRequest()
        paymentRequest.merchantIdentifier = "your_merchant_identifier"
        paymentRequest.countryCode = "US"
        paymentRequest.currencyCode = "USD"
        paymentRequest.supportedNetworks = [.visa, .masterCard, .amex]
        paymentRequest.merchantCapabilities = .threeDSecure

        // Add payment items from cart
        for item in cartItems {
            let paymentItem = PKPaymentSummaryItem(label: item.name, amount: item.price)
            paymentRequest.paymentSummaryItems.append(paymentItem)
        }

        // Add total amount
        let totalItem = PKPaymentSummaryItem(label: "Total", amount: totalAmount)
        paymentRequest.paymentSummaryItems.append(totalItem)

        // Present Apple Pay sheet
        let paymentController = PKPaymentAuthorizationController(paymentRequest: paymentRequest)
        paymentController.delegate = self
        paymentController.present(completion: nil)
    }

}

// MARK: - PKPaymentAuthorizationControllerDelegate
extension MarketplaceViewModel: PKPaymentAuthorizationControllerDelegate {

    func paymentAuthorizationController(_ controller: PKPaymentAuthorizationController, didAuthorizePayment payment: PKPayment, handler completion: @escaping (PKPaymentAuthorizationResult) -> Void) {
        let paymentResult = PKPaymentAuthorizationResult(status: .success, errors: nil)
        completion(paymentResult)
    }

    func paymentAuthorizationControllerDidFinish(_ controller: PKPaymentAuthorizationController) {
        controller.dismiss(completion: nil)
    }

}
```

#### Helpers
``` swift
struct Product: Identifiable, Equatable {
    let id: UUID
    let name: String
    let price: NSDecimalNumber
}
``` 
``` swift
final class ProductService {
    static func getProducts() async -> [Product]  {
        let products: [Product] = [
            Product(id: UUID(), name: "Product 1", price: 10.0),
            Product(id: UUID(), name: "Product 2", price: 20.0),
            Product(id: UUID(), name: "Product 3", price: 15.0)
        ]
        return products
    }
}
```

#### Thank you for reading! 😊
