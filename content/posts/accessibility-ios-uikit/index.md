+++
title = 'Accessibility iOS UIKit'
date = 2024-05-30T07:04:49+03:00
tags = ["Accessibility", "iOS", "UIKit"]
draft = false
+++

### Introduction
I was curious to find out how to make an application more accessible. You can look at popular applications like YouTube or Netflix; they all have accessibility features like VoiceOver and dynamic fonts. I decided to create this example for a fruit calorie counter. It contains a list of fruits with the fruit name, fruit calories, and a favorite button.

![alt image](images/0.png#center)

### Where to Start
Before diving into implementation details, I want to highlight some information about the existing accessibility features and what I will be focusing on.

#### Existing Accessibility Features
Apple provides a variety of tools; here are the most important ones:
1. **VoiceOver**: A screen reader that allows visually impaired users to interact with their devices. 
2. **Dynamic Type**: Support for adjustable text sizes. Users can choose larger text sizes in the system settings, and apps should respond appropriately by scaling text and UI elements.
3. **Contrast and Color**: Ensuring sufficient contrast between text and background to make content readable for users with visual impairments. 
4. **Switch Control**: A feature that allows users with limited mobility to control their device using adaptive accessories. 
5. **AssistiveTouch**: A feature that helps users with physical disabilities perform actions that would otherwise require gestures. 
6. **Labels and Hints**: Using accessibility labels and hints to provide descriptive text for UI elements, which helps VoiceOver users understand what an element does.

### Focus Areas
I will be focusing on **Labels and Hints**, **VoiceOver**, and **Dynamic Type**.

### Implementation
#### First Step
The first step would be to mark `yourUIElement.isAccessibilityElement = true` to enable your component's visibility for VoiceOver.

#### Second Step
The second step would be to define `accessibilityElements` for UI components that you want to access for VoiceOver. VoiceOver will read elements from top to bottom.
```swift
override var accessibilityElements: [Any]? {
    get {
        return [
            nameLabel as Any,
            caloriesLabel as Any,
            favouriteButton as Any
        ]
    }
    set { }
}
```

![alt image](images/2.gif#center)

#### Third Step
The third step would be to add an `accessibilityLabel`; it will help identify the control or view.
```swift
favouriteButton.accessibilityLabel = "favourite"
```

#### Fourth Step
The fourth step would be to add an `accessibilityHint`; it describes the result of performing an action on the element.
```swift
favouriteButton.accessibilityHint = favouriteButton.isSelected ? "makes favourite" : "removes favourite"
```

### Adding Dynamic Type to Font Size
If you run your application and go to the `Accessibility Inspector` and `Run Audit` for it, you will probably see an error like `Dynamic font sizes are unsupported`. This means if you go and change the **Text Size** for your entire system, your application's font size will not change.

To fix it, you need to add `adjustsFontForContentSizeCategory = true` - it indicates whether the corresponding element should automatically update its font when the device’s `UIContentSizeCategory` is changed.
```swift
nameLabel.adjustsFontForContentSizeCategory = true
caloriesLabel.adjustsFontForContentSizeCategory = true
```

![alt image](images/1.gif#center)

If you need a custom font in your application, you need to transform the custom font to `Dynamic Type` by using `UIFontMetrics.default.scaledFont`.

Now you can actually test it from the `Accessibility Inspector` settings page.

### Caveats
The last thing is to make the table view automatically calculate row size because your content can potentially be clipped, as it happened to me 😊 You just need to add `UITableView.automaticDimension` to the `heightForRowAt` method of `UITableViewDelegate`.

### Download Materials
[Download](https://github.com/dmytrochumakov/accessibility-ios-uikit/archive/refs/heads/main.zip)

#### Thank you for reading! 😊
