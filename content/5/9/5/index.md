---
linkTitle: "9.5 Mini Project: Accessibility Checker"
title: "Flutter Accessibility Checker App: A Mini Project for Kids"
description: "Learn how to build an accessibility checker tool in Flutter, focusing on color contrast and accessibility metrics. This project guides young coders through creating a user-friendly app that evaluates text and color combinations for accessibility."
categories:
- Flutter Development
- Kids Coding
- Accessibility
tags:
- Flutter
- Accessibility
- Coding for Kids
- Color Contrast
- App Development
date: 2024-10-25
type: docs
nav_weight: 950000
canonical: "https://fluttermasterylibrary.com/5/9/5"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 9.5 Mini Project: Accessibility Checker

Welcome to another exciting project in our Flutter adventure! In this mini-project, we'll create an **Accessibility Checker** app. This app will help us understand the importance of accessibility in design by checking if the colors we choose for text and backgrounds are easy to read for everyone.

### Project Overview

The Accessibility Checker app will allow users to input text and select colors for the text and background. It will then evaluate these choices based on accessibility standards, particularly focusing on color contrast. This project is a great way to learn about making apps more inclusive and user-friendly.

### Step-by-Step Instructions

Let's dive into building our Accessibility Checker app!

#### 1. Setting Up the Project

First, we'll create a new Flutter project. Open your terminal or command prompt and run the following command:

```bash
flutter create accessibility_checker
```

This command sets up a new Flutter project named `accessibility_checker`. Once it's created, open the project in your favorite code editor.

#### 2. Designing the Input Screen

Next, we'll design the input screen where users can enter text and choose colors.

- Open `lib/main.dart` and replace the existing code with the following:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(AccessibilityCheckerApp());
}

class AccessibilityCheckerApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Accessibility Checker',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: AccessibilityCheckerScreen(),
    );
  }
}

class AccessibilityCheckerScreen extends StatefulWidget {
  @override
  _AccessibilityCheckerScreenState createState() => _AccessibilityCheckerScreenState();
}

class _AccessibilityCheckerScreenState extends State<AccessibilityCheckerScreen> {
  final TextEditingController _textController = TextEditingController();
  Color _textColor = Colors.black;
  Color _backgroundColor = Colors.white;
  String _feedback = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Accessibility Checker'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _textController,
              decoration: InputDecoration(labelText: 'Enter your text here'),
            ),
            SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                ElevatedButton(
                  onPressed: () => _pickColor(context, true),
                  child: Text('Select Text Color'),
                ),
                ElevatedButton(
                  onPressed: () => _pickColor(context, false),
                  child: Text('Select Background Color'),
                ),
              ],
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: _checkAccessibility,
              child: Text('Check Accessibility'),
            ),
            SizedBox(height: 20),
            Text(
              _feedback,
              style: TextStyle(fontSize: 18),
            ),
          ],
        ),
      ),
    );
  }

  Future<void> _pickColor(BuildContext context, bool isTextColor) async {
    Color? pickedColor = await showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('Pick a color'),
        content: SingleChildScrollView(
          child: ColorPicker(
            pickerColor: isTextColor ? _textColor : _backgroundColor,
            onColorChanged: (color) {
              setState(() {
                if (isTextColor) {
                  _textColor = color;
                } else {
                  _backgroundColor = color;
                }
              });
            },
          ),
        ),
        actions: <Widget>[
          ElevatedButton(
            child: Text('Select'),
            onPressed: () {
              Navigator.of(context).pop();
            },
          ),
        ],
      ),
    );
  }

  void _checkAccessibility() {
    bool isGoodContrast = isContrastGood(_textColor, _backgroundColor);
    setState(() {
      _feedback = getContrastFeedback(isGoodContrast);
    });
  }

  bool isContrastGood(Color fg, Color bg) {
    double contrast = (fg.computeLuminance() + 0.05) / (bg.computeLuminance() + 0.05);
    return contrast > 1.5;
  }

  String getContrastFeedback(bool isGood) {
    return isGood ? 'Good contrast!' : 'Poor contrast. Consider different colors.';
  }
}
```

- **Explanation:**
  - We created a simple UI with a `TextField` for input, buttons to select text and background colors, and a button to check accessibility.
  - The `_pickColor` function allows users to choose colors.
  - The `_checkAccessibility` function evaluates the color contrast.

#### 3. Implementing Accessibility Check

The core of our app is checking if the chosen colors have good contrast. We use a simple formula to calculate contrast:

```dart
bool isContrastGood(Color fg, Color bg) {
  double contrast = (fg.computeLuminance() + 0.05) / (bg.computeLuminance() + 0.05);
  return contrast > 1.5;
}

