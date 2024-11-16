---
linkTitle: "4.3.1 Project Setup: Notes App"
title: "Riverpod State Management: Project Setup for Notes App"
description: "Learn how to set up a Flutter project using Riverpod for state management. This comprehensive guide covers the architecture and features of a Notes app, including creating, editing, and deleting notes."
categories:
- Flutter
- State Management
- Riverpod
tags:
- Flutter
- Riverpod
- State Management
- Notes App
- App Development
date: 2024-10-25
type: docs
nav_weight: 431000
canonical: "https://fluttermasterylibrary.com/7/4/3/1"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 4.3.1 Project Setup: Notes App

In this section, we will embark on a journey to build a simple yet functional Notes app using Riverpod for state management. This project will serve as a practical example of how to leverage Riverpod's capabilities to manage state in a Flutter application. We will cover the entire process from setting up the project to planning the architecture and implementing core features such as creating, editing, and deleting notes.

### App Overview

Before diving into the technical setup, let's outline the key features of our Notes app:

- **Create Notes:** Users can add new notes with a title and content.
- **Edit Notes:** Existing notes can be modified to update their content.
- **Delete Notes:** Users can remove notes they no longer need.
- **List View:** Display all notes in a list format for easy navigation.
- **Detail View:** View the full content of a note in a dedicated screen.

These features will allow us to explore various aspects of state management, including handling lists of items, managing individual item states, and updating the UI in response to state changes.

### Setting Up the Project

#### Step 1: Create a New Flutter Project

To begin, we need to create a new Flutter project. Open your terminal and run the following command:

```bash
flutter create notes_app
```

This command will generate a new Flutter project with the default structure. Navigate into the project directory:

```bash
cd notes_app
```

#### Step 2: Add Riverpod to Your Project

Riverpod is a powerful state management solution for Flutter. To add it to your project, open the `pubspec.yaml` file and add the following dependencies:

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_riverpod: ^2.0.0
```

After adding the dependencies, run the following command to install them:

```bash
flutter pub get
```

This will fetch the Riverpod package and make it available in your project.

#### Step 3: Set Up the Project Structure

Organizing your project structure is crucial for maintainability and scalability. Here's a suggested structure for our Notes app:

```
lib/
  |- main.dart
  |- models/
      |- note.dart
  |- providers/
      |- note_provider.dart
  |- screens/
      |- home_screen.dart
      |- note_detail_screen.dart
      |- note_edit_screen.dart
  |- widgets/
      |- note_list.dart
      |- note_item.dart
```

- **models/**: Contains data models, such as `note.dart`, which defines the structure of a note.
- **providers/**: Houses provider logic, including `note_provider.dart`, where we manage the state of notes.
- **screens/**: Contains the UI screens of the app, such as `home_screen.dart` for the main list of notes and `note_detail_screen.dart` for viewing a note.
- **widgets/**: Includes reusable UI components like `note_list.dart` and `note_item.dart`.

### Planning the App Architecture

Before writing any code, it's essential to plan the app's architecture. This involves deciding how data flows through the app, how state is managed, and how different components interact.

#### State Management with Riverpod

Riverpod offers a robust way to manage state in Flutter applications. It provides a variety of providers to handle different types of state, including:

- **Provider**: For immutable state.
- **StateProvider**: For simple mutable state.
- **StateNotifierProvider**: For complex state logic with a `StateNotifier`.
- **FutureProvider**: For asynchronous data.
- **StreamProvider**: For stream-based data.

For our Notes app, we'll primarily use `StateNotifierProvider` to manage the list of notes, as it allows us to encapsulate complex state logic and easily notify listeners of state changes.

#### Data Flow and UI Interaction

The app will follow a unidirectional data flow pattern, where data flows in a single direction:

1. **State**: Managed by Riverpod providers.
2. **UI**: Reacts to state changes and displays data.
3. **User Actions**: Trigger state updates through providers.

This pattern ensures a clear separation of concerns and makes the app easier to reason about and maintain.

#### Designing the Note Model

Let's define the data model for a note. Create a new file `note.dart` in the `models` directory:

```dart
class Note {
  final String id;
  final String title;
  final String content;

