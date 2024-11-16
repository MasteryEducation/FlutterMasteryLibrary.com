---
linkTitle: "3.3.1 Setting Up the Project"
title: "Setting Up the Project for a Todo App with Provider in Flutter"
description: "Learn how to set up a Flutter project for a Todo app using the Provider package. This guide covers project initialization, dependency management, and basic UI planning."
categories:
- Flutter Development
- State Management
- Mobile App Development
tags:
- Flutter
- Provider
- Todo App
- State Management
- Mobile Development
date: 2024-10-25
type: docs
nav_weight: 331000
canonical: "https://fluttermasterylibrary.com/7/3/3/1"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 3.3.1 Setting Up the Project

In this section, we will embark on a journey to build a Todo app using Flutter and the Provider package. This app will allow users to add, update, and delete tasks, as well as mark them as complete or incomplete. While persisting tasks across sessions is an optional feature that we will explore in later chapters, this section will focus on setting up the project, organizing the codebase, and planning the user interface.

### Project Overview

Before diving into the technical setup, let's outline the core features of our Todo app:

- **Add Tasks:** Users can create new tasks with a title and description.
- **Update Tasks:** Users can modify existing tasks to change their details.
- **Delete Tasks:** Users can remove tasks they no longer need.
- **Toggle Task Completion:** Users can mark tasks as complete or incomplete.
- **Persist Tasks:** (Optional) Save tasks across app sessions for later retrieval.

These features will form the backbone of our Todo app, providing a practical context for exploring state management with Provider.

### Creating the Flutter Project

To get started, we need to create a new Flutter project. Open your terminal or command prompt and run the following command:

```bash
flutter create todo_app_provider
```

This command initializes a new Flutter project named `todo_app_provider`. Once the project is created, navigate into the project directory:

```bash
cd todo_app_provider
```

#### Directory Structure

Understanding the directory structure is crucial for organizing your Flutter project effectively. Here's a brief overview of the key directories and files:

- **`lib/`:** This is where the main application code resides. You'll organize your Dart files here, including UI components, models, and providers.
  - **`lib/main.dart`:** The entry point of your Flutter application.
  - **`lib/screens/`:** A directory to store different screen widgets, such as the Todo List Screen and Add/Edit Todo Screen.
  - **`lib/models/`:** A directory for data models, such as the Todo model.
  - **`lib/providers/`:** A directory for provider classes that manage state.
- **`pubspec.yaml`:** The configuration file where you define dependencies and other project settings.

### Adding Dependencies

To use the Provider package, we need to add it to our project's dependencies. Open the `pubspec.yaml` file and add the following under the `dependencies` section:

```yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.0.0
```

After updating the `pubspec.yaml`, run the following command to install the new dependencies:

```bash
flutter pub get
```

This command fetches the Provider package and makes it available in your project.

### Setting Up Main.dart

With the project initialized and dependencies added, it's time to set up the `main.dart` file. This file serves as the entry point of your Flutter application. We'll configure it to use the `ChangeNotifierProvider` from the Provider package.

Here's how you can set up `main.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'providers/todo_provider.dart';
import 'screens/todo_list_screen.dart';

