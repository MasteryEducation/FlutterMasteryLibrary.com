---
linkTitle: "11.2.2 Saving Simple Data"
title: "Saving Simple Data with Shared Preferences in Flutter"
description: "Learn how to save simple data types using shared_preferences in Flutter, including strings, integers, booleans, and more. Explore best practices for managing keys and handling errors."
categories:
- Flutter Development
- Mobile App Development
- Data Persistence
tags:
- Flutter
- Shared Preferences
- Data Storage
- Mobile Development
- Dart
date: 2024-10-25
type: docs
nav_weight: 1122000
canonical: "https://fluttermasterylibrary.com/4/11/2/2"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 11.2.2 Saving Simple Data with Shared Preferences in Flutter

In mobile app development, persisting user data is crucial for maintaining a seamless user experience. Flutter provides several ways to achieve data persistence, and one of the simplest methods is through the `shared_preferences` package. This package allows you to store simple data types such as strings, integers, booleans, doubles, and lists of strings in a key-value pair format. This section will guide you through the process of saving simple data using `shared_preferences`, highlighting best practices and potential pitfalls.

### Supported Data Types

The `shared_preferences` package supports the following data types:

- **Strings**: Useful for storing text data like usernames or preferences.
- **Integers**: Ideal for storing numerical values such as user age or scores.
- **Doubles**: Suitable for storing decimal numbers, such as ratings or measurements.
- **Booleans**: Perfect for toggling states like login status or feature flags.
- **String Lists**: Useful for storing collections of strings, such as a list of favorite items.

### Storing Data

To store data using `shared_preferences`, you need to follow these steps:

1. **Add the Dependency**: First, ensure that the `shared_preferences` package is included in your `pubspec.yaml` file:

   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     shared_preferences: ^2.0.6
   ```

2. **Import the Package**: Import the `shared_preferences` package in your Dart file:

   ```dart
   import 'package:shared_preferences/shared_preferences.dart';
   ```

3. **Save Data**: Use the appropriate `set` method to store data. Here are examples for different data types:

   ```dart
   Future<void> saveUserName(String username) async {
     final prefs = await SharedPreferences.getInstance();
     await prefs.setString('username', username);
   }

   Future<void> saveUserAge(int age) async {
     final prefs = await SharedPreferences.getInstance();
     await prefs.setInt('user_age', age);
   }

   Future<void> saveUserLoggedIn(bool isLoggedIn) async {
     final prefs = await SharedPreferences.getInstance();
     await prefs.setBool('is_logged_in', isLoggedIn);
   }
   ```

   Each method is asynchronous, returning a `Future` that completes once the data is saved.

### Organizing Keys

When using `shared_preferences`, it's important to manage keys effectively to avoid conflicts and ensure data integrity:

- **Use Descriptive Names**: Keys should be descriptive and indicate the data they represent. For example, use `user_name` instead of `name`.
- **Consistent Naming Convention**: Adopt a consistent naming convention, such as snake_case or camelCase, to maintain readability.
- **Namespace Keys**: If your app has multiple modules, consider namespacing keys (e.g., `profile_user_name`) to prevent collisions.

### Error Handling

While `shared_preferences` is straightforward, it's essential to handle potential errors gracefully:

- **Check for Null Values**: When retrieving data, always check for null values to avoid runtime errors.
- **Handle Exceptions**: Use try-catch blocks to handle exceptions that may occur during data storage or retrieval.

### Practical Example: Saving User Preferences

Let's consider a practical example where we save user preferences such as theme settings and login status:

```dart
Future<void> saveThemePreference(bool isDarkMode) async {
  try {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setBool('is_dark_mode', isDarkMode);
  } catch (e) {
    print('Failed to save theme preference: $e');
  }
}

Future<void> saveLoginStatus(bool isLoggedIn) async {
  try {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setBool('is_logged_in', isLoggedIn);
  } catch (e) {
    print('Failed to save login status: $e');
  }
}
```

In this example, we use try-catch blocks to handle any exceptions that might occur during the saving process, ensuring that our app remains robust even in the face of errors.

### Visualizing the Data Saving Process

To better understand the flow of saving data using `shared_preferences`, consider the following Mermaid.js diagram:

```mermaid
graph LR
    A[App] --> B[User Action]
    B --> C[Call Save Function]
    C --> D[shared_preferences.setX]
    D --> E[Data Saved Persistently]
