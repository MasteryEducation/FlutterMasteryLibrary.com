---
linkTitle: "7.1.4 Displaying Fetched Data in the UI"
title: "Displaying Fetched Data in the UI: A Comprehensive Guide to Using FutureBuilder in Flutter"
description: "Learn how to effectively display fetched data in Flutter using FutureBuilder, handle asynchronous states, and optimize your app's UI responsiveness."
categories:
- Flutter Development
- Mobile App Development
- UI/UX Design
tags:
- Flutter
- FutureBuilder
- Async Programming
- UI Design
- Mobile Development
date: 2024-10-25
type: docs
nav_weight: 714000
canonical: "https://fluttermasterylibrary.com/1/7/1/4"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 7.1.4 Displaying Fetched Data in the UI

In the world of mobile app development, displaying data fetched from a network or database is a common requirement. Flutter provides a powerful widget called `FutureBuilder` that allows developers to build widgets based on the state of a `Future`. This section will guide you through the process of displaying fetched data in the UI using `FutureBuilder`, understanding the snapshot properties, handling different states, and best practices for maintaining a responsive UI.

### Creating an Async Function

Before we dive into using `FutureBuilder`, it's essential to understand how to create asynchronous functions in Flutter. Asynchronous functions are necessary for performing network calls or any operation that might take some time to complete. In Flutter, these functions return `Future` objects.

Here's a simple example of an asynchronous function that fetches user data:

```dart
Future<User> fetchUser() async {
  // Simulate a network call
  await Future.delayed(Duration(seconds: 2));
  return User(name: 'John Doe', age: 30);
}
```

In this example, `fetchUser` is an asynchronous function that simulates a network call by delaying for 2 seconds before returning a `User` object.

### Using `FutureBuilder`

`FutureBuilder` is a widget that builds itself based on the latest snapshot of interaction with a `Future`. It is a powerful tool for handling asynchronous data in Flutter.

#### Example Usage

Let's see how to use `FutureBuilder` to display user data fetched by the `fetchUser` function:

```dart
@override
Widget build(BuildContext context) {
  return FutureBuilder<User>(
    future: fetchUser(),
    builder: (context, snapshot) {
      if (snapshot.connectionState == ConnectionState.waiting) {
        return CircularProgressIndicator();
      } else if (snapshot.hasError) {
        return Text('Error: ${snapshot.error}');
      } else if (snapshot.hasData) {
        User user = snapshot.data!;
        return Text('Hello, ${user.name}');
      } else {
        return Text('No data available.');
      }
    },
  );
}
```

In this example, `FutureBuilder` takes a `Future<User>` and a builder function. The builder function is called whenever the `Future`'s state changes, and it receives a `snapshot` containing the current state of the `Future`.

### Understanding `snapshot`

The `snapshot` object in the `FutureBuilder` builder function provides several properties that help you understand the current state of the `Future`.

#### Properties

- **`connectionState`**: Indicates the state of the asynchronous computation. It can be one of the following:
  - `none`: No connection is initiated.
  - `waiting`: The `Future` is waiting for data.
  - `active`: The `Future` is active (rarely used).
  - `done`: The `Future` has completed.

- **`data`**: Contains the data returned by the `Future` if it has completed successfully.

- **`error`**: Contains any error thrown during the `Future`'s execution.

#### Connection States

Understanding the connection states is crucial for providing appropriate UI feedback to the user.

- **Loading State**: When `connectionState` is `waiting`, it's a good practice to show a loading indicator, such as `CircularProgressIndicator`, to inform the user that data is being fetched.

- **Error State**: If `snapshot.hasError` is true, display an error message. Ensure that the error message is user-friendly and provides enough information for the user to understand what went wrong.

- **Data Available**: When `snapshot.hasData` is true, it means the `Future` has completed successfully, and you can display the fetched data.

### Handling Different States

Let's explore how to handle different states in `FutureBuilder` with a more detailed example:

```dart
@override
Widget build(BuildContext context) {
  return FutureBuilder<User>(
    future: fetchUser(),
    builder: (context, snapshot) {
      switch (snapshot.connectionState) {
        case ConnectionState.none:
          return Text('Press button to start.');
        case ConnectionState.waiting:
          return CircularProgressIndicator();
        case ConnectionState.active:
          return Text('Fetching data...');
        case ConnectionState.done:
          if (snapshot.hasError) {
            return Text('Error: ${snapshot.error}');
          } else if (snapshot.hasData) {
            User user = snapshot.data!;
            return Text('Hello, ${user.name}');
          } else {
            return Text('No data available.');
          }
      }
    },
  );
}
```

In this example, we use a `switch` statement to handle different connection states. This approach makes the code more readable and easier to maintain.

### Using `ListView.builder` with Fetched Data

When fetching a list of items, you can use `ListView.builder` inside `FutureBuilder` to display the data efficiently. Here's how you can do it:

