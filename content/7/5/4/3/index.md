---
linkTitle: "5.4.3 Updating UI Based on State"
title: "Updating UI Based on State with Bloc Pattern in Flutter"
description: "Learn how to effectively update your Flutter app's UI based on state changes using the Bloc pattern, including practical examples and best practices."
categories:
- Flutter
- State Management
- Bloc Pattern
tags:
- Flutter
- Bloc
- State Management
- UI Update
- Responsive Design
date: 2024-10-25
type: docs
nav_weight: 543000
---

## 5.4.3 Updating UI Based on State

In the world of Flutter development, managing state efficiently is crucial for building responsive and interactive applications. The Bloc pattern is a powerful tool that helps developers manage state in a predictable and scalable way. In this section, we will explore how to update the UI based on state changes using the Bloc pattern, focusing on practical implementations and best practices.

### Using BlocBuilder

The `BlocBuilder` widget is a core component of the Bloc library that allows you to rebuild parts of your UI in response to state changes. It listens to a `Bloc` and rebuilds its widget tree whenever the state changes. This is particularly useful for creating dynamic UIs that respond to user interactions or asynchronous data updates.

#### How BlocBuilder Works

`BlocBuilder` takes two generic parameters: the type of the `Bloc` and the type of the `State` it manages. It also requires a `builder` function, which is called every time the state changes. The `builder` function provides the current `BuildContext` and the current state, allowing you to return different widgets based on the state.

Here's a simple example of using `BlocBuilder` to update the UI based on different states of a weather application:

```dart
BlocBuilder<WeatherBloc, WeatherState>(
  builder: (context, state) {
    if (state is WeatherInitial) {
      return Text('Please Select a City');
    } else if (state is WeatherLoading) {
      return CircularProgressIndicator();
    } else if (state is WeatherLoaded) {
      return WeatherDisplay(weather: state.weather);
    } else if (state is WeatherError) {
      return Text(state.message);
    }
    return Container();
  },
);
```

In this example, the UI displays different widgets based on the current state of the `WeatherBloc`. This approach ensures that the UI is always in sync with the application's state.

### Building Responsive UI Components

Creating responsive UI components is essential for providing a seamless user experience across different devices and screen sizes. In our weather application example, we need to ensure that components like `WeatherDisplay` adapt to various screen dimensions.

#### Designing the WeatherDisplay Widget

The `WeatherDisplay` widget is responsible for showing the weather data when the `WeatherLoaded` state is active. Here's an example implementation:

```dart
class WeatherDisplay extends StatelessWidget {
  final Weather weather;

  WeatherDisplay({required this.weather});

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        if (constraints.maxWidth < 600) {
          return Column(
            children: [
              Text(
                weather.cityName,
                style: TextStyle(fontSize: 24),
              ),
              Text(
                '${weather.temperature}°C',
                style: TextStyle(fontSize: 48),
              ),
            ],
          );
        } else {
          return Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                weather.cityName,
                style: TextStyle(fontSize: 24),
              ),
              SizedBox(width: 20),
              Text(
                '${weather.temperature}°C',
                style: TextStyle(fontSize: 48),
              ),
            ],
          );
        }
      },
    );
  }
}
```

In this widget, we use `LayoutBuilder` to determine the available space and adjust the layout accordingly. This ensures that the UI remains responsive, providing a consistent experience on both small and large screens.

### User Interactions

User interactions are a critical aspect of any application, and managing these interactions effectively is key to a smooth user experience. In our weather application, we need to allow users to search for a city and update the weather information accordingly.

#### Implementing City Search Functionality

To enable city search functionality, we can use a `TextField` widget to capture user input and dispatch events to the `WeatherBloc` to fetch weather data for the entered city.

