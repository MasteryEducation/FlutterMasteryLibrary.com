---
linkTitle: "7.3.1 Project Setup: Task Manager"
title: "Task Manager App Setup with MobX: A Comprehensive Guide"
description: "Learn how to set up a Task Manager app using MobX in Flutter. This guide covers project creation, structure, and planning for effective state management."
categories:
- Flutter Development
- State Management
- MobX
tags:
- Flutter
- MobX
- State Management
- Task Manager
- App Development
date: 2024-10-25
type: docs
nav_weight: 731000
---

## 7.3.1 Project Setup: Task Manager

In this section, we will embark on an exciting journey to build a Task Manager app using MobX for state management in Flutter. This guide will walk you through setting up the project, organizing the codebase, and planning the app's features and user interface. By the end of this section, you'll have a solid foundation to develop a fully functional Task Manager app where users can create, update, and manage tasks efficiently.

### App Overview

The Task Manager app aims to provide users with a seamless experience in managing their tasks. The app will include the following features:

- **Viewing a List of Tasks:** Users can see all their tasks in a list format, with each task displaying its title and completion status.
- **Adding New Tasks:** Users can add new tasks by providing a title and an optional description.
- **Marking Tasks as Completed:** Users can mark tasks as completed, visually distinguishing them from pending tasks.
- **Filtering Tasks:** Users can filter tasks to view all, only completed, or only pending tasks.

This app will serve as a practical example of using MobX for state management, showcasing how reactive programming can enhance user experience by keeping the UI in sync with the underlying data.

### Setting Up the Project

Let's start by setting up the Flutter project for our Task Manager app. Follow these steps to get started:

1. **Create a New Flutter Project:**

   Open your terminal or command prompt and run the following command to create a new Flutter project named `task_manager_mobx`:

   ```bash
   flutter create task_manager_mobx
   ```

   This command will generate a new Flutter project with the necessary files and directories.

2. **Add Dependencies:**

   Open the `pubspec.yaml` file in your project and add the following dependencies:

   ```yaml
   dependencies:
     flutter_mobx: ^2.0.0
     mobx: ^2.0.0

   dev_dependencies:
     build_runner: ^2.1.0
     mobx_codegen: ^2.0.0
   ```

   These dependencies include `mobx` for state management, `flutter_mobx` for integrating MobX with Flutter, and `mobx_codegen` along with `build_runner` for code generation.

3. **Install Packages:**

   Run the following command to install the added packages:

   ```bash
   flutter pub get
   ```

   This command will fetch the dependencies and make them available for use in your project.

### Project Structure

Organizing your project files is crucial for maintainability and scalability. Here's a suggested structure for the Task Manager app:

```
lib/
├── main.dart
├── stores/
│   └── task_store.dart
├── models/
│   └── task.dart
├── widgets/
│   └── task_item.dart
└── screens/
    ├── home_screen.dart
    └── add_task_screen.dart
```

- **`main.dart`:** The entry point of the application.
- **`stores/`:** Contains MobX stores for managing application state.
- **`models/`:** Defines data models used in the app, such as the `Task` model.
- **`widgets/`:** Custom widgets used throughout the app, like `TaskItem`.
- **`screens/`:** Contains the main screens of the app, such as the home screen and the add task screen.

### Planning the App

Before diving into coding, it's essential to plan the app's user interface and define the flow between screens. Here are some steps to guide your planning process:

1. **Sketch the UI:**

   Visualize the app's layout by sketching the UI components. Consider how users will interact with the app, and plan the placement of buttons, lists, and input fields.

2. **Define the Flow:**

   Outline the navigation flow between screens. For instance, users will start on the home screen, where they can view tasks and navigate to the add task screen to create new tasks.

3. **List Components and Stores:**

   Identify the components and MobX stores needed for the app. For example, you'll need a `TaskStore` to manage the list of tasks and a `Task` model to represent individual tasks.

4. **Consider State Management:**

   Think about how MobX will manage the app's state. Plan how tasks will be added, updated, and filtered using MobX observables and actions.

### Key Takeaways

