+++
title = 'Securing user data with Keychain, Touch ID, and Face ID'
date = 2024-05-13T07:31:49+03:00
tags = ["security", "Securing", "user data", "Keychain", "Touch ID", "Face ID"]
draft = false
+++

### Introduction
I was eager to learn about securing user data using `Keychain` and `biometric authentication`. Here are a few steps I found:

### Caveats 
You can test accessing Keychain data using Touch ID and Face ID only on a real device.

### First Step 
The first step is to add the `Privacy - Face ID Usage Description` key to your `Info.plist`. Without it, you would not be able to retrieve data from `Keychain` using `Face ID`.

### Second Step 
The second step would be to add the `addCredentials` method to be able to save user data to `Keychain`.
``` swift 
/// Stores credentials for the given server.
func addCredentials(_ credentials: Credentials, server: String) throws {
    // Use the username as the account, and get the password as data.
    let account = credentials.username
    let password = credentials.password.data(using: String.Encoding.utf8)!

    // Create an access control instance that dictates how the item can be read later.
    let access = SecAccessControlCreateWithFlags(nil, // Use the default allocator.
                                                 kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly,
                                                 .userPresence,
                                                 nil) // Ignore any error.

    // Allow a device unlock in the last 10 seconds to be used to get at keychain items.
    let context = LAContext()
    context.touchIDAuthenticationAllowableReuseDuration = 10

    // Build the query for use in the add operation.
    let query: [String: Any] = [kSecClass as String: kSecClassInternetPassword,
                                kSecAttrAccount as String: account,
                                kSecAttrServer as String: server,
                                kSecAttrAccessControl as String: access as Any,
                                kSecUseAuthenticationContext as String: context,
                                kSecValueData as String: password]

    let status = SecItemAdd(query as CFDictionary, nil)
    guard status == errSecSuccess else { throw KeychainError(status: status) }
}
```

### Third Step 
The third step is to add the `readCredentials` method to be capable of retrieving user data from `Keychain`.
``` swift 
/// Reads the stored credentials for the given server.
func readCredentials(server: String) throws -> Credentials {
    let context = LAContext()
    context.localizedReason = "Access your password on the keychain"
    let query: [String: Any] = [kSecClass as String: kSecClassInternetPassword,
                                kSecAttrServer as String: server,
                                kSecMatchLimit as String: kSecMatchLimitOne,
                                kSecReturnAttributes as String: true,
                                kSecUseAuthenticationContext as String: context,
                                kSecReturnData as String: true]

    var item: CFTypeRef?
    let status = SecItemCopyMatching(query as CFDictionary, &item)
    guard status == errSecSuccess else { throw KeychainError(status: status) }

    guard let existingItem = item as? [String: Any],
          let passwordData = existingItem[kSecValueData as String] as? Data,
          let password = String(data: passwordData, encoding: String.Encoding.utf8),
          let account = existingItem[kSecAttrAccount as String] as? String
    else {
        throw KeychainError(status: errSecInternalError)
    }

    return Credentials(username: account, password: password)
}
```

### Fourth Step 
The fourth step is to add the `deleteCredentials` method to have the ability to delete user data from `Keychain`.
``` swift 
/// Deletes credentials for the given server.
func deleteCredentials(server: String) throws {
    let query: [String: Any] = [kSecClass as String: kSecClassInternetPassword,
                                kSecAttrServer as String: server]

    let status = SecItemDelete(query as CFDictionary)
    guard status == errSecSuccess else { throw KeychainError(status: status) }
}
```

#### UI
``` swift 
import SwiftUI
import LocalAuthentication

struct ContentView: View {

    @State private var status: String = ""

    var body: some View {
        VStack {
            ForEach(Command.allCases) { command in
                Button(command.rawValue) {
                    switch command {
                    case .add:
                        // Normally, username and password would come from the user interface.
                        let credentials = Credentials(username: "appleseed", password: "1234")
                        do {
                            try addCredentials(credentials, server: server)
                            status = statusMessage(.add, nil)
                        } catch {
                            status = error.localizedDescription
                        }
                    case .read:
                        do {
                            status = statusMessage(.read, try readCredentials(server: server))
                        } catch {
                            status = error.localizedDescription
                        }
                    case .delete:
                        do {
                            try deleteCredentials(server: server)
                            status = statusMessage(.delete, nil)
                        } catch {
                            status = error.localizedDescription
                        }
                    }
                }
                if command != .delete {
                    Spacer()
                }
            }
            Spacer()
            Text(status)
        }
    }

}
```

#### Helpers
``` swift 
enum Command: String, CaseIterable, Identifiable {

    var id: String { rawValue }

    case add
    case read
    case delete

}

/// The username and password that we want to store or read.
struct Credentials {
    var username: String
    var password: String
}

/// Keychain errors we might encounter.
struct KeychainError: Error {
    var status: OSStatus

    var localizedDescription: String {
        return SecCopyErrorMessageString(status, nil) as String? ?? "Unknown error."
    }
}

/// The server we are accessing with the credentials.
let server = "www.example.com"

func statusMessage(_ command: Command, _ credentials: Credentials? = nil) -> String {
    switch command {
    case .add:
        return "Added credentials."
    case .read:
        return "Read credentials: \(credentials!.username)/\(credentials!.password)"
    case .delete:
        return "Deleted credentials."
    }
}
```

You can find more detailed information and project details in the [Apple Developer Documentation](https://developer.apple.com/documentation/localauthentication/accessing-keychain-items-with-face-id-or-touch-id).

#### Thank you for reading! 😊