void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => TodoProvider(),
      child: MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Todo App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: TodoListScreen(),
    );
  }
}
```

#### Explanation:

- **`ChangeNotifierProvider`:** This widget provides an instance of `TodoProvider` to the widget tree. It listens for changes and rebuilds the UI accordingly.
- **`MyApp`:** The root widget of the application, which sets up the `MaterialApp` and defines the initial screen (`TodoListScreen`).

### Planning the UI

Before diving into the code, it's beneficial to plan the user interface. Wireframing is a great way to visualize the app's layout and flow. Here are the main screens we'll implement:

- **Todo List Screen:** Displays a list of tasks with options to add, edit, or delete them. This screen will be the main hub of the app.
- **Add/Edit Todo Screen:** Allows users to create new tasks or modify existing ones. This screen will include input fields for task details and buttons to save or cancel changes.

Consider using tools like Figma or Sketch to create wireframes, or sketch them out on paper. This planning phase will help you identify the components and interactions needed, making the coding process smoother.

### Conclusion

Setting up the project is a crucial step in building a robust Flutter application. By organizing the directory structure, adding necessary dependencies, and planning the UI, you've laid a solid foundation for developing the Todo app. In the next sections, we'll delve deeper into implementing the app's features and managing state with Provider.

### Additional Resources

- [Flutter Documentation](https://flutter.dev/docs): Official Flutter documentation for further reading.
- [Provider Package](https://pub.dev/packages/provider): Explore the Provider package on pub.dev for more details and examples.
- [Wireframing Tools](https://www.figma.com/): Figma is a popular tool for designing and prototyping UI.

## Quiz Time!

{{< quizdown >}}

### What command is used to create a new Flutter project named `todo_app_provider`?

- [x] `flutter create todo_app_provider`
- [ ] `flutter init todo_app_provider`
- [ ] `flutter new todo_app_provider`
- [ ] `flutter start todo_app_provider`

> **Explanation:** The `flutter create` command initializes a new Flutter project with the specified name.

### Where should you define the dependencies for a Flutter project?

- [x] In the `pubspec.yaml` file
- [ ] In the `main.dart` file
- [ ] In the `lib/` directory
- [ ] In the `build.gradle` file

> **Explanation:** The `pubspec.yaml` file is used to define dependencies and other project settings in a Flutter project.

### What is the purpose of the `ChangeNotifierProvider` in the `main.dart` file?

- [x] To provide an instance of `TodoProvider` to the widget tree
- [ ] To initialize the Flutter application
- [ ] To handle HTTP requests
- [ ] To manage routing between screens

> **Explanation:** `ChangeNotifierProvider` is used to provide an instance of a class that extends `ChangeNotifier` to the widget tree, allowing for state management.

### Which directory is typically used to store screen widgets in a Flutter project?

- [x] `lib/screens/`
- [ ] `lib/models/`
- [ ] `lib/providers/`
- [ ] `lib/utils/`

> **Explanation:** The `lib/screens/` directory is commonly used to organize screen widgets in a Flutter project.

### What is the main purpose of wireframing in UI design?

- [x] To visualize the app's layout and flow before coding
- [ ] To write the app's backend logic
- [ ] To optimize app performance
- [ ] To manage app state

> **Explanation:** Wireframing helps visualize the layout and flow of an app, making the coding process more efficient.

### Which command is used to install dependencies defined in `pubspec.yaml`?

- [x] `flutter pub get`
- [ ] `flutter install`
- [ ] `flutter dependencies`
- [ ] `flutter setup`

> **Explanation:** `flutter pub get` fetches and installs the dependencies listed in the `pubspec.yaml` file.

### What is the entry point of a Flutter application?

- [x] `main.dart`
- [ ] `app.dart`
- [ ] `index.dart`
- [ ] `home.dart`

> **Explanation:** The `main.dart` file serves as the entry point of a Flutter application.

### Which widget is used to set up the initial screen of a Flutter app?

- [x] `MaterialApp`
- [ ] `Scaffold`
- [ ] `Container`
- [ ] `Column`

> **Explanation:** `MaterialApp` is used to set up the initial screen and configure the app's theme and routing.

### What is the purpose of the `TodoProvider` class in the context of this project?

- [x] To manage the state of the Todo app
- [ ] To handle network requests
- [ ] To define the UI layout
- [ ] To manage user authentication

> **Explanation:** `TodoProvider` is responsible for managing the state of the Todo app, including tasks and their statuses.

### True or False: Persisting tasks across sessions is a mandatory feature in this chapter.

- [ ] True
- [x] False

> **Explanation:** Persisting tasks across sessions is an optional feature that will be explored in later chapters.

{{< /quizdown >}}