```

This diagram illustrates the process from user action to data being saved persistently, highlighting the role of `shared_preferences` in the data storage workflow.

### Best Practices and Common Pitfalls

- **Avoid Overusing Shared Preferences**: While convenient, `shared_preferences` is not suitable for large data sets or complex data structures. Use it for simple, lightweight data.
- **Regularly Clear Unused Data**: Periodically clear out unused or obsolete data to prevent clutter and potential performance issues.
- **Consider Security Implications**: Data stored in `shared_preferences` is not encrypted. Avoid storing sensitive information such as passwords or personal data.

### Conclusion

Using `shared_preferences` in Flutter is an effective way to persist simple data types, enhancing the user experience by retaining preferences and states across sessions. By following best practices for key management and error handling, you can ensure that your app remains reliable and user-friendly. As you continue to develop your Flutter applications, consider how `shared_preferences` can be integrated to improve data persistence and user satisfaction.

For further exploration, refer to the official [shared_preferences documentation](https://pub.dev/packages/shared_preferences) and consider experimenting with more complex data persistence solutions as your app's needs evolve.

## Quiz Time!

{{< quizdown >}}

### Which data types are supported by the `shared_preferences` package?

- [x] Strings
- [x] Integers
- [x] Booleans
- [ ] Complex Objects

> **Explanation:** `shared_preferences` supports simple data types like strings, integers, booleans, doubles, and string lists, but not complex objects directly.

### What is the correct method to save a boolean value using `shared_preferences`?

- [ ] setString
- [ ] setInt
- [x] setBool
- [ ] setDouble

> **Explanation:** The `setBool` method is used to save boolean values in `shared_preferences`.

### Why is it important to use descriptive names for keys in `shared_preferences`?

- [x] To avoid conflicts and ensure data integrity
- [ ] To make the app run faster
- [ ] To reduce the app size
- [ ] To improve network performance

> **Explanation:** Descriptive key names help avoid conflicts and ensure data integrity by clearly indicating the data they represent.

### What should you do if you encounter an error while saving data with `shared_preferences`?

- [ ] Ignore the error
- [x] Use a try-catch block to handle the exception
- [ ] Restart the app
- [ ] Reinstall the package

> **Explanation:** Using a try-catch block allows you to handle exceptions gracefully, ensuring the app remains robust.

### Which of the following is a best practice when using `shared_preferences`?

- [x] Regularly clear unused data
- [ ] Store large data sets
- [ ] Use complex data structures
- [ ] Encrypt all data

> **Explanation:** Regularly clearing unused data helps prevent clutter and potential performance issues.

### What is a potential security concern when using `shared_preferences`?

- [x] Data is not encrypted
- [ ] Data is too large
- [ ] Data is too complex
- [ ] Data is stored in the cloud

> **Explanation:** Data stored in `shared_preferences` is not encrypted, so sensitive information should not be stored there.

### How can you ensure that your app handles null values when retrieving data from `shared_preferences`?

- [x] Check for null values
- [ ] Use default values
- [ ] Ignore null values
- [ ] Restart the app

> **Explanation:** Checking for null values ensures that your app can handle cases where data might not be present.

### What is the first step to use `shared_preferences` in a Flutter project?

- [ ] Import the package
- [x] Add the dependency in `pubspec.yaml`
- [ ] Write the save function
- [ ] Create a new Dart file

> **Explanation:** Adding the dependency in `pubspec.yaml` is the first step to include `shared_preferences` in your project.

### Which method is used to save a list of strings in `shared_preferences`?

- [ ] setString
- [ ] setInt
- [ ] setBool
- [x] setStringList

> **Explanation:** The `setStringList` method is used to save a list of strings in `shared_preferences`.

### True or False: `shared_preferences` is suitable for storing large data sets.

- [ ] True
- [x] False

> **Explanation:** `shared_preferences` is not suitable for large data sets; it is designed for simple, lightweight data storage.

{{< /quizdown >}}
