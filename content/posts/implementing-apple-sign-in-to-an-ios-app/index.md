+++
title = 'Implementing Apple Sign-In to an iOS App'
date = 2024-07-18T07:26:13+03:00
tags = ["Implementing", "Apple Sign-In", "iOS", "app", "Swift", "Authentication Services"]
draft = false
+++

### Introduction 
The Apple Sign-In feature is very helpful and offers users login functionality with one click. It could be highly beneficial from a business perspective to attract more potential customers by providing easy and secure access to application functionality. In this article, I will focus on how to implement Apple Sign-In.

Before implementation, let’s set up the necessary options to be able to run the app without errors.
- Add `Sign in with Apple` Capability to the project
![alt image](images/0.png#center)

> :memo: Before testing on the simulator, you need to be signed in to an account with enabled two-factor authentication.

### Implementing Apple Sign-In
Apple offers a built-in solution that uses the [Authentication Services](https://developer.apple.com/documentation/authenticationservices) framework with [ASAuthorizationAppleIDButton](https://developer.apple.com/documentation/authenticationservices/asauthorizationappleidbutton).

#### First Step 
The first step is to set up the Sign-In button.
```swift
func setupAppleIDButton() {
    let authButton = ASAuthorizationAppleIDButton()
    authButton.frame = CGRect(origin: view.center, size: CGSize(width: 128, height: 56))
    authButton.addTarget(self, action: #selector(handleAuthorizationAppleIDButtonPress), for: .touchUpInside)
    view.addSubview(authButton)
}
```

> :bulb: `ASAuthorizationAppleIDButton` - A control that enables users to initiate the Sign In with Apple flow.

#### Second Step 
The second step is to handle the authorization Apple ID button press.
```swift
@objc
func handleAuthorizationAppleIDButtonPress() {
    let appleIdProvider = ASAuthorizationAppleIDProvider()
    let request = appleIdProvider.createRequest()
    request.requestedScopes = [.fullName, .email]

    let authorizationController = ASAuthorizationController(authorizationRequests: [request])
    authorizationController.delegate = self
    authorizationController.presentationContextProvider = self
    authorizationController.performRequests()
}
```

> :bulb: [`ASAuthorizationAppleIDProvider`](https://developer.apple.com/documentation/authenticationservices/asauthorizationappleidprovider) - A mechanism for generating requests to authenticate users based on their Apple ID.

> :bulb: [`ASAuthorizationController`](https://developer.apple.com/documentation/authenticationservices/asauthorizationcontroller) - A controller that manages authorization requests that a provider creates.

#### Third Step 
The third step is to implement `authorizationController.delegate` and `authorizationController.presentationContextProvider`.
```swift
// MARK: - ASAuthorizationControllerDelegate
extension LoginViewController: ASAuthorizationControllerDelegate {
    /// - Tag: did_complete_authorization
    public func authorizationController(controller _: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization) {
        switch authorization.credential {
        case let appleIDCredential as ASAuthorizationAppleIDCredential:

            // Create an account in your system.
            let userIdentifier = appleIDCredential.user
            let fullName = appleIDCredential.fullName
            let email = appleIDCredential.email

        case let passwordCredential as ASPasswordCredential:

            // Sign in using an existing iCloud Keychain credential.
            let username = passwordCredential.user
            let password = passwordCredential.password

        default:
            break
        }
    }

    /// - Tag: did_complete_error
    public func authorizationController(controller _: ASAuthorizationController, didCompleteWithError error: any Error) {
        // Handle error.
        print(error)
    }
}
```

> :bulb: [`ASAuthorizationControllerDelegate`](https://developer.apple.com/documentation/authenticationservices/asauthorizationcontrollerdelegate) - An interface for providing information about the outcome of an authorization request.

```swift
// MARK: - ASAuthorizationControllerPresentationContextProviding
extension LoginViewController: ASAuthorizationControllerPresentationContextProviding {
    /// - Tag: provide_presentation_anchor
    public func presentationAnchor(for _: ASAuthorizationController) -> ASPresentationAnchor {
        view.window!
    }
}
```

> :bulb: [`ASAuthorizationControllerPresentationContextProviding`](https://developer.apple.com/documentation/authenticationservices/asauthorizationcontrollerpresentationcontextproviding) - An interface the controller uses to ask a delegate for a presentation context.

### Resources
[Implementing User Authentication with Sign In with Apple](https://developer.apple.com/documentation/sign_in_with_apple/implementing_user_authentication_with_sign_in_with_apple)

### Download Materials
[Download](https://docs-assets.developer.apple.com/published/d42a7d7e3c/ImplementingUserAuthenticationWithSignInWithApple.zip)

#### Thank you for reading! 😊
