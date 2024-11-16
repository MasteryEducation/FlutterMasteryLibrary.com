---
linkTitle: "5.3.1 Hydrated Bloc"
title: "Hydrated Bloc: Persisting State in Flutter Applications"
description: "Learn how to use Hydrated Bloc for automatic state persistence in Flutter apps, ensuring seamless user experiences across app launches."
categories:
- Flutter
- State Management
- Bloc Pattern
tags:
- Flutter
- State Management
- Bloc
- Hydrated Bloc
- Persistence
date: 2024-10-25
type: docs
nav_weight: 531000
---

## 5.3.1 Hydrated Bloc: Persisting State in Flutter Applications

In the realm of mobile app development, ensuring that an application maintains its state across sessions is crucial for providing a seamless user experience. This is where the Hydrated Bloc package comes into play, offering an elegant solution for state persistence in Flutter applications. In this section, we'll explore the purpose of Hydrated Bloc, how to integrate it into your Flutter project, and best practices for its use.

### Purpose of Hydrated Bloc

Hydrated Bloc is an extension of the Bloc library, designed to automatically persist and restore the state of your Blocs. This means that when a user closes and reopens your app, the state is retained, providing a consistent experience. This feature is particularly useful for applications where state persistence is critical, such as in e-commerce apps where a user's cart should remain intact between sessions.

#### Key Benefits:
- **Automatic State Persistence:** Hydrated Bloc handles the serialization and deserialization of state, saving developers from implementing custom persistence logic.
- **Seamless User Experience:** By maintaining state across app launches, users enjoy a more fluid and consistent interaction with the app.
- **Reduced Boilerplate:** With Hydrated Bloc, much of the complexity of state management is abstracted away, allowing developers to focus on building features.

### Adding the `hydrated_bloc` Package

To get started with Hydrated Bloc, you need to add the package to your Flutter project. Open your `pubspec.yaml` file and include the following dependency:

```yaml
dependencies:
  flutter:
    sdk: flutter
  hydrated_bloc: ^8.0.0
  path_provider: ^2.0.0
```

The `path_provider` package is necessary to determine the appropriate storage directory for the persisted state.

### Setting Up HydratedBloc

Once the package is added, the next step is to set up `HydratedBlocStorage`. This involves initializing the storage in the `main` function before running the app. Here’s how you can do it:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:hydrated_bloc/hydrated_bloc.dart';
import 'package:path_provider/path_provider.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  HydratedBloc.storage = await HydratedStorage.build(
    storageDirectory: await getApplicationDocumentsDirectory(),
  );
  runApp(MyApp());
}
```

#### Explanation:
- **WidgetsFlutterBinding.ensureInitialized():** Ensures that the Flutter engine is fully initialized before any asynchronous operations.
- **HydratedStorage.build():** Creates a storage instance using the directory provided by `getApplicationDocumentsDirectory()`, which is platform-specific.

### Updating Bloc Classes

To leverage Hydrated Bloc, update your Bloc classes to extend `HydratedBloc` instead of the standard `Bloc`. This change allows your Bloc to automatically handle state persistence.

```dart
import 'package:hydrated_bloc/hydrated_bloc.dart';

class CounterBloc extends HydratedBloc<CounterEvent, int> {
  CounterBloc() : super(0);

  @override
  Stream<int> mapEventToState(CounterEvent event) async* {
    if (event is Increment) {
      yield state + 1;
    } else if (event is Decrement) {
      yield state - 1;
    }
  }

  @override
  int fromJson(Map<String, dynamic> json) => json['value'] as int;

  @override
  Map<String, dynamic> toJson(int state) => {'value': state};
}
```

#### Key Methods:
- **fromJson:** Converts the JSON map back into the Bloc's state.
- **toJson:** Serializes the state into a JSON map for storage.

### Implementing `fromJson` and `toJson`

The `fromJson` and `toJson` methods are crucial for defining how your state is serialized and deserialized. This allows Hydrated Bloc to store the state in a format that can be easily retrieved and reconstructed.

```dart
@override
int fromJson(Map<String, dynamic> json) => json['value'] as int;

@override
Map<String, dynamic> toJson(int state) => {'value': state};
```

#### Considerations:
- Ensure that the data types used in `fromJson` and `toJson` match the state type.
- Handle potential null values or missing keys gracefully to avoid runtime errors.

### Testing Persistence

To verify that your state is being persisted correctly, follow these steps:

1. **Run the App:** Launch your Flutter application.
2. **Interact with the App:** Perform actions that change the state, such as incrementing a counter.
3. **Close the App:** Terminate the app completely.
4. **Relaunch the App:** Open the app again and observe that the state is restored to its previous value.

This simple test confirms that the state persistence mechanism is functioning as expected.

### Best Practices

While Hydrated Bloc simplifies state persistence, there are several best practices to consider:

- **Data Security and Privacy:** Ensure that sensitive data is encrypted before being persisted. This is especially important for applications handling personal or financial information.
- **Versioning and Data Migrations:** As your application evolves, the structure of your state may change. Implement versioning in your `fromJson` and `toJson` methods to handle migrations gracefully.
- **Efficient State Management:** Only persist the necessary state to minimize storage usage and improve performance.
- **Testing Across Platforms:** Test your persistence logic on all target platforms (iOS, Android, etc.) to ensure consistent behavior.

### Real-World Application

Consider an e-commerce app where users can add items to a shopping cart. Using Hydrated Bloc, the cart's state can be persisted across app launches, ensuring that users don't lose their selections if they accidentally close the app.

```dart
class CartBloc extends HydratedBloc<CartEvent, List<Item>> {
  CartBloc() : super([]);

