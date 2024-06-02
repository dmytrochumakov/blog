+++
title = 'Accessibility iOS SwiftUI'
date = 2024-06-02T09:04:06+03:00
tags = ["Accessibility", "iOS", "SwiftUI"]
draft = false
+++

### Introduction
Previously, I posted about [Accessibility for UIKit](https://dmytros.blog/posts/accessibility-ios-uikit/). The idea behind this post is to find differences between UIKit Accessibility and SwiftUI features.

### Similarities:
Both UIKit and SwiftUI have [`accessibilityLabel`](https://developer.apple.com/documentation/objectivec/nsobject/1615181-accessibilitylabel) and [`accessibilityHints`](https://developer.apple.com/documentation/objectivec/nsobject/1615093-accessibilityhint) APIs.

### Differences:
- To use dynamic type for fonts, you need additional modifiers in SwiftUI.
  ``` swift
  struct ScaledFont: ViewModifier {
        @Environment(\.sizeCategory) var sizeCategory
        var name: String
        var size: Double

        func body(content: Content) -> some View {
            let scaledSize = UIFontMetrics.default.scaledValue(for: size)
            return content.font(.custom(name, size: scaledSize))
        }
    }

    extension View {
        func scaledFont(name: String, textSize size: Double) -> some View {
            return self.modifier(ScaledFont(name: name, size: size))
        }
    }
  ```
- To step over elements in a list, you need to add `.accessibilityElement(children: .combine)` to each row in SwiftUI.
  ``` swift
  struct FruitCaloriesCounter: View {
        var body: some View {
            NavigationView {
                List(fruits) { fruit in
                    FruitRow(fruit: fruit)
                        .accessibilityElement(children: .combine)
                }
                .navigationTitle("Fruits Calories Counter")
                .accessibilityElement(children: .contain)
                .navigationBarTitleDisplayMode(.inline)
            }
        }
    }
  ```
- In UIKit, you can insert and remove `accessibilityTraits` depending on the button state:
  ``` swift
  if button.isSelected {
      button.accessibilityTraits.insert(.header)
  } else {
      button.accessibilityTraits.remove(.header)
  }
  ```
- In SwiftUI, you need to pass `.accessibilityAddTraits(selected ? [.isSelected, .isButton] : .isButton)` to one modifier.
  ``` swift
  Button(action: {
    selected.toggle()
  }) {
    Image(systemName: selected ? "star.fill" : "star")
        .frame(width: 44, height: 44)
        .accessibilityLabel("favourite")
        .accessibilityHint(selected ? "removes favourite" : "makes favourite")
        .accessibilityAddTraits(selected ? [.isSelected, .isButton] : .isButton)
  }
  .buttonStyle(.plain)
  ```

### Complete Sample
``` swift
import SwiftUI

let fruits = [
    Fruit(name: "Apple", calories: 52),
    Fruit(name: "Banana", calories: 89),
    Fruit(name: "Orange", calories: 47),
    Fruit(name: "Pineapple", calories: 50),
    Fruit(name: "Strawberry", calories: 32)
]

struct Fruit: Identifiable {
    var id: String { name }
    let name: String
    let calories: Int
}

struct FruitRow: View {

    @State private var selected = false
    let fruit: Fruit

    var body: some View {
        HStack(spacing: 8) {
            VStack(alignment: .leading, spacing: 8) {
                Text(fruit.name)
                    .scaledFont(name: "Helvetica", textSize: 20)
                    .accessibilityLabel(fruit.name)
                Text("\(fruit.calories) per 100g")
                    .scaledFont(name: "Helvetica", textSize: 15)
                    .accessibilityLabel("\(fruit.calories) calories per 100 grams")
            }
            Spacer()
            Button(action: {
                selected.toggle()
            }) {
                Image(systemName: selected ? "star.fill" : "star")
                    .frame(width: 44, height: 44)
                    .accessibilityLabel("favourite")
                    .accessibilityHint(selected ? "removes favourite" : "makes favourite")
                    .accessibilityAddTraits(selected ? [.isSelected, .isButton] : .isButton)
            }
            .buttonStyle(.plain)
        }
    }

}

struct FruitCaloriesCounter: View {

    var body: some View {
        NavigationView {
            List(fruits) { fruit in
                FruitRow(fruit: fruit)
                    .accessibilityElement(children: .combine)
            }
            .navigationTitle("Fruits Calories Counter")
            .accessibilityElement(children: .contain)
            .navigationBarTitleDisplayMode(.inline)
        }
    }

}

struct ContentView: View {
    var body: some View {
        FruitCaloriesCounter()
    }
}

#Preview {
    ContentView()
}

struct ScaledFont: ViewModifier {
    @Environment(\.sizeCategory) var sizeCategory
    var name: String
    var size: Double

    func body(content: Content) -> some View {
        let scaledSize = UIFontMetrics.default.scaledValue(for: size)
        return content.font(.custom(name, size: scaledSize))
    }
}

extension View {
    func scaledFont(name: String, textSize size: Double) -> some View {
        return self.modifier(ScaledFont(name: name, size: size))
    }
}
```

#### Thank you for reading! 😊