- **Importance of Planning:** Proper planning is crucial before starting to code. It helps you visualize the app's structure and flow, making development smoother and more efficient.
- **Project Setup:** Ensure you are comfortable with setting up the project and understand the overall structure. This foundation will make it easier to implement features and manage state effectively.

By following this guide, you are now equipped to set up the Task Manager app using MobX in Flutter. In the next sections, we'll dive deeper into implementing the app's features, leveraging MobX for state management, and creating a seamless user experience.

## Quiz Time!

{{< quizdown >}}

### What is the primary goal of the Task Manager app?

- [x] To allow users to create, update, and manage tasks efficiently.
- [ ] To provide a platform for social networking.
- [ ] To serve as a music streaming service.
- [ ] To function as a weather forecasting tool.

> **Explanation:** The Task Manager app is designed to help users manage their tasks by allowing them to create, update, and delete tasks, as well as filter them based on their status.

### What command is used to create a new Flutter project named `task_manager_mobx`?

- [x] `flutter create task_manager_mobx`
- [ ] `flutter init task_manager_mobx`
- [ ] `flutter new task_manager_mobx`
- [ ] `flutter start task_manager_mobx`

> **Explanation:** The `flutter create task_manager_mobx` command initializes a new Flutter project with the specified name.

### Which dependencies are added to the `pubspec.yaml` file for MobX integration?

- [x] `flutter_mobx`, `mobx`, `build_runner`, `mobx_codegen`
- [ ] `flutter_bloc`, `bloc`, `build_runner`, `bloc_codegen`
- [ ] `provider`, `mobx`, `build_runner`, `provider_codegen`
- [ ] `redux`, `mobx`, `build_runner`, `redux_codegen`

> **Explanation:** The dependencies `flutter_mobx`, `mobx`, `build_runner`, and `mobx_codegen` are necessary for integrating MobX with Flutter and generating code.

### What is the purpose of the `stores/` directory in the project structure?

- [x] To contain MobX stores for managing application state.
- [ ] To store images and other assets.
- [ ] To hold configuration files.
- [ ] To keep database schemas.

> **Explanation:** The `stores/` directory is used to organize MobX stores, which manage the application's state.

### Which file serves as the entry point of the Flutter application?

- [x] `main.dart`
- [ ] `task_store.dart`
- [ ] `home_screen.dart`
- [ ] `task.dart`

> **Explanation:** The `main.dart` file is the entry point of a Flutter application where the `main()` function is defined.

### What should be considered when planning the app's UI?

- [x] Sketching the UI layout and defining user interactions.
- [ ] Choosing the color scheme only.
- [ ] Deciding on the app's logo.
- [ ] Selecting a font style.

> **Explanation:** Planning the UI involves sketching the layout and considering how users will interact with the app, ensuring a seamless user experience.

### Why is it important to plan the app before coding?

- [x] To visualize the app's structure and flow, making development smoother.
- [ ] To increase the project's budget.
- [ ] To delay the development process.
- [ ] To reduce the app's functionality.

> **Explanation:** Proper planning helps visualize the app's structure and flow, leading to a more efficient development process and a better-organized codebase.

### What is the role of `mobx_codegen` in the project?

- [x] To generate code for MobX observables and actions.
- [ ] To compile the Flutter application.
- [ ] To manage database connections.
- [ ] To handle network requests.

> **Explanation:** `mobx_codegen` is used to generate code for MobX observables and actions, reducing boilerplate and enhancing productivity.

### How does MobX enhance user experience in the Task Manager app?

- [x] By keeping the UI in sync with the underlying data through reactive programming.
- [ ] By providing a dark mode theme.
- [ ] By integrating with social media platforms.
- [ ] By offering offline capabilities.

> **Explanation:** MobX enhances user experience by using reactive programming to keep the UI in sync with the underlying data, ensuring real-time updates and responsiveness.

### True or False: The Task Manager app allows users to filter tasks based on their completion status.

- [x] True
- [ ] False

> **Explanation:** The Task Manager app includes a feature that allows users to filter tasks to view all, only completed, or only pending tasks.

{{< /quizdown >}}
