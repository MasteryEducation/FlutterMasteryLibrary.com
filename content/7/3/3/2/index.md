---

linkTitle: "3.3.2 Creating Models and ChangeNotifiers"
title: "Creating Models and ChangeNotifiers in Flutter"
description: "Explore how to create models and ChangeNotifiers in Flutter for effective state management using the Provider package, with practical examples and best practices."
categories:
- Flutter
- State Management
- Mobile Development
tags:
- Flutter
- Provider
- ChangeNotifier
- State Management
- Todo App
date: 2024-10-25
type: docs
nav_weight: 332000
---

## 3.3.2 Creating Models and ChangeNotifiers

In this section, we delve into the process of creating models and ChangeNotifiers in Flutter, focusing on their role within the Provider package for state management. We will explore how to define a `Todo` model, implement a `TodoProvider` using `ChangeNotifier`, and adhere to best practices for efficient state management. This knowledge is crucial for building robust and scalable Flutter applications.

### Defining the Todo Model

The first step in managing state with Provider is to define the data model. In our Todo app example, the `Todo` model represents individual tasks. This model encapsulates the properties and behaviors of a task, such as its title, description, and completion status.

#### Creating the Todo Class

The `Todo` class is a simple Dart class that holds the properties of a task. Below is the implementation of the `Todo` class:

```dart
class Todo {
  String id;
  String title;
  String description;
  bool isCompleted;

  Todo({
    required this.id,
    required this.title,
    required this.description,
    this.isCompleted = false,
  });
}
```

**Explanation:**

- **Properties:**
  - `id`: A unique identifier for each task, typically a string.
  - `title`: The name or title of the task.
  - `description`: A detailed description of the task.
  - `isCompleted`: A boolean indicating whether the task is completed, defaulting to `false`.

- **Constructor:**
  - The constructor initializes the properties, with `isCompleted` defaulting to `false`.

This class serves as the blueprint for creating task instances in the app.

### Implementing the TodoProvider

The `TodoProvider` class is responsible for managing the state of the list of todos. It extends `ChangeNotifier`, which is a part of the Flutter framework that allows widgets to listen to changes in state and rebuild accordingly.

#### Creating the TodoProvider Class

Below is the implementation of the `TodoProvider` class:

```dart
import 'package:flutter/foundation.dart';

class TodoProvider extends ChangeNotifier {
  List<Todo> _todos = [];

  List<Todo> get todos => _todos;

  void addTodo(Todo todo) {
    _todos.add(todo);
    notifyListeners();
  }

  void updateTodo(String id, String title, String description) {
    final index = _todos.indexWhere((todo) => todo.id == id);
    if (index != -1) {
      _todos[index].title = title;
      _todos[index].description = description;
      notifyListeners();
    }
  }

  void deleteTodo(String id) {
    _todos.removeWhere((todo) => todo.id == id);
    notifyListeners();
  }

  void toggleCompletion(String id) {
    final index = _todos.indexWhere((todo) => todo.id == id);
    if (index != -1) {
      _todos[index].isCompleted = !_todos[index].isCompleted;
      notifyListeners();
    }
  }
}
```

**Explanation:**

- **State Management:**
  - `_todos`: A private list that holds all the `Todo` objects.
  - `todos`: A getter that provides read-only access to the `_todos` list.

- **Methods:**
  - `addTodo`: Adds a new `Todo` to the list and calls `notifyListeners()` to update the UI.
  - `updateTodo`: Updates the title and description of an existing `Todo`.
  - `deleteTodo`: Removes a `Todo` from the list by its `id`.
  - `toggleCompletion`: Toggles the `isCompleted` status of a `Todo`.

- **ChangeNotifier:**
  - `notifyListeners()`: This method is called whenever there is a change in the state that should be reflected in the UI.

### Thread Safety and Concurrency

In Flutter, UI operations are typically single-threaded, meaning they run on the main thread. However, when dealing with data that might be accessed or modified from multiple threads, such as when performing network requests or heavy computations, thread safety becomes a concern.

For our `TodoProvider`, since the operations are simple and run on the main thread, explicit thread safety mechanisms are not necessary. However, if you anticipate concurrent modifications, consider using synchronization mechanisms like `Mutex` or Dart's `synchronized` package to ensure thread safety.

### Best Practices

To ensure efficient and maintainable state management, consider the following best practices:

- **Encapsulation:** Keep the state private and expose only necessary data through getters.
- **Single Responsibility:** Ensure that the provider focuses solely on state management logic, avoiding UI-related code.
- **Efficiency:** Use `notifyListeners()` judiciously to avoid unnecessary UI rebuilds.
- **Modularity:** Keep your models and providers modular to facilitate testing and maintenance.

### Practical Example

Let's integrate the `TodoProvider` into a simple Flutter app to see it in action. Below is a basic setup that demonstrates how to use the provider in a Flutter application.

#### Setting Up the Provider

First, add the `provider` package to your `pubspec.yaml` file:

```yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.0.0
```