  Note({
    required this.id,
    required this.title,
    required this.content,
  });
}
```

This simple model includes an `id`, `title`, and `content` for each note.

#### Implementing the Note Provider

Next, we'll implement the provider logic to manage the state of notes. Create a file `note_provider.dart` in the `providers` directory:

```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../models/note.dart';

class NoteNotifier extends StateNotifier<List<Note>> {
  NoteNotifier() : super([]);

  void addNote(Note note) {
    state = [...state, note];
  }

  void editNote(String id, String title, String content) {
    state = [
      for (final note in state)
        if (note.id == id)
          Note(
            id: note.id,
            title: title,
            content: content,
          )
        else
          note,
    ];
  }

  void deleteNote(String id) {
    state = state.where((note) => note.id != id).toList();
  }
}

final noteProvider = StateNotifierProvider<NoteNotifier, List<Note>>((ref) {
  return NoteNotifier();
});
```

In this provider, we define methods to add, edit, and delete notes. The `StateNotifier` manages a list of `Note` objects, and the `StateNotifierProvider` exposes this state to the rest of the app.

### Conclusion

Setting up a Flutter project with Riverpod for state management involves careful planning and organization. By defining a clear architecture and utilizing Riverpod's powerful features, we can build a robust Notes app that efficiently manages state. In the following sections, we'll dive deeper into implementing the UI and integrating these components to create a fully functional application.

## Quiz Time!

{{< quizdown >}}

### What are the key features of the Notes app we are building?

- [x] Create, edit, and delete notes
- [ ] Share notes with others
- [x] List and view notes
- [ ] Sync notes with cloud storage

> **Explanation:** The key features include creating, editing, deleting, listing, and viewing notes. Sharing and cloud sync are not part of the initial feature set.

### Which command is used to create a new Flutter project?

- [x] `flutter create notes_app`
- [ ] `flutter new notes_app`
- [ ] `flutter init notes_app`
- [ ] `flutter start notes_app`

> **Explanation:** The `flutter create notes_app` command initializes a new Flutter project with the name `notes_app`.

### Where should the `note.dart` file be placed in the project structure?

- [x] In the `models/` directory
- [ ] In the `providers/` directory
- [ ] In the `screens/` directory
- [ ] In the `widgets/` directory

> **Explanation:** The `note.dart` file, which defines the data model, should be placed in the `models/` directory.

### What type of provider is used to manage the list of notes?

- [x] `StateNotifierProvider`
- [ ] `Provider`
- [ ] `FutureProvider`
- [ ] `StreamProvider`

> **Explanation:** `StateNotifierProvider` is used to manage complex state logic, such as a list of notes, with a `StateNotifier`.

### What is the purpose of the `StateNotifier` in the `note_provider.dart` file?

- [x] To manage the state of notes
- [ ] To handle asynchronous operations
- [ ] To provide UI components
- [ ] To manage network requests

> **Explanation:** The `StateNotifier` is responsible for managing the state of notes, including adding, editing, and deleting them.

### Which dependency is added to the `pubspec.yaml` file for state management?

- [x] `flutter_riverpod`
- [ ] `provider`
- [ ] `bloc`
- [ ] `redux`

> **Explanation:** `flutter_riverpod` is the dependency added for state management using Riverpod.

### What pattern does the app follow for data flow?

- [x] Unidirectional data flow
- [ ] Bidirectional data flow
- [ ] Circular data flow
- [ ] Random data flow

> **Explanation:** The app follows a unidirectional data flow pattern, where data flows in a single direction from state to UI to user actions.

### What is the role of the `noteProvider` in the app?

- [x] To expose the state of notes to the app
- [ ] To render the UI components
- [ ] To handle network requests
- [ ] To manage user authentication

> **Explanation:** The `noteProvider` exposes the state of notes to the app, allowing the UI to react to changes.

### What is the first step in setting up the project?

- [x] Create a new Flutter project
- [ ] Install dependencies
- [ ] Design the UI
- [ ] Write the business logic

> **Explanation:** The first step is to create a new Flutter project using the `flutter create` command.

### True or False: Planning the app architecture is not necessary before coding.

- [ ] True
- [x] False

> **Explanation:** Planning the app architecture is crucial before coding to ensure a well-organized and maintainable project.

{{< /quizdown >}}
