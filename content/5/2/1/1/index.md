---
linkTitle: "2.1.1 Writing 'Hello, World!'"
title: "Create Your First Flutter App: Writing 'Hello, World!'"
description: "Learn how to create your first Flutter application by displaying a simple 'Hello, World!' message. This guide will walk you through setting up your code editor, writing the code, and running your app."
categories:
- Flutter
- Programming
- Kids Coding
tags:
- Flutter
- Hello World
- Dart
- Kids Coding
- Beginner Programming
date: 2024-10-25
type: docs
nav_weight: 211000
canonical: "https://fluttermasterylibrary.com/5/2/1/1"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 2.1.1 Writing "Hello, World!"

Welcome to your first adventure in coding with Flutter! Today, we're going to create a simple app that displays the message "Hello, World!" on the screen. This is a traditional first step in programming, and it's a great way to get familiar with how Flutter works. Let's dive in!

### What is "Hello, World!"?

"Hello, World!" is a classic program that programmers use to learn a new language or framework. It's a simple way to see how to display text on the screen, and it gives you a taste of what coding is all about. By the end of this section, you'll have your very own Flutter app showing this iconic message!

### Step-by-Step Guide to Your First Flutter App

#### 1. Open Your Code Editor

First, let's open the code editor you set up in Chapter 1. This is where you'll write and edit your code. If you're using Visual Studio Code, click on the icon to launch it. Remember, your code editor is like your digital notebook where all your coding magic happens!

#### 2. Create a New Dart File

Now, let's create a new file where we'll write our Flutter code. In your code editor, follow these steps:

- Click on "File" in the top menu.
- Select "New File" from the dropdown.
- Save this new file as `main.dart`. This is the file where we'll write our Flutter app.

#### 3. Write the Code

It's time to write some code! Copy and paste the following code into your `main.dart` file:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Hello, World!'),
        ),
        body: Center(
          child: Text('Hello, World!'),
        ),
      ),
    );
  }
}
```

**Let's break down what this code does:**

- **Import Statement:** `import 'package:flutter/material.dart';` brings in the Flutter material design library, which provides a lot of the tools we need to build our app.
- **Main Function:** `void main() { runApp(MyApp()); }` is the entry point of our app. It tells Flutter to start running the app and use `MyApp` as the main widget.
- **MyApp Class:** This is a custom widget that extends `StatelessWidget`. It defines the structure of our app.
- **MaterialApp Widget:** This widget sets up the basic structure of our app, including the home screen.
- **Scaffold Widget:** Provides a framework for the app's layout, including the app bar and body.
- **AppBar and Text Widgets:** These widgets display the "Hello, World!" message in the app bar and the center of the screen.

#### 4. Run the App

Now that you've written your code, it's time to see it in action! Follow these steps to run your app:

- Open the terminal in your code editor. You can usually find it under the "View" menu.
- Type `flutter run` and press Enter. This command tells Flutter to build and run your app.
- Wait a few moments, and soon you'll see your app open with the "Hello, World!" message displayed!

### Visualizing the Code Structure

To help you understand how the code is structured, here's a simple flowchart using Mermaid.js:

```mermaid
graph TD;
    A[Start] --> B[Import Flutter Material Library];
    B --> C[Define Main Function];
    C --> D[Create MyApp Class];
    D --> E[Return MaterialApp Widget];
    E --> F[Scaffold Widget];
    F --> G[AppBar with Text];
    F --> H[Body with Center and Text];
    G --> I[Display "Hello, World!"];
    H --> I;
```

### How Does It Feel?

Congratulations! You've just created your first Flutter app. How does it feel to see your first message on the screen? This is just the beginning of your coding journey, and there's so much more to explore and create.

### Encouragement and Next Steps

Creating your first app is a big achievement, and you should be proud of yourself. As you continue learning, remember that coding is all about experimenting and having fun. Don't be afraid to try new things and make mistakes—it's all part of the learning process.

In the next sections, we'll explore more about Flutter and Dart, and you'll get to build even more exciting apps. Keep up the great work!

## Quiz Time!

{{< quizdown >}}

### What is the purpose of the "Hello, World!" program?

- [x] To display a simple message on the screen
- [ ] To create a complex application
- [ ] To connect to the internet
- [ ] To manage a database

> **Explanation:** The "Hello, World!" program is traditionally used to display a simple message on the screen, helping beginners understand the basics of a programming language.

### What is the main function in a Flutter app?

- [x] It is the entry point of the app
- [ ] It is used to display images
- [ ] It is a function to handle user input
- [ ] It is a function to manage data

> **Explanation:** The main function is the entry point of a Flutter app, where the execution starts.

### Which widget is used to create the basic structure of a Flutter app?

- [x] MaterialApp
- [ ] Text
- [ ] Image
- [ ] Button

> **Explanation:** The MaterialApp widget is used to create the basic structure of a Flutter app, providing a framework for the app's layout.

### What does the Scaffold widget do?

- [x] Provides a framework for the app's layout
- [ ] Displays images
- [ ] Handles user input
- [ ] Manages data

> **Explanation:** The Scaffold widget provides a framework for the app's layout, including the app bar and body.

### What is the purpose of the AppBar widget?

- [x] To display a title or other widgets at the top of the app
- [ ] To manage user input
- [ ] To display images
- [ ] To handle data storage

> **Explanation:** The AppBar widget is used to display a title or other widgets at the top of the app, typically used for navigation.

### How do you run a Flutter app from the terminal?

- [x] By typing `flutter run`
- [ ] By typing `flutter start`
- [ ] By typing `flutter execute`
- [ ] By typing `flutter launch`

> **Explanation:** You run a Flutter app from the terminal by typing `flutter run`, which builds and launches the app.

### What does the Text widget do?

- [x] Displays text on the screen
- [ ] Displays images
- [ ] Handles user input
- [ ] Manages data

> **Explanation:** The Text widget is used to display text on the screen in a Flutter app.

### What is the purpose of the Center widget?

- [x] To center its child widget within the available space
- [ ] To display images
- [ ] To handle user input
- [ ] To manage data

> **Explanation:** The Center widget is used to center its child widget within the available space, making it useful for aligning content.

### What does the `import 'package:flutter/material.dart';` statement do?

- [x] Imports the Flutter material design library
- [ ] Imports images
- [ ] Imports user data
- [ ] Imports network resources

> **Explanation:** The `import 'package:flutter/material.dart';` statement imports the Flutter material design library, providing tools to build the app.

### True or False: The "Hello, World!" program is only used in Flutter.

- [ ] True
- [x] False

> **Explanation:** False. The "Hello, World!" program is a traditional first program used in many programming languages, not just Flutter.

{{< /quizdown >}}
