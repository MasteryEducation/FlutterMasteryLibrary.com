---
linkTitle: "7.3.2 Implementing Stores"
title: "Implementing Stores with MobX in Flutter"
description: "Explore how to implement stores using MobX in Flutter for efficient state management, focusing on creating and managing a task list."
categories:
- Flutter
- State Management
- MobX
tags:
- Flutter
- MobX
- State Management
- Task Management
- Dart
date: 2024-10-25
type: docs
nav_weight: 732000
---

## 7.3.2 Implementing Stores with MobX in Flutter

In this section, we will delve into the implementation of stores using MobX in Flutter, focusing on creating a task management application. MobX is a powerful state management library that leverages reactive programming principles to make state management intuitive and scalable. By the end of this article, you will have a comprehensive understanding of how to implement and utilize stores in MobX to manage application state effectively.

### Understanding MobX Stores

MobX stores are central repositories where the state of your application resides. They are responsible for holding the state and defining the logic to manipulate it. In MobX, stores are typically implemented as classes that contain observables, actions, and computed properties. Observables represent the reactive state, actions define the logic to modify the state, and computed properties derive new state from existing observables.

### Defining the Task Model

Before we create our store, we need a model to represent the tasks in our application. The `Task` model will encapsulate the properties of a task, such as its title, description, and completion status.

Create a new file `models/task.dart` and define the `Task` class as follows:

```dart
class Task {
  String id;
  String title;
  String description;
  bool isCompleted;

  Task({
    required this.id,
    required this.title,
    required this.description,
    this.isCompleted = false,
  });
}
```

This simple class provides a blueprint for our tasks, with an `id`, `title`, `description`, and a boolean `isCompleted` to track whether the task is done.

### Creating the Task Store

With the task model in place, we can now create a MobX store to manage a list of tasks. The store will handle adding, removing, and toggling the completion status of tasks.

#### Setting Up the Store

Create a new file `stores/task_store.dart`. Start by importing the necessary packages and setting up the code generation part:

```dart
import 'package:mobx/mobx.dart';
import '../models/task.dart';
part 'task_store.g.dart';

class TaskStore = _TaskStore with _$TaskStore;
```

The `part 'task_store.g.dart';` directive is crucial for MobX code generation, which we'll discuss further.

#### Defining the Store Class

Define the `_TaskStore` class with observables and actions:

```dart
abstract class _TaskStore with Store {
  @observable
  ObservableList<Task> taskList = ObservableList<Task>();

  @action
  void addTask(Task task) {
    taskList.add(task);
  }

  @action
  void removeTask(String id) {
    taskList.removeWhere((task) => task.id == id);
  }

  @action
  void toggleTaskStatus(String id) {
    final index = taskList.indexWhere((task) => task.id == id);
    if (index != -1) {
      taskList[index].isCompleted = !taskList[index].isCompleted;
    }
  }
}
```

- **Observables**: `ObservableList<Task>` is used to store the list of tasks. This makes the list reactive, meaning any changes to it will automatically update any UI components observing it.
- **Actions**: Methods like `addTask`, `removeTask`, and `toggleTaskStatus` are marked with the `@action` decorator, indicating that they modify the state.

### Implementing Computed Properties

Computed properties in MobX allow you to derive new state from existing observables. In our task store, we can create computed properties to filter tasks based on their completion status.

Add the following computed properties to your `_TaskStore` class:

```dart
@computed
List<Task> get completedTasks =>
    taskList.where((task) => task.isCompleted).toList();

@computed
List<Task> get pendingTasks =>
    taskList.where((task) => !task.isCompleted).toList();
```

- **Completed Tasks**: Filters the `taskList` to return only tasks that are marked as completed.
- **Pending Tasks**: Filters the `taskList` to return tasks that are not yet completed.

### Running Code Generation

MobX uses code generation to create the necessary boilerplate code for observables and actions. To generate the `.g.dart` files, run the following command in your terminal:

```bash
flutter pub run build_runner build
```

This command will generate the `task_store.g.dart` file, which contains the code required for MobX to function correctly.

### Best Practices

- **Focus on State Management**: Keep your store focused on managing state and business logic. Avoid mixing UI logic with your store.
- **Annotate Observables**: Ensure all observables are properly annotated with `@observable` and actions with `@action`.
- **Keep Code Generation Updated**: Regularly run the build runner to keep your generated code in sync with your store definitions.

### Key Takeaways

- **Centralized State Management**: MobX stores provide a centralized location for managing application state, making your codebase more organized and maintainable.
- **Reactive Updates**: With MobX, changes to the state automatically propagate to the UI, reducing the need for manual updates.
- **Scalable Architecture**: As your application grows, MobX stores can be easily extended to accommodate new features and requirements.

### Practical Example