```dart
class CitySearch extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final weatherBloc = context.read<WeatherBloc>();

    return Padding(
      padding: const EdgeInsets.all(8.0),
      child: Column(
        children: [
          TextField(
            decoration: InputDecoration(
              labelText: 'Enter City',
              border: OutlineInputBorder(),
            ),
            onSubmitted: (cityName) {
              weatherBloc.add(FetchWeather(cityName: cityName));
            },
          ),
        ],
      ),
    );
  }
}
```

In this example, we use `context.read<WeatherBloc>()` to access the `WeatherBloc` instance and dispatch a `FetchWeather` event when the user submits the city name. This triggers the Bloc to fetch new weather data and update the state, which in turn updates the UI through the `BlocBuilder`.

### Practical Code Example: Weather Application

Let's put everything together in a complete example of a weather application that uses the Bloc pattern to manage state and update the UI.

#### Setting Up the Bloc

First, we define the `WeatherBloc`, `WeatherState`, and `WeatherEvent` classes:

```dart
// weather_bloc.dart
import 'package:flutter_bloc/flutter_bloc.dart';

abstract class WeatherEvent {}

class FetchWeather extends WeatherEvent {
  final String cityName;

  FetchWeather({required this.cityName});
}

abstract class WeatherState {}

class WeatherInitial extends WeatherState {}

class WeatherLoading extends WeatherState {}

class WeatherLoaded extends WeatherState {
  final Weather weather;

  WeatherLoaded({required this.weather});
}

class WeatherError extends WeatherState {
  final String message;

  WeatherError({required this.message});
}

class WeatherBloc extends Bloc<WeatherEvent, WeatherState> {
  WeatherBloc() : super(WeatherInitial());

  @override
  Stream<WeatherState> mapEventToState(WeatherEvent event) async* {
    if (event is FetchWeather) {
      yield WeatherLoading();
      try {
        final weather = await fetchWeather(event.cityName);
        yield WeatherLoaded(weather: weather);
      } catch (e) {
        yield WeatherError(message: e.toString());
      }
    }
  }
}
```

In this setup, `WeatherBloc` listens for `FetchWeather` events, fetches weather data, and updates the state accordingly.

#### Integrating Bloc with the UI

Next, we integrate the Bloc with the UI using `BlocProvider` and `BlocBuilder`:

```dart
// main.dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: BlocProvider(
        create: (context) => WeatherBloc(),
        child: WeatherPage(),
      ),
    );
  }
}

class WeatherPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Weather App'),
      ),
      body: Column(
        children: [
          CitySearch(),
          BlocBuilder<WeatherBloc, WeatherState>(
            builder: (context, state) {
              if (state is WeatherInitial) {
                return Text('Please Select a City');
              } else if (state is WeatherLoading) {
                return CircularProgressIndicator();
              } else if (state is WeatherLoaded) {
                return WeatherDisplay(weather: state.weather);
              } else if (state is WeatherError) {
                return Text(state.message);
              }
              return Container();
            },
          ),
        ],
      ),
    );
  }
}
```

In this example, `BlocProvider` is used to provide the `WeatherBloc` to the widget tree, and `BlocBuilder` is used to rebuild the UI based on the current state.

### Best Practices and Common Pitfalls

When using the Bloc pattern to update the UI based on state, consider the following best practices and common pitfalls:

- **Keep UI Logic Separate from Business Logic:** Ensure that your UI components are only responsible for rendering the UI and not for managing business logic. Use the Bloc to handle state transitions and business logic.
- **Optimize Rebuilds:** Use `BlocBuilder` judiciously to avoid unnecessary rebuilds. Consider using `BlocListener` for one-time actions or side effects that don't require a UI rebuild.
- **Handle Errors Gracefully:** Always provide a way to handle errors and display meaningful messages to the user.
- **Test Your Blocs:** Write unit tests for your Blocs to ensure that state transitions occur as expected.

### Conclusion

Updating the UI based on state changes using the Bloc pattern is a powerful technique that allows you to build responsive and maintainable Flutter applications. By leveraging `BlocBuilder`, creating responsive UI components, and managing user interactions effectively, you can create dynamic applications that provide a seamless user experience. Remember to follow best practices and avoid common pitfalls to ensure your application remains scalable and maintainable.

