+++
title = 'UI testing SwiftUI views using XCTest Framework'
date = 2024-05-08T06:34:32+03:00
tags = ["UI testing", "SwiftUI", "XCTest", "To-Do", "app"]
draft = false
+++

### Introduction
After spending some time developing my personal iOS app, I found myself in a position where I needed to add UI tests to my application. 
The reason behind this decision was the necessity to change the architecture to make it more scalable. 
However, this task proved to be challenging due to certain parts of the code being tightly coupled. 
The situation was quite frustrating. 
To address this problem, I decided to incorporate UI tests that could help identify issues during the refactoring process.

### Caveats
- Make sure to run UI tests from a generated bundle specifically designed for UI testing. If you attempt to test the UI using a bundle intended for Unit tests, you will consistently encounter the error: `No target application path specified via test configuration: <XCTestConfiguration: 0x102b051f0>`. 
- Also, don't forget to hide the keyboard when necessary. If you need to tap on the `tab bar` and forget to close it, the operation will not succeed because it won't be able to locate the `tab bar` button. To resolve this, simply add the following code:
```swift
app.buttons["Return"].tap()
```

### Implementation
Here's an example of a To-Do list with functionalities for listing and adding tasks.

#### UI Tests
```swift
    var app: XCUIApplication!

    override func setUpWithError() throws {
        continueAfterFailure = false
        app = XCUIApplication()
        app.launch()
    }

    func testAddTask() throws {
        let addTaskTab = app.tabBars.buttons["Add Task"]
        addTaskTab.tap()

        let textField = app.textFields["Enter task"]
        textField.tap()
        textField.typeText("New Task")

        // Dismiss the keyboard
        app.buttons["Return"].tap()

        let addTaskButton = app.buttons["AddTaskButton"]
        addTaskButton.tap()

        app.tabBars.buttons["Tasks"].tap()

        XCTAssertTrue(app.staticTexts["New Task"].exists)
    }
```

#### UI
```swift
struct ContentView: View {
    @StateObject var viewModel = TaskViewModel()

    var body: some View {
        TabView {
            TaskListView(viewModel: viewModel)
                .tabItem {
                    Image(systemName: "list.bullet")
                    Text("Tasks")
                }
            AddTaskView(viewModel: viewModel)
                .tabItem {
                    Image(systemName: "plus.circle")
                    Text("Add Task")
                }
        }
    }
}
```

```swift
struct TaskListView: View {
    @ObservedObject var viewModel: TaskViewModel

    var body: some View {
        NavigationView {
            List(viewModel.tasks) { task in
                Text(task.title)
            }
            .navigationBarTitle("Tasks")
        }
    }
}
```

```swift
struct AddTaskView: View {
    @ObservedObject var viewModel: TaskViewModel
    @State private var newTaskTitle = ""

    var body: some View {
        VStack {
            TextField("Enter task", text: $newTaskTitle)
                .padding()
            Button("Add Task") {
                viewModel.addTask(title: newTaskTitle)
                newTaskTitle = ""
            }
            .accessibilityIdentifier("AddTaskButton")
            .padding()
        }
        .navigationTitle("Add Task")
    }
}
```

```swift
final class TaskViewModel: ObservableObject {
    @Published var tasks: [Task] = []

    func addTask(title: String) {
        let newTask = Task(title: title)
        tasks.append(newTask)
    }
}
```

```swift
struct Task: Identifiable {
    let id = UUID()
    var title: String
    var isCompleted: Bool = false
}
```

#### Thank you for reading! 😊
