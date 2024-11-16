---
linkTitle: "3.3.3 Building the User Interface"
title: "Building the User Interface for a Todo App with Provider in Flutter"
description: "Learn how to build a user interface for a Todo app using the Provider package in Flutter. This guide covers designing the main screen, creating a Todo item widget, building add/edit screens, implementing navigation, and ensuring UI updates."
categories:
- Flutter Development
- State Management
- Mobile App Development
tags:
- Flutter
- Provider
- State Management
- Todo App
- UI Design
date: 2024-10-25
type: docs
nav_weight: 333000
canonical: "https://fluttermasterylibrary.com/7/3/3/3"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 3.3.3 Building the User Interface

In this section, we will explore how to build a user interface for a Todo app using the Provider package in Flutter. This comprehensive guide will walk you through designing the main screen to display the list of todos, creating a Todo item widget, building add/edit screens, implementing navigation, and ensuring that the UI updates in response to changes in the provider. By the end of this section, you will have a functional Todo app UI that effectively utilizes the Provider for state management.

### Todo List Screen

The Todo List Screen is the heart of our application, where users can view all their todos. We'll use a `ListView.builder` to create a scrollable list of todos. This approach is efficient for displaying large lists as it only builds the widgets that are visible on the screen.

#### Designing the Main Screen

To design the main screen, we will use the `Consumer<TodoProvider>` widget to access the list of todos from the provider. This allows us to rebuild the UI whenever the list of todos changes.

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'todo_provider.dart'; // Assume this is where your provider is defined

class TodoListScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Todo List'),
      ),
      body: Consumer<TodoProvider>(
        builder: (context, todoProvider, child) {
          return ListView.builder(
            itemCount: todoProvider.todos.length,
            itemBuilder: (context, index) {
              var todo = todoProvider.todos[index];
              return TodoItemWidget(todo: todo);
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => AddEditTodoScreen()),
          );
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

**Key Points:**

- **Consumer Widget:** We use `Consumer<TodoProvider>` to listen to changes in the `TodoProvider`. This ensures that the `ListView` updates whenever the list of todos changes.
- **ListView.builder:** This widget efficiently builds a scrollable list of todos.
- **FloatingActionButton:** Provides a way to navigate to the `AddEditTodoScreen` for adding new todos.

### Todo Item Widget

Each todo item in the list will be represented by a separate widget. This widget will display the title and completion status of the todo and provide interaction capabilities such as tapping or swiping to edit or delete the todo.

#### Creating the Todo Item Widget

```dart
import 'package:flutter/material.dart';
import 'todo.dart'; // Assume this is your Todo model

class TodoItemWidget extends StatelessWidget {
  final Todo todo;

  TodoItemWidget({required this.todo});

  @override
  Widget build(BuildContext context) {
    return ListTile(
      title: Text(todo.title),
      trailing: Checkbox(
        value: todo.isCompleted,
        onChanged: (bool? value) {
          // Update the completion status
        },
      ),
      onTap: () {
        // Navigate to edit screen
      },
      onLongPress: () {
        // Show options to delete
      },
    );
  }
}
```

**Key Points:**

- **ListTile:** Provides a simple way to display a title and a trailing widget, such as a checkbox.
- **Checkbox:** Allows users to mark a todo as completed. You can update the todo's completion status here.
- **Gesture Detection:** Use `onTap` and `onLongPress` to handle interactions like editing or deleting a todo.

### Add/Edit Todo Screen

This screen will allow users to add a new todo or edit an existing one. We'll use `TextFormField` widgets for input and implement form validation to ensure that users provide valid data.

#### Building the Add/Edit Todo Screen

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'todo_provider.dart';
import 'todo.dart';

class AddEditTodoScreen extends StatefulWidget {
  final Todo? todo;

  AddEditTodoScreen({this.todo});

  @override
  _AddEditTodoScreenState createState() => _AddEditTodoScreenState();
}

class _AddEditTodoScreenState extends State<AddEditTodoScreen> {
  final _formKey = GlobalKey<FormState>();
  late String _title;

  @override
  void initState() {
    super.initState();
    _title = widget.todo?.title ?? '';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.todo == null ? 'Add Todo' : 'Edit Todo'),
      ),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                initialValue: _title,
                decoration: InputDecoration(labelText: 'Title'),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Please enter a title';
                  }
                  return null;
                },
                onSaved: (value) {
                  _title = value!;
                },
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    _formKey.currentState!.save();
                    if (widget.todo == null) {
                      Provider.of<TodoProvider>(context, listen: false)
                          .addTodo(Todo(title: _title));
                    } else {
                      Provider.of<TodoProvider>(context, listen: false)
                          .updateTodo(widget.todo!.copyWith(title: _title));
                    }
                    Navigator.pop(context);
                  }
                },
                child: Text(widget.todo == null ? 'Add' : 'Update'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

**Key Points:**

- **Form and Validation:** We use a `Form` widget with a `GlobalKey` to validate and save form data.
- **TextFormField:** Provides input fields with validation logic.
- **Provider Usage:** We use the provider to add or update a todo based on whether we're adding a new todo or editing an existing one.

### Navigation

Navigation between screens is crucial for a seamless user experience. We'll use `Navigator.push` to transition between the Todo List Screen and the Add/Edit Todo Screen.

#### Implementing Navigation

The navigation logic is already integrated into the `FloatingActionButton` and `onTap` methods in the previous examples. Here's a quick recap:

- **Add Todo:** Use the `FloatingActionButton` to navigate to the `AddEditTodoScreen`.
- **Edit Todo:** Use the `onTap` method in `TodoItemWidget` to navigate to the `AddEditTodoScreen` with the selected todo.

### Updating the UI

Ensuring that the UI updates in response to changes in the provider is essential for a responsive app. The `Consumer` widget handles this by listening to changes in the provider and rebuilding the UI accordingly.

#### Testing UI Updates

To verify that the UI updates correctly, perform the following tests:

- **Add a Todo:** Use the Add/Edit Todo Screen to add a new todo and check that it appears in the list.
- **Edit a Todo:** Tap a todo to edit it and ensure the changes are reflected in the list.
- **Delete a Todo:** Implement a delete option and verify that the todo is removed from the list.

### Conclusion

Building a user interface for a Todo app using the Provider package in Flutter involves designing a responsive main screen, creating interactive todo item widgets, building forms for adding and editing todos, and implementing seamless navigation. By following the steps outlined in this guide, you can create a functional and efficient UI that leverages the power of Provider for state management.

### Additional Resources

- [Flutter Documentation](https://flutter.dev/docs)
- [Provider Package Documentation](https://pub.dev/packages/provider)
- [State Management in Flutter](https://flutter.dev/docs/development/data-and-backend/state-mgmt)

## Quiz Time!

{{< quizdown >}}

### What widget is used to efficiently display a scrollable list of todos?

- [x] ListView.builder
- [ ] Column
- [ ] Row
- [ ] GridView

> **Explanation:** `ListView.builder` is used to efficiently display a scrollable list of items by only building the widgets that are visible on the screen.

### Which widget is used to listen to changes in the provider and rebuild the UI?

- [x] Consumer
- [ ] Provider
- [ ] StreamBuilder
- [ ] FutureBuilder

> **Explanation:** The `Consumer` widget listens to changes in the provider and rebuilds the UI when the provider's state changes.

### What is the purpose of the `GlobalKey<FormState>` in the Add/Edit Todo Screen?

- [x] To manage the form's state and validation
- [ ] To uniquely identify the form widget
- [ ] To store the form's data
- [ ] To navigate between screens

> **Explanation:** `GlobalKey<FormState>` is used to manage the form's state and validation, allowing us to validate and save form data.

### How do you navigate to the Add/Edit Todo Screen from the main screen?

- [x] Using Navigator.push
- [ ] Using Navigator.pop
- [ ] Using Navigator.replace
- [ ] Using Navigator.remove

> **Explanation:** `Navigator.push` is used to navigate to a new screen by adding it to the navigation stack.

### What method is used to add a new todo in the provider?

- [x] addTodo
- [ ] createTodo
- [ ] insertTodo
- [ ] saveTodo

> **Explanation:** The `addTodo` method is used to add a new todo to the list managed by the provider.

### What widget is used to display a checkbox in each todo item?

- [x] Checkbox
- [ ] Switch
- [ ] Radio
- [ ] ToggleButton

> **Explanation:** The `Checkbox` widget is used to display a checkbox, allowing users to mark a todo as completed.

### How can you ensure that the UI updates when a todo is added or edited?

- [x] By using Consumer to listen to provider changes
- [ ] By calling setState manually
- [ ] By using a StatefulWidget
- [ ] By using a StreamBuilder

> **Explanation:** Using `Consumer` ensures that the UI updates automatically when the provider's state changes.

### What gesture detection methods are used in the Todo Item Widget?

- [x] onTap and onLongPress
- [ ] onSwipe and onDrag
- [ ] onDoubleTap and onHover
- [ ] onScroll and onZoom

> **Explanation:** `onTap` and `onLongPress` are used to detect user interactions such as tapping or long-pressing on a todo item.

### What is the purpose of the `TextFormField` widget in the Add/Edit Todo Screen?

- [x] To provide input fields with validation
- [ ] To display static text
- [ ] To create a button
- [ ] To manage navigation

> **Explanation:** `TextFormField` provides input fields with built-in validation capabilities, making it ideal for forms.

### True or False: The `FloatingActionButton` is used to delete todos from the list.

- [ ] True
- [x] False

> **Explanation:** The `FloatingActionButton` is used to navigate to the Add/Edit Todo Screen, not to delete todos.

{{< /quizdown >}}