```dart
Future<List<Item>> fetchItems() async {
  // Simulate a network call
  await Future.delayed(Duration(seconds: 2));
  return List.generate(10, (index) => Item(title: 'Item $index'));
}

@override
Widget build(BuildContext context) {
  return FutureBuilder<List<Item>>(
    future: fetchItems(),
    builder: (context, snapshot) {
      if (snapshot.connectionState == ConnectionState.waiting) {
        return CircularProgressIndicator();
      } else if (snapshot.hasError) {
        return Text('Error: ${snapshot.error}');
      } else if (snapshot.hasData) {
        return ListView.builder(
          itemCount: snapshot.data!.length,
          itemBuilder: (context, index) {
            Item item = snapshot.data![index];
            return ListTile(
              title: Text(item.title),
            );
          },
        );
      } else {
        return Text('No data available.');
      }
    },
  );
}
```

In this example, `fetchItems` is an asynchronous function that returns a list of `Item` objects. `FutureBuilder` is used to build a `ListView` once the data is available.

### Best Practices

To ensure a smooth user experience and maintainable code, consider the following best practices:

- **Keep the UI Responsive**: Always show loading indicators when data is being fetched. This provides feedback to the user and prevents the UI from appearing frozen.

- **User-Friendly Error Messages**: Display clear and concise error messages. Avoid technical jargon that might confuse the user.

- **Separation of Concerns**: Separate UI code from data fetching logic. This can be achieved by using services or repositories to handle data fetching, making your code more modular and easier to test.

- **Error Handling**: Implement robust error handling to manage network failures or unexpected errors gracefully.

- **Optimize Network Calls**: Cache data when possible to reduce network calls and improve performance.

### Practice Exercises

To reinforce your understanding of displaying fetched data in the UI, try the following exercises:

1. **Build a Simple News App**: Create a news app that fetches articles from an API and displays them in a list. Use `FutureBuilder` to handle the asynchronous data fetching.

2. **Implement Pull-to-Refresh**: Enhance the news app by adding pull-to-refresh functionality, allowing users to refresh the list of articles manually.

3. **Error Handling Exercise**: Modify the news app to handle network errors gracefully. Display a retry button when an error occurs.

4. **Data Caching**: Implement data caching in the news app to improve performance and reduce network usage.

5. **UI Enhancements**: Experiment with different loading indicators and error messages to improve the user experience.

### Conclusion

Displaying fetched data in the UI is a fundamental aspect of mobile app development. By using `FutureBuilder`, you can efficiently manage asynchronous data fetching and provide a responsive user interface. Remember to handle different states appropriately and follow best practices to ensure a smooth user experience.

---

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of the `FutureBuilder` widget in Flutter?

- [x] To build widgets based on the state of a `Future`.
- [ ] To handle user input events.
- [ ] To manage stateful widgets.
- [ ] To create animations.

> **Explanation:** `FutureBuilder` is designed to build widgets based on the state of a `Future`, allowing developers to handle asynchronous data fetching efficiently.

### Which `ConnectionState` indicates that a `Future` has completed?

- [ ] none
- [ ] waiting
- [ ] active
- [x] done

> **Explanation:** The `done` state indicates that the `Future` has completed, whether successfully or with an error.

### How should you handle the loading state in a `FutureBuilder`?

- [ ] Display an error message.
- [x] Show a loading indicator like `CircularProgressIndicator`.
- [ ] Display the fetched data immediately.
- [ ] Do nothing.

> **Explanation:** Showing a loading indicator provides feedback to the user that data is being fetched, improving the user experience.

### What should you display if `snapshot.hasError` is true?

- [ ] The fetched data.
- [x] An error message.
- [ ] A loading indicator.
- [ ] A success message.

> **Explanation:** If `snapshot.hasError` is true, it means an error occurred during the `Future`'s execution, and an error message should be displayed.

### In the context of `FutureBuilder`, what does `snapshot.data` contain?

- [x] The data returned by the `Future` if it has completed successfully.
- [ ] The error thrown by the `Future`.
- [ ] The current connection state.
- [ ] The initial data.

> **Explanation:** `snapshot.data` contains the data returned by the `Future` once it has completed successfully.

### Why is it important to separate UI code from data fetching logic?

- [x] To make the code more modular and easier to test.
- [ ] To increase the complexity of the code.
- [ ] To reduce the number of files in the project.
- [ ] To improve the app's performance.

> **Explanation:** Separating UI code from data fetching logic helps in creating modular, maintainable, and testable code.

### What is a common practice to optimize network calls in a Flutter app?

- [ ] Increase the number of network calls.
- [x] Cache data to reduce network calls.
- [ ] Use synchronous network calls.
- [ ] Avoid error handling.

> **Explanation:** Caching data helps in reducing the number of network calls, improving performance and user experience.

### Which widget is commonly used to display a list of items fetched from a network?

- [ ] GridView
- [x] ListView.builder
- [ ] Column
- [ ] Stack

> **Explanation:** `ListView.builder` is commonly used to efficiently display a list of items fetched from a network.

### What is the benefit of showing a loading indicator during data fetching?

- [x] It provides feedback to the user, indicating that data is being fetched.
- [ ] It increases the app's complexity.
- [ ] It hides the UI from the user.
- [ ] It slows down the app.

> **Explanation:** A loading indicator informs the user that data is being fetched, enhancing the user experience by preventing the UI from appearing frozen.

### True or False: `FutureBuilder` can only be used with network calls.

- [ ] True
- [x] False

> **Explanation:** `FutureBuilder` can be used with any asynchronous operation, not just network calls. It can handle any `Future`, such as database queries or file operations.

{{< /quizdown >}}
