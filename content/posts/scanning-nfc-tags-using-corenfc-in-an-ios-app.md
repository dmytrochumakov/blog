+++
title = 'Scanning NFC tags using CoreNFC in an iOS app'
date = 2024-07-12T07:27:51+03:00
tags = ["Scanning", "NFC", "tags", "CoreNFC", "iOS", "app", "swift"]
draft = false
+++

### Introduction
I never had a chance to work with NFC (Near Field Communication), but I have always been curious to find out how it works. In this article, I will focus on scanning NFC tags using [`CoreNFC`](https://developer.apple.com/documentation/corenfc) with [`NFCNDEFReaderSession`](https://developer.apple.com/documentation/corenfc/nfcndefreadersession).

### Preparation 
Before we begin, let's add the necessary objects:
- `Near Field Communication Tag Reading` capability to the project.
- `Privacy - NFC Scan Usage Description` key to `Info.plist`.
- `Near Field Communication Tag Reader Session Formats` to the entitlements file.

### First Step 
The first step before starting scanning is to check if the device supports NFC reading by using the `NFCNDEFReaderSession.readingAvailable` property.

``` swift 
guard NFCNDEFReaderSession.readingAvailable else {
    let alertController = UIAlertController(
        title: "Scanning Not Supported",
        message: "This device doesn't support tag scanning.",
        preferredStyle: .alert
    )
    alertController.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
    self.present(alertController, animated: true, completion: nil)
    return
}
``` 

### Second Step 
The second step is to create an [`NFCNDEFReaderSession`](https://developer.apple.com/documentation/corenfc/nfcndefreadersession) object by calling the [`init(delegate:queue:invalidateAfterFirstRead:)`](https://developer.apple.com/documentation/corenfc/nfcndefreadersession/2882064-init) initializer method. Set up [`alertMessage`](https://developer.apple.com/documentation/corenfc/nfcreadersessionprotocol/2919987-alertmessage) to give users instructions while the phone is scanning NFC tags. Finally, call [`begin()`](https://developer.apple.com/documentation/corenfc/nfcreadersessionprotocol/2874112-begin) to start the reader session.

``` swift 
session = NFCNDEFReaderSession(delegate: self, queue: nil, invalidateAfterFirstRead: false)
session?.alertMessage = "Hold your iPhone near the item to learn more about it."
session?.begin()
``` 

### Third Step 
The third step is to implement [`NFCNDEFReaderSessionDelegate`](https://developer.apple.com/documentation/corenfc/nfcndefreadersessiondelegate) to be able to receive notifications from the reader session when it reads an NDEF message or becomes invalid due to ending the session or encountering an error.

``` swift 
// MARK: - NFCNDEFReaderSessionDelegate

/// - Tag: processingTagData
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    DispatchQueue.main.async {
        // Process detected NFCNDEFMessage objects.
        self.detectedMessages.append(contentsOf: messages)
        self.tableView.reloadData()
    }
}

/// - Tag: endScanning
func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
    // Check the invalidation reason from the returned error.
    if let readerError = error as? NFCReaderError {
        // Show an alert when the invalidation reason is not because of a
        // successful read during a single-tag read session, or because the
        // user canceled a multiple-tag read session from the UI or
        // programmatically using the invalidate method call.
        if (readerError.code != .readerSessionInvalidationErrorFirstNDEFTagRead)
            && (readerError.code != .readerSessionInvalidationErrorUserCanceled) {
            let alertController = UIAlertController(
                title: "Session Invalidated",
                message: error.localizedDescription,
                preferredStyle: .alert
            )
            alertController.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
            DispatchQueue.main.async {
                self.present(alertController, animated: true, completion: nil)
            }
        }
    }

    // To read new tags, a new session instance is required.
    self.session = nil
}
``` 

### Resources
You can find a more detailed article in the [Apple Developer Documentation](https://developer.apple.com/documentation/corenfc/building_an_nfc_tag-reader_app), where each step is explained in depth.

### Download Materials
[Download](https://docs-assets.developer.apple.com/published/3111a8e27d/BuildingAnNFCTagReaderApp.zip)

#### Thank you for reading! 😊