String getContrastFeedback(bool isGood) {
  return isGood ? 'Good contrast!' : 'Poor contrast. Consider different colors.';
}
```

- **Explanation:**
  - `computeLuminance()` calculates the brightness of a color.
  - We compare the luminance of the foreground (text) and background colors to determine if the contrast is sufficient.

#### 4. Displaying Feedback

After checking the contrast, we display feedback to the user. The feedback is shown as a simple text message indicating whether the contrast is good or poor.

#### 5. Enhancing the App

To make our app more helpful, let's add some enhancements:

- **Recommendations:** Suggest color combinations that meet accessibility standards.
- **Color Palettes:** Provide a list of accessible color palettes for users to choose from.

You can expand the app by adding a list of recommended colors and updating the UI to allow users to select from these options.

#### 6. Testing the App

Finally, test your app by trying different text and background color combinations. Ensure that the feedback accurately reflects the accessibility of the chosen colors.

### Complete Code Example

Here's the complete code for the Accessibility Checker app. Feel free to modify and experiment with it to better understand how it works.

```dart
// Complete code provided above
```

### Interactive Exercise

Now it's your turn! Try different color combinations in your app and see how they perform. Think about why accessibility is important and how it can make apps better for everyone.

### Visual Aids

Below are some screenshots showing the input interface and feedback messages:

- **Input Screen:**
  ![Input Screen](path/to/input_screen.png)

- **Feedback Message:**
  ![Feedback Message](path/to/feedback_message.png)

### Conclusion

Congratulations on building your Accessibility Checker app! You've learned how to evaluate color contrast and why accessibility is crucial in app design. Keep experimenting and thinking about how you can make your apps more inclusive.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the Accessibility Checker app?

- [x] To evaluate color contrast for accessibility
- [ ] To create colorful designs
- [ ] To test text input functionality
- [ ] To learn about Flutter animations

> **Explanation:** The app is designed to check if the color contrast between text and background is accessible.

### Which Flutter widget is used for text input in the app?

- [x] TextField
- [ ] Text
- [ ] Container
- [ ] Column

> **Explanation:** The `TextField` widget is used to allow users to input text.

### What does the `computeLuminance()` method do?

- [x] Calculates the brightness of a color
- [ ] Changes the color of a widget
- [ ] Adjusts the size of a widget
- [ ] Computes the contrast between two colors

> **Explanation:** `computeLuminance()` calculates how bright a color is, which is used to determine contrast.

### What feedback does the app provide if the contrast is poor?

- [x] "Poor contrast. Consider different colors."
- [ ] "Good contrast!"
- [ ] "Contrast is perfect!"
- [ ] "Try again."

> **Explanation:** The app provides feedback to suggest changing colors if the contrast is poor.

### How can you enhance the Accessibility Checker app?

- [x] Add recommendations for better color combinations
- [x] Include a list of accessible color palettes
- [ ] Remove the color selection feature
- [ ] Limit text input length

> **Explanation:** Enhancements include adding recommendations and accessible color palettes.

### What is the contrast threshold used in the app?

- [x] 1.5
- [ ] 2.0
- [ ] 3.0
- [ ] 1.0

> **Explanation:** The app uses a contrast threshold of 1.5 for demonstration purposes.

### Why is accessibility important in app design?

- [x] It ensures apps are usable by everyone
- [x] It improves user experience
- [ ] It makes apps more colorful
- [ ] It reduces app size

> **Explanation:** Accessibility ensures that apps are inclusive and provide a better user experience for all.

### What is a potential enhancement for the app?

- [x] Suggesting accessible color combinations
- [ ] Adding more text fields
- [ ] Removing the feedback feature
- [ ] Limiting color choices

> **Explanation:** Suggesting accessible color combinations can enhance the app's usefulness.

### What should you do after building the app?

- [x] Test different color combinations
- [ ] Delete the project
- [ ] Ignore feedback
- [ ] Only use default colors

> **Explanation:** Testing different combinations helps ensure the app works as intended.

### True or False: The app can only check text input for accessibility.

- [ ] True
- [x] False

> **Explanation:** The app checks color contrast, not just text input, for accessibility.

{{< /quizdown >}}