  @override
  Stream<List<Item>> mapEventToState(CartEvent event) async* {
    if (event is AddItem) {
      yield List.from(state)..add(event.item);
    } else if (event is RemoveItem) {
      yield List.from(state)..remove(event.item);
    }
  }

  @override
  List<Item> fromJson(Map<String, dynamic> json) {
    return (json['items'] as List)
        .map((item) => Item.fromJson(item))
        .toList();
  }

  @override
  Map<String, dynamic> toJson(List<Item> state) {
    return {'items': state.map((item) => item.toJson()).toList()};
  }
}
```

### Conclusion

Hydrated Bloc is a powerful tool for managing state persistence in Flutter applications. By following the steps outlined in this section, you can ensure that your app provides a consistent and reliable user experience across sessions. As you implement Hydrated Bloc, keep in mind the best practices discussed, particularly around data security and versioning, to build robust and future-proof applications.

### Additional Resources

- [Hydrated Bloc Documentation](https://pub.dev/packages/hydrated_bloc)
- [Bloc Library](https://bloclibrary.dev/#/)
- [Path Provider Documentation](https://pub.dev/packages/path_provider)

These resources provide further insights and examples to deepen your understanding of Hydrated Bloc and its capabilities.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of Hydrated Bloc in Flutter applications?

- [x] To automatically persist and restore the state across app launches.
- [ ] To handle network requests more efficiently.
- [ ] To improve the UI rendering performance.
- [ ] To manage user authentication.

> **Explanation:** Hydrated Bloc is designed to persist and restore the state of Blocs automatically, ensuring a seamless user experience across app sessions.

### Which package is essential for determining the storage directory for Hydrated Bloc?

- [x] path_provider
- [ ] shared_preferences
- [ ] sqflite
- [ ] http

> **Explanation:** The `path_provider` package is used to get the appropriate storage directory for persisting state with Hydrated Bloc.

### What method in Hydrated Bloc is used to convert the state into a JSON map?

- [x] toJson
- [ ] fromJson
- [ ] mapEventToState
- [ ] build

> **Explanation:** The `toJson` method is responsible for serializing the state into a JSON map for storage.

### How do you initialize HydratedBlocStorage in a Flutter app?

- [x] By calling `HydratedBloc.storage = await HydratedStorage.build(...)` in the `main` function.
- [ ] By extending the `Bloc` class.
- [ ] By using the `BlocProvider` widget.
- [ ] By implementing the `mapEventToState` method.

> **Explanation:** HydratedBlocStorage is initialized in the `main` function using `HydratedStorage.build`, which sets up the storage for state persistence.

### What should you do to handle changes in state structure over time?

- [x] Implement versioning and data migrations in `fromJson` and `toJson`.
- [ ] Use a different state management solution.
- [ ] Avoid persisting state altogether.
- [ ] Manually delete old state files.

> **Explanation:** Implementing versioning and data migrations in `fromJson` and `toJson` ensures that changes in state structure are handled gracefully.

### Which method is overridden to deserialize the state from JSON in Hydrated Bloc?

- [x] fromJson
- [ ] toJson
- [ ] mapEventToState
- [ ] build

> **Explanation:** The `fromJson` method is overridden to deserialize the state from a JSON map.

### What is a key benefit of using Hydrated Bloc?

- [x] It reduces boilerplate code for state persistence.
- [ ] It increases the app's network speed.
- [ ] It simplifies UI design.
- [ ] It enhances database performance.

> **Explanation:** Hydrated Bloc abstracts much of the complexity involved in state persistence, reducing boilerplate code.

### Which of the following is a best practice when using Hydrated Bloc?

- [x] Encrypt sensitive data before persisting it.
- [ ] Persist all possible state data to ensure completeness.
- [ ] Use Hydrated Bloc only for network-related states.
- [ ] Avoid using `toJson` and `fromJson` methods.

> **Explanation:** Encrypting sensitive data before persisting it is a best practice to ensure data security and privacy.

### What happens if you don't handle null values in `fromJson`?

- [x] It can lead to runtime errors.
- [ ] The app will automatically handle it.
- [ ] The state will reset to its initial value.
- [ ] The app will crash on startup.

> **Explanation:** Not handling null values in `fromJson` can lead to runtime errors, as the method might attempt to access non-existent keys.

### True or False: Hydrated Bloc can only be used for simple state management tasks.

- [ ] True
- [x] False

> **Explanation:** False. Hydrated Bloc can be used for both simple and complex state management tasks, providing automatic state persistence across app launches.

{{< /quizdown >}}
