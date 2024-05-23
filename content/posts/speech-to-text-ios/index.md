+++
title = 'Speech To Text iOS'
date = 2024-05-23T08:37:45+03:00
tags = ["speech to text", "iOS"]
draft = false
+++

### Introduction 
I always wanted an iOS app that would allow me to economize my time by converting speech to text. 
I know this option is built into the keyboard, but you first need to click the text field, then tap on the microphone, and finally speak. 
I wanted a one-click option with the possibility to integrate it into all my daily routines. Here is what I discovered:

![alt image](images/0.gif#center)

### First Step 
The first step is to request authorization to access the device's microphone using the `Privacy - Speech Recognition Usage Description` key and the `Privacy - Microphone Usage Description` key.

### Second Step
The second step is to add a `SpeechRecognizerService` that will detect and transcribe speech. It consists of `SFSpeechRecognizer`, `AVAudioSession`, and `AVAudioEngine`. Your transcribed speech will be stored in the `transcript` property.

```swift
import Foundation
import AVFoundation
import Speech

/// A helper for transcribing speech to text using SFSpeechRecognizer and AVAudioEngine.
actor SpeechRecognizerService: ObservableObject {
    enum RecognizerError: Error {
        case nilRecognizer
        case notAuthorizedToRecognize
        case notPermittedToRecord
        case recognizerIsUnavailable

        var message: String {
            switch self {
            case .nilRecognizer: return "Can't initialize speech recognizer"
            case .notAuthorizedToRecognize: return "Not authorized to recognize speech"
            case .notPermittedToRecord: return "Not permitted to record audio"
            case .recognizerIsUnavailable: return "Recognizer is unavailable"
            }
        }
    }

    @MainActor @Published private(set) var transcript: String = ""

    private var audioEngine: AVAudioEngine?
    private var request: SFSpeechAudioBufferRecognitionRequest?
    private var task: SFSpeechRecognitionTask?
    private let recognizer: SFSpeechRecognizer?

    /**
     Initializes a new speech recognizer. If this is the first time you've used the class, it
     requests access to the speech recognizer and the microphone.
     */
    init() {
        recognizer = SFSpeechRecognizer()
        guard recognizer != nil else {
            transcribe(RecognizerError.nilRecognizer)
            return
        }

        Task {
            do {
                guard await SFSpeechRecognizer.hasAuthorizationToRecognize() else {
                    throw RecognizerError.notAuthorizedToRecognize
                }
                guard await AVAudioSession.sharedInstance().hasPermissionToRecord() else {
                    throw RecognizerError.notPermittedToRecord
                }
            } catch {
                transcribe(error)
            }
        }
    }

    @MainActor func startTranscribing() {
        Task {
            await transcribe()
        }
    }

    @MainActor func resetTranscript() {
        Task {
            await reset()
        }
    }

    @MainActor func stopTranscribing() {
        Task {
            await reset()
        }
    }

}

private extension SpeechRecognizerService {

    /**
     Begin transcribing audio.

     Creates a `SFSpeechRecognitionTask` that transcribes speech to text until you call `stopTranscribing()`.
     The resulting transcription is continuously written to the published `transcript` property.
     */
    func transcribe() {
        guard let recognizer, recognizer.isAvailable else {
            self.transcribe(RecognizerError.recognizerIsUnavailable)
            return
        }

        do {
            let (audioEngine, request) = try Self.prepareEngine()
            self.audioEngine = audioEngine
            self.request = request
            self.task = recognizer.recognitionTask(with: request, resultHandler: { [weak self] result, error in
                self?.recognitionHandler(audioEngine: audioEngine, result: result, error: error)
            })
        } catch {
            self.reset()
            self.transcribe(error)
        }
    }

    /// Reset the speech recognizer.
    func reset() {
        task?.cancel()
        audioEngine?.stop()
        audioEngine = nil
        request = nil
        task = nil
    }

    static func prepareEngine() throws -> (AVAudioEngine, SFSpeechAudioBufferRecognitionRequest) {
        let audioEngine = AVAudioEngine()

        let request = SFSpeechAudioBufferRecognitionRequest()
        request.shouldReportPartialResults = true

        let audioSession = AVAudioSession.sharedInstance()
        try audioSession.setCategory(.playAndRecord, mode: .measurement, options: .duckOthers)
        try audioSession.setActive(true, options: .notifyOthersOnDeactivation)
        let inputNode = audioEngine.inputNode

        let recordingFormat = inputNode.outputFormat(forBus: 0)
        inputNode.installTap(onBus: 0, bufferSize: 1024, format: recordingFormat) { (buffer: AVAudioPCMBuffer, when: AVAudioTime) in
            request.append(buffer)
        }
        audioEngine.prepare()
        try audioEngine.start()

        return (audioEngine, request)
    }

    nonisolated func recognitionHandler(audioEngine: AVAudioEngine, result: SFSpeechRecognitionResult?, error: Error?) {
        let receivedFinalResult = result?.isFinal ?? false
        let receivedError = error != nil

        if receivedFinalResult || receivedError {
            audioEngine.stop()
            audioEngine.inputNode.removeTap(onBus: 0)
        }

        if let result {
            transcribe(result.bestTranscription.formattedString)
        }
    }

    nonisolated func transcribe(_ message: String) {
        Task { @MainActor in
            transcript = message
        }
    }

    nonisolated func transcribe(_ error: Error) {
        var errorMessage = ""
        if let error = error as? RecognizerError {
            errorMessage += error.message
        } else {
            errorMessage += error.localizedDescription
        }
        Task { @MainActor [errorMessage] in
            transcript = "<< \(errorMessage) >>"
        }
    }

}

extension SFSpeechRecognizer {
    static func hasAuthorizationToRecognize() async -> Bool {
        await withCheckedContinuation { continuation in
            requestAuthorization { status in
                continuation.resume(returning: status == .authorized)
            }
        }
    }
}

extension AVAudioSession {
    func hasPermissionToRecord() async -> Bool {
        await withCheckedContinuation { continuation in
            requestRecordPermission { authorized in
                continuation.resume(returning: authorized)
            }
        }
    }
}

```

### Third Step 
The third step is to integrate speech recognition.

```swift
import SwiftUI

struct ContentView: View {

    @StateObject private var speechRecognizerService: SpeechRecognizerService = SpeechRecognizerService()

    var body: some View {
        ScrollView {
            VStack {
                Button("Start") {
                    speechRecognizerService.startTranscribing()
                }
                Divider()
                Button("Stop") {
                    speechRecognizerService.stopTranscribing()
                }
                Divider()
                Text(speechRecognizerService.transcript)
            }
        }
        .padding()
    }
}

#Preview {
    ContentView()
}
```

#### Thank you for reading! 😊
