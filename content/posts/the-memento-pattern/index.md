+++
title = 'The Memento Pattern'
date = 2024-03-22T08:29:30+03:00
tags = ["chain memento pattern", "design patterns", "swift", "oop"]
draft = false
+++

### What is a Memento Pattern?
> The Memento Pattern helps return an object to one of its previous states; for instance, if the user requests an “undo” operation.

![alt image](images/0.jpg#center)
[Source](https://www.learncsdesign.com/learn-the-memento-design-pattern/)

### What problems does it solve?

The Memento Pattern helps solve following problems:

- **Undo/Redo Functionality**: Memento allows you to capture an object’s state at a specific point in time and store it externally. This enables you to implement undo/redo functionality by restoring the object to its previous state.
- **Checkpointing**: In applications where users need to save progress or create checkpoints (such as in games or long processes), the Memento Pattern allows you to save the state of an object at various intervals so that users can return to those points later.

### Real-world code example
``` swift 
// Memento: Represents the state of the TextEditor
struct TextEditorMemento {
    let text: String
}

// Originator: Creates and stores states in Memento objects
class TextEditor {
    private var text: String = ""
    
    func setText(_ text: String) {
        self.text = text
    }
    
    func getText() -> String {
        return text
    }
    
    func save() -> TextEditorMemento {
        return TextEditorMemento(text: text)
    }
    
    func restore(fromMemento memento: TextEditorMemento) {
        self.text = memento.text
    }
}

// Caretaker: Manages the mementos
class TextEditorHistory {
    private var history: [TextEditorMemento] = []
    private let editor: TextEditor
    
    init(editor: TextEditor) {
        self.editor = editor
    }
    
    func save() {
        let snapshot = editor.save()
        history.append(snapshot)
    }
    
    func undo() {
        guard let lastSnapshot = history.popLast() else {
            print("Nothing to undo.")
            return
        }
        editor.restore(fromMemento: lastSnapshot)
    }
    
    func printHistory() {
        print("Text Editor History:")
        for (index, snapshot) in history.enumerated() {
            print("Step \(index + 1): \(snapshot.text)")
        }
        print("Current text: \(editor.getText())")
    }
}

// Example usage
let textEditor = TextEditor()
let history = TextEditorHistory(editor: textEditor)

textEditor.setText("Hello, World!")
history.save()

textEditor.setText("This is a Swift example.")
history.save()

textEditor.setText("Using Memento Pattern.")
history.save()

history.printHistory() 

history.undo() 

print("After Undo:")
history.printHistory() 
``` 

#### Thank you for reading! 😊