## Quiz Time!

{{< quizdown >}}

### What is the primary role of `BlocBuilder` in the Bloc pattern?

- [x] To rebuild the UI in response to state changes.
- [ ] To dispatch events to the Bloc.
- [ ] To manage the business logic of the application.
- [ ] To handle asynchronous operations.

> **Explanation:** `BlocBuilder` is used to rebuild the UI based on state changes, making it a crucial component for creating dynamic and responsive interfaces.

### Which widget is used to capture user input for city search in the weather application example?

- [ ] BlocBuilder
- [ ] BlocProvider
- [x] TextField
- [ ] WeatherDisplay

> **Explanation:** The `TextField` widget is used to capture user input, allowing users to enter a city name for weather search.

### How does `BlocProvider` contribute to the Bloc pattern?

- [x] It provides the Bloc to the widget tree, making it accessible to child widgets.
- [ ] It rebuilds the UI based on state changes.
- [ ] It handles asynchronous data fetching.
- [ ] It manages state transitions.

> **Explanation:** `BlocProvider` is responsible for providing the Bloc instance to the widget tree, ensuring that child widgets can access it.

### What is a common pitfall when using `BlocBuilder`?

- [ ] Not handling user interactions.
- [ ] Overusing `BlocProvider`.
- [x] Causing unnecessary UI rebuilds.
- [ ] Ignoring asynchronous operations.

> **Explanation:** Overusing `BlocBuilder` can lead to unnecessary UI rebuilds, which can impact performance. It's important to use it judiciously.

### What is the purpose of the `WeatherDisplay` widget in the example?

- [ ] To capture user input for city search.
- [x] To display weather data when the `WeatherLoaded` state is active.
- [ ] To handle state transitions in the Bloc.
- [ ] To manage asynchronous operations.

> **Explanation:** The `WeatherDisplay` widget is responsible for displaying weather data when the `WeatherLoaded` state is active, providing a visual representation of the weather information.

### How can you access the `WeatherBloc` instance within a widget?

- [ ] Using `BlocBuilder`.
- [x] Using `context.read<WeatherBloc>()`.
- [ ] Using `BlocProvider`.
- [ ] Using `WeatherDisplay`.

> **Explanation:** You can access the `WeatherBloc` instance within a widget using `context.read<WeatherBloc>()`, which allows you to interact with the Bloc.

### What is the role of the `FetchWeather` event in the Bloc pattern?

- [x] To trigger the Bloc to fetch weather data for a specified city.
- [ ] To rebuild the UI based on state changes.
- [ ] To provide the Bloc to the widget tree.
- [ ] To handle user interactions.

> **Explanation:** The `FetchWeather` event is dispatched to the Bloc to trigger the fetching of weather data for a specified city, initiating a state transition.

### Which state represents the initial state of the `WeatherBloc`?

- [ ] WeatherLoading
- [ ] WeatherLoaded
- [x] WeatherInitial
- [ ] WeatherError

> **Explanation:** `WeatherInitial` represents the initial state of the `WeatherBloc`, indicating that no weather data has been fetched yet.

### What should you do to handle errors gracefully in the Bloc pattern?

- [ ] Ignore errors and focus on UI updates.
- [ ] Use `BlocProvider` to manage errors.
- [x] Provide meaningful error messages to the user.
- [ ] Use `BlocBuilder` to handle errors.

> **Explanation:** Handling errors gracefully involves providing meaningful error messages to the user, ensuring a better user experience.

### True or False: `BlocBuilder` can be used to dispatch events to the Bloc.

- [ ] True
- [x] False

> **Explanation:** `BlocBuilder` is not used to dispatch events to the Bloc; it is used to rebuild the UI based on state changes. Events are dispatched using methods like `context.read<Bloc>().add(Event)`.

{{< /quizdown >}}