Then, wrap your app with a `ChangeNotifierProvider` to provide the `TodoProvider` to the widget tree:

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

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
      home: TodoListScreen(),
    );
  }
}
```

#### Building the Todo List Screen

Create a `TodoListScreen` widget to display the list of todos:

```dart
class TodoListScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final todoProvider = Provider.of<TodoProvider>(context);

    return Scaffold(
      appBar: AppBar(
        title: Text('Todo List'),
      ),
      body: ListView.builder(
        itemCount: todoProvider.todos.length,
        itemBuilder: (context, index) {
          final todo = todoProvider.todos[index];
          return ListTile(
            title: Text(todo.title),
            subtitle: Text(todo.description),
            trailing: Checkbox(
              value: todo.isCompleted,
              onChanged: (value) {
                todoProvider.toggleCompletion(todo.id);
              },
            ),
            onLongPress: () {
              todoProvider.deleteTodo(todo.id);
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Add a new Todo
          todoProvider.addTodo(Todo(
            id: DateTime.now().toString(),
            title: 'New Todo',
            description: 'Description',
          ));
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

**Explanation:**

- **Provider Access:** Use `Provider.of<TodoProvider>(context)` to access the provider instance.
- **ListView.builder:** Dynamically builds the list of todos using the provider's data.
- **Checkbox and Long Press:** Allows toggling completion and deleting todos, respectively.

### Conclusion

By defining a `Todo` model and implementing a `TodoProvider` with `ChangeNotifier`, we have created a robust foundation for managing state in a Flutter application. This approach not only simplifies state management but also enhances the scalability and maintainability of the app.

### Additional Resources

- [Flutter Provider Documentation](https://pub.dev/packages/provider)
- [Official Flutter Documentation](https://flutter.dev/docs)
- [Dart Language Tour](https://dart.dev/guides/language/language-tour)

By following these guidelines and utilizing the Provider package, you can effectively manage state in your Flutter applications, ensuring a smooth and responsive user experience.

## Quiz Time!

{{< quizdown >}}

### What is the primary role of the `Todo` class in the Todo app?

- [x] To represent individual tasks with properties such as title and completion status.
- [ ] To manage the state of the entire list of todos.
- [ ] To handle user input and UI interactions.
- [ ] To store application-wide settings and preferences.

> **Explanation:** The `Todo` class is a data model that represents individual tasks, encapsulating properties like title, description, and completion status.

### Which method in the `TodoProvider` class is responsible for notifying listeners of state changes?

- [ ] `addTodo`
- [ ] `updateTodo`
- [ ] `deleteTodo`
- [x] `notifyListeners`

> **Explanation:** `notifyListeners` is called within methods like `addTodo`, `updateTodo`, and `deleteTodo` to inform listeners about state changes.

### How does the `TodoProvider` class ensure that the UI updates when the state changes?

- [ ] By directly modifying UI components.
- [x] By calling `notifyListeners` after state changes.
- [ ] By using a separate UI update thread.
- [ ] By storing UI state within the provider.

> **Explanation:** The `TodoProvider` uses `notifyListeners` to trigger UI updates when the state changes, allowing widgets to rebuild with the new data.

### What is the purpose of the `ChangeNotifier` class in Flutter?

- [ ] To manage network requests.
- [x] To provide a way for widgets to listen to state changes.
- [ ] To handle user authentication.
- [ ] To optimize app performance.

> **Explanation:** `ChangeNotifier` is a class in Flutter that allows widgets to listen to changes in state and rebuild accordingly.

### Which of the following is a best practice when using providers in Flutter?

- [x] Keep the provider focused on state management logic.
- [ ] Include UI-related code within the provider.
- [ ] Use global variables for state management.
- [ ] Avoid using `notifyListeners` to reduce complexity.

> **Explanation:** Providers should focus solely on state management logic, keeping UI-related code separate to maintain a clean architecture.

### What is the function of the `ListView.builder` in the `TodoListScreen` widget?

- [ ] To add new todos to the list.
- [x] To dynamically build the list of todos using the provider's data.
- [ ] To handle user authentication.
- [ ] To manage network requests.

> **Explanation:** `ListView.builder` is used to dynamically build the list of todos based on the data provided by the `TodoProvider`.

### How can thread safety be ensured when modifying the list of todos in a multi-threaded environment?

- [ ] By using global variables.
- [ ] By avoiding the use of `notifyListeners`.
- [x] By using synchronization mechanisms like `Mutex` or Dart's `synchronized` package.
- [ ] By storing data in local variables.

> **Explanation:** In a multi-threaded environment, synchronization mechanisms like `Mutex` or Dart's `synchronized` package can be used to ensure thread safety.

### What is the default value of the `isCompleted` property in the `Todo` class?

- [ ] `true`
- [x] `false`
- [ ] `null`
- [ ] `undefined`

> **Explanation:** The `isCompleted` property defaults to `false` in the `Todo` class, indicating that tasks are incomplete by default.

### Which package is used to integrate the `TodoProvider` into the widget tree?

- [ ] `http`
- [ ] `flutter_bloc`
- [x] `provider`
- [ ] `redux`

> **Explanation:** The `provider` package is used to integrate the `TodoProvider` into the widget tree, allowing widgets to access and listen to state changes.

### True or False: The `TodoProvider` should directly modify UI components to ensure efficient state management.

- [ ] True
- [x] False

> **Explanation:** False. The `TodoProvider` should focus on state management logic and not directly modify UI components, maintaining a separation of concerns.

{{< /quizdown >}}