Let's consider a practical example of how you might integrate this store into a Flutter application. Imagine a simple task management app where users can add, remove, and toggle tasks.

#### Integrating the Store with Flutter

To use the `TaskStore` in your Flutter application, you'll typically provide it using a state management solution like `Provider` or directly within your widget tree.

Here's a basic example of how you might use the `TaskStore` in a Flutter widget:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_mobx/flutter_mobx.dart';
import 'stores/task_store.dart';

class TaskListPage extends StatelessWidget {
  final TaskStore taskStore = TaskStore();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Task List')),
      body: Observer(
        builder: (_) => ListView.builder(
          itemCount: taskStore.taskList.length,
          itemBuilder: (context, index) {
            final task = taskStore.taskList[index];
            return ListTile(
              title: Text(task.title),
              subtitle: Text(task.description),
              trailing: Checkbox(
                value: task.isCompleted,
                onChanged: (value) {
                  taskStore.toggleTaskStatus(task.id);
                },
              ),
            );
          },
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Add a new task
          taskStore.addTask(Task(
            id: DateTime.now().toString(),
            title: 'New Task',
            description: 'Task Description',
          ));
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

- **Observer Widget**: The `Observer` widget listens for changes in the `taskStore` and rebuilds the UI accordingly.
- **Task Management**: Users can toggle task completion using a checkbox, and new tasks can be added with a floating action button.

### Conclusion

Implementing stores with MobX in Flutter offers a powerful way to manage application state. By leveraging observables, actions, and computed properties, you can create responsive and scalable applications. As you continue to explore MobX, consider experimenting with more complex state management scenarios and integrating MobX with other libraries to enhance your application's capabilities.

### Further Exploration

- **MobX Documentation**: [MobX Documentation](https://mobx.netlify.app/)
- **Flutter MobX Package**: [Flutter MobX](https://pub.dev/packages/flutter_mobx)
- **Code Examples**: Explore the full code on [GitHub](https://github.com/mobxjs/mobx.dart)

## Quiz Time!

{{< quizdown >}}

### What is the primary role of a MobX store in a Flutter application?

- [x] To manage and centralize application state
- [ ] To handle UI rendering
- [ ] To manage network requests
- [ ] To store user preferences

> **Explanation:** MobX stores are responsible for managing and centralizing the application state, making it easier to maintain and scale.

### Which decorator is used to mark a method as an action in MobX?

- [ ] @observable
- [x] @action
- [ ] @computed
- [ ] @store

> **Explanation:** The `@action` decorator is used to mark methods that modify the state in MobX.

### What is the purpose of computed properties in MobX?

- [ ] To store static data
- [x] To derive new state from existing observables
- [ ] To handle asynchronous operations
- [ ] To manage UI components

> **Explanation:** Computed properties in MobX are used to derive new state from existing observables, allowing for efficient state management.

### How do you generate the necessary MobX boilerplate code in Flutter?

- [ ] By manually writing it
- [ ] By using Flutter's build tools
- [x] By running `flutter pub run build_runner build`
- [ ] By using a third-party library

> **Explanation:** Running `flutter pub run build_runner build` generates the necessary MobX boilerplate code.

### What is the advantage of using `ObservableList` in MobX?

- [x] It makes the list reactive, automatically updating the UI on changes
- [ ] It improves list sorting
- [ ] It enhances list filtering
- [ ] It reduces memory usage

> **Explanation:** `ObservableList` makes the list reactive, ensuring that any changes to the list automatically update the UI components observing it.

### Which of the following is NOT a feature of MobX?

- [ ] Observables
- [ ] Actions
- [ ] Computed properties
- [x] Middleware

> **Explanation:** Middleware is not a feature of MobX. MobX focuses on observables, actions, and computed properties for state management.

### What is the purpose of the `part 'task_store.g.dart';` directive in the store file?

- [ ] To import external libraries
- [x] To include generated code for MobX
- [ ] To define new classes
- [ ] To manage dependencies

> **Explanation:** The `part 'task_store.g.dart';` directive includes the generated code necessary for MobX to function.

### How can you filter tasks based on their completion status in a MobX store?

- [ ] By using actions
- [ ] By using observables
- [x] By using computed properties
- [ ] By using middleware

> **Explanation:** Computed properties are used to filter tasks based on their completion status in a MobX store.

### What is a key benefit of using MobX for state management in Flutter?

- [x] Reactive updates to the UI
- [ ] Simplified UI design
- [ ] Enhanced network performance
- [ ] Reduced code complexity

> **Explanation:** MobX provides reactive updates to the UI, automatically reflecting state changes without manual intervention.

### True or False: MobX stores should contain UI logic to improve performance.

- [ ] True
- [x] False

> **Explanation:** MobX stores should focus on state management and business logic, not UI logic, to maintain a clean architecture.

{{< /quizdown >}}
