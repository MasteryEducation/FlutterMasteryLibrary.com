---
linkTitle: "6.3.1 Project Setup: Shopping Cart App"
title: "Shopping Cart App Project Setup with Redux in Flutter"
description: "Learn how to set up a shopping cart application using Redux in Flutter, including architecture design, state management, and key implementation steps."
categories:
- Flutter Development
- State Management
- Redux
tags:
- Flutter
- Redux
- Shopping Cart
- State Management
- App Development
date: 2024-10-25
type: docs
nav_weight: 631000
---

## 6.3.1 Project Setup: Shopping Cart App

In this section, we will embark on an exciting journey to build a shopping cart application using Redux in Flutter. This project will serve as a practical example to understand how Redux can be utilized to manage state effectively in a Flutter application. We'll cover everything from setting up the project to designing the architecture and implementing key features. By the end of this guide, you'll have a solid foundation to build and expand upon a shopping cart app using Redux.

### App Overview

The primary goal of this project is to create a shopping cart application that allows users to interact with a list of products. The app will include the following features:

- **Viewing a List of Products:** Users can browse through a catalog of products.
- **Adding and Removing Products from the Cart:** Users can add products to their shopping cart and remove them as needed.
- **Updating Quantities:** Users can adjust the quantity of each product in their cart.
- **Viewing the Total Cost:** The app will calculate and display the total cost of the items in the cart.

These features will provide a comprehensive understanding of how to manage state using Redux in a Flutter application.

### Designing the Architecture

Before diving into the code, it's crucial to plan out the architecture of the application. A well-thought-out architecture will make the development process smoother and the code more maintainable.

#### State Structure

The state of our application will be managed by a central `AppState` class. This class will hold the list of available products and the items currently in the cart.

```dart
class AppState {
  final List<Product> products;
  final List<CartItem> cartItems;

  AppState({required this.products, required this.cartItems});

  AppState.initial()
      : products = initialProductList,
        cartItems = [];
}
```

- **Products:** A list of all available products in the store.
- **CartItems:** A list of items that have been added to the cart.

#### Actions

In Redux, actions are payloads of information that send data from your application to your Redux store. We'll define the following actions:

- **AddToCartAction:** Adds a product to the cart.
- **RemoveFromCartAction:** Removes a product from the cart.
- **UpdateQuantityAction:** Updates the quantity of a specific product in the cart.

```dart
class AddToCartAction {
  final Product product;

  AddToCartAction(this.product);
}

class RemoveFromCartAction {
  final Product product;

  RemoveFromCartAction(this.product);
}

class UpdateQuantityAction {
  final Product product;
  final int quantity;

  UpdateQuantityAction(this.product, this.quantity);
}
```

#### Reducers

Reducers specify how the application's state changes in response to actions sent to the store. We'll implement reducers to handle the defined actions and update the state accordingly.

```dart
AppState appReducer(AppState state, dynamic action) {
  if (action is AddToCartAction) {
    return AppState(
      products: state.products,
      cartItems: List.from(state.cartItems)..add(CartItem(product: action.product, quantity: 1)),
    );
  } else if (action is RemoveFromCartAction) {
    return AppState(
      products: state.products,
      cartItems: state.cartItems.where((item) => item.product != action.product).toList(),
    );
  } else if (action is UpdateQuantityAction) {
    return AppState(
      products: state.products,
      cartItems: state.cartItems.map((item) {
        if (item.product == action.product) {
          return CartItem(product: item.product, quantity: action.quantity);
        }
        return item;
      }).toList(),
    );
  }
  return state;
}
```

### Setting Up the Project

Let's start by setting up the Flutter project and adding the necessary dependencies.

#### Create a New Flutter Project

Open your terminal and run the following command to create a new Flutter project:

```bash
flutter create shopping_cart_redux
```

Navigate into the project directory:

```bash
cd shopping_cart_redux
```

#### Add Redux and Flutter Redux Dependencies

Open the `pubspec.yaml` file and add the `redux` and `flutter_redux` packages:

```yaml
dependencies:
  flutter:
    sdk: flutter
  redux: ^5.0.0
  flutter_redux: ^0.8.2
```

Run `flutter pub get` to install the new dependencies.

### Organizing Files

A well-organized file structure is essential for maintaining a clean and scalable codebase. Here's how we'll structure our project:

- **lib/actions/**: Contains all the action classes.
- **lib/models/**: Contains data models such as `Product` and `CartItem`.
- **lib/reducers/**: Contains the reducer functions.
- **lib/middleware/**: (Optional) Contains middleware functions if needed.
- **lib/ui/**: Contains all the UI components and screens.

#### Create the Necessary Folders

Inside the `lib` directory, create the following folders:

```bash
mkdir lib/actions lib/models lib/reducers lib/middleware lib/ui
```

### Key Takeaways

- **Planning is Crucial:** Before writing any code, take the time to plan out the architecture and state management strategy. This will save time and reduce errors during development.
- **Clear Architecture:** A well-defined architecture makes the codebase easier to understand and maintain. It also facilitates collaboration among team members.
- **Redux for State Management:** Redux provides a predictable state container, making it easier to manage and debug state changes in your application.

### Practical Code Examples

Let's implement some of the core components of our shopping cart app.

#### Product Model

Create a `Product` model in `lib/models/product.dart`:

```dart
class Product {
  final String id;
  final String name;
  final double price;

  Product({required this.id, required this.name, required this.price});
}
```

#### CartItem Model

Create a `CartItem` model in `lib/models/cart_item.dart`:

```dart
class CartItem {
  final Product product;
  final int quantity;

  CartItem({required this.product, required this.quantity});
}
```

#### Initial Product List

Define an initial list of products in `lib/models/initial_products.dart`:

```dart
import 'product.dart';

final List<Product> initialProductList = [
  Product(id: '1', name: 'Laptop', price: 999.99),
  Product(id: '2', name: 'Smartphone', price: 499.99),
  Product(id: '3', name: 'Headphones', price: 199.99),
];
```

#### Implementing the Store

In `lib/main.dart`, set up the Redux store and connect it to the Flutter app:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_redux/flutter_redux.dart';
import 'package:redux/redux.dart';

import 'models/product.dart';
import 'models/cart_item.dart';
import 'reducers/app_reducer.dart';

void main() {
  final store = Store<AppState>(
    appReducer,
    initialState: AppState.initial(),
  );

  runApp(MyApp(store: store));
}

class MyApp extends StatelessWidget {
  final Store<AppState> store;

  MyApp({required this.store});

  @override
  Widget build(BuildContext context) {
    return StoreProvider<AppState>(
      store: store,
      child: MaterialApp(
        title: 'Shopping Cart Redux',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        home: ProductListScreen(),
      ),
    );
  }
}
```

### Conclusion

This comprehensive guide has walked you through setting up a shopping cart application using Redux in Flutter. We've covered the architecture, state management, and key implementation steps. By following these guidelines, you can build a robust and scalable shopping cart app. Remember, the key to successful app development lies in thorough planning, clear architecture, and effective state management.

### Further Exploration

To deepen your understanding of Redux and Flutter, consider exploring the following resources:

- **Books:** "Flutter in Action" by Eric Windmill
- **Online Courses:** "Flutter & Dart - The Complete Guide" on Udemy
- **Official Documentation:** [Redux Documentation](https://redux.js.org/) and [Flutter Documentation](https://flutter.dev/docs)

## Quiz Time!

{{< quizdown >}}

### What is the primary goal of the shopping cart application in this project?

- [x] To demonstrate state management using Redux in Flutter.
- [ ] To showcase advanced UI animations in Flutter.
- [ ] To build a real-time chat application.
- [ ] To integrate machine learning models in Flutter.

> **Explanation:** The project aims to demonstrate state management using Redux in Flutter by building a shopping cart application.

### Which of the following is NOT a feature of the shopping cart application?

- [ ] Viewing a list of products.
- [x] Real-time chat functionality.
- [ ] Adding and removing products from the cart.
- [ ] Viewing the total cost.

> **Explanation:** Real-time chat functionality is not a feature of the shopping cart application. The focus is on managing products and cart items.

### What is the purpose of the `AppState` class in the Redux setup?

- [x] To hold the application's state, including products and cart items.
- [ ] To manage UI animations.
- [ ] To handle network requests.
- [ ] To store user authentication data.

> **Explanation:** The `AppState` class holds the application's state, including the list of products and cart items.

### Which action is used to add a product to the cart?

- [x] AddToCartAction
- [ ] RemoveFromCartAction
- [ ] UpdateQuantityAction
- [ ] FetchProductsAction

> **Explanation:** The `AddToCartAction` is used to add a product to the cart.

### What is the role of reducers in Redux?

- [x] To specify how the application's state changes in response to actions.
- [ ] To manage UI components.
- [ ] To handle user authentication.
- [ ] To perform network requests.

> **Explanation:** Reducers specify how the application's state changes in response to actions sent to the store.

### How do you create a new Flutter project for this application?

- [x] Use the command `flutter create shopping_cart_redux`.
- [ ] Use the command `flutter init shopping_cart_redux`.
- [ ] Use the command `flutter new shopping_cart_redux`.
- [ ] Use the command `flutter start shopping_cart_redux`.

> **Explanation:** The command `flutter create shopping_cart_redux` is used to create a new Flutter project.

### Which folder is NOT part of the recommended file structure for this project?

- [ ] lib/actions/
- [ ] lib/models/
- [ ] lib/reducers/
- [x] lib/services/

> **Explanation:** The recommended file structure includes `lib/actions/`, `lib/models/`, and `lib/reducers/`, but not `lib/services/`.

### What is the purpose of the `flutter_redux` package?

- [x] To connect Redux with Flutter's UI components.
- [ ] To manage network requests.
- [ ] To handle animations in Flutter.
- [ ] To perform database operations.

> **Explanation:** The `flutter_redux` package connects Redux with Flutter's UI components, allowing for state management in the app.

### What is the initial state of the `AppState` class?

- [x] It contains an initial list of products and an empty cart.
- [ ] It contains an empty list of products and cart items.
- [ ] It contains user authentication data.
- [ ] It contains network request configurations.

> **Explanation:** The initial state of the `AppState` class contains an initial list of products and an empty cart.

### True or False: Planning the architecture before coding is unnecessary for small projects.

- [ ] True
- [x] False

> **Explanation:** Planning the architecture is crucial for any project, regardless of size, as it ensures a clear and maintainable codebase.

{{< /quizdown >}}
