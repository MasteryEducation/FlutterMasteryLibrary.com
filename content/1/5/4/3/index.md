---
linkTitle: "5.4.3 Route Transition Animations"
title: "Route Transition Animations in Flutter: Customizing Navigation with PageRouteBuilder"
description: "Explore how to create custom route transition animations in Flutter using PageRouteBuilder, and learn best practices for smooth and consistent app navigation."
categories:
- Flutter Development
- Mobile App Development
- User Interface Design
tags:
- Flutter
- Route Transitions
- PageRouteBuilder
- Custom Animations
- Mobile UI
date: 2024-10-25
type: docs
nav_weight: 543000
canonical: "https://fluttermasterylibrary.com/1/5/4/3"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 5.4.3 Route Transition Animations

In the world of mobile app development, user experience is paramount. One of the key aspects that contribute to a seamless user experience is the transition animations between different screens or routes. Flutter, with its rich set of widgets and tools, provides developers with the ability to customize these animations to enhance the overall feel of the app. In this section, we will delve into the intricacies of route transition animations in Flutter, focusing on how to implement and customize them using `PageRouteBuilder`.

### Custom Route Transitions

Flutter's navigation system is built around the concept of routes. By default, Flutter provides basic transition animations such as slide transitions when navigating between routes. However, to create a unique and engaging user experience, you might want to customize these transitions. Custom route transitions allow you to define how a new screen appears and how the current screen disappears, providing a more cohesive and branded experience.

### Using PageRouteBuilder

The `PageRouteBuilder` class in Flutter is a powerful tool that allows developers to define custom transitions. It provides two main parameters: `pageBuilder` and `transitionsBuilder`.

#### Implementation

Let's start with a basic example of using `PageRouteBuilder` to create a fade transition when navigating to a new page.

```dart
Navigator.push(
  context,
  PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => NewPage(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return FadeTransition(
        opacity: animation,
        child: child,
      );
    },
  ),
);
```

#### Breaking Down the Parameters

- **`pageBuilder`:** This parameter is responsible for building the new page that you want to navigate to. It takes three arguments: `context`, `animation`, and `secondaryAnimation`. The `animation` and `secondaryAnimation` are used to define the transition animations.

- **`transitionsBuilder`:** This parameter defines how the transition animation should be performed. It also takes four arguments: `context`, `animation`, `secondaryAnimation`, and `child`. The `child` is the widget returned by the `pageBuilder`. In the example above, we use a `FadeTransition` to gradually change the opacity of the new page from 0 to 1.

### Custom Animations

While the `FadeTransition` is a simple and elegant way to transition between routes, Flutter offers a variety of other transition widgets that you can use to create more dynamic effects.

#### SlideTransition

A `SlideTransition` can be used to slide a new page in from a specific direction. Here's how you can implement a slide transition from the bottom:

```dart
Navigator.push(
  context,
  PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => NewPage(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      const begin = Offset(0.0, 1.0);
      const end = Offset.zero;
      const curve = Curves.ease;

      var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));

      var offsetAnimation = animation.drive(tween);

      return SlideTransition(
        position: offsetAnimation,
        child: child,
      );
    },
  ),
);
```

#### ScaleTransition

A `ScaleTransition` can be used to scale the new page in, creating a zoom effect:

```dart
Navigator.push(
  context,
  PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => NewPage(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      var tween = Tween(begin: 0.0, end: 1.0).chain(CurveTween(curve: Curves.ease));

      return ScaleTransition(
        scale: animation.drive(tween),
        child: child,
      );
    },
  ),
);
```

#### Combining Animations

For more complex effects, you can combine multiple animations. For instance, you can create a transition that both slides and fades a new page in:

```dart
Navigator.push(
  context,
  PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => NewPage(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      const begin = Offset(0.0, 1.0);
      const end = Offset.zero;
      const curve = Curves.ease;

      var tween = Tween(begin: begin, end: end).chain(CurveTween(curve: curve));
      var offsetAnimation = animation.drive(tween);

      return SlideTransition(
        position: offsetAnimation,
        child: FadeTransition(
          opacity: animation,
          child: child,
        ),
      );
    },
  ),
);
```

### Shared Axis Transition (Advanced)

For developers looking to implement more advanced transitions, the `animations` package provides a set of pre-defined Material motion transitions. One such transition is the Shared Axis Transition, which animates changes in the position of elements along a shared axis.

To use the Shared Axis Transition, you need to add the `animations` package to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  animations: ^2.0.0
```

Here's an example of implementing a Shared Axis Transition:

```dart
import 'package:animations/animations.dart';

Navigator.push(
  context,
  PageRouteBuilder(
    pageBuilder: (context, animation, secondaryAnimation) => NewPage(),
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      return SharedAxisTransition(
        animation: animation,
        secondaryAnimation: secondaryAnimation,
        transitionType: SharedAxisTransitionType.horizontal,
        child: child,
      );
    },
  ),
);
```

### Best Practices

When implementing custom route transitions, it's essential to keep a few best practices in mind:

- **Smooth Transitions:** Ensure that transitions are smooth and do not cause jarring effects. This can be achieved by using appropriate curves and durations.

- **Consistency:** Maintain consistency in transition styles throughout your app to provide a cohesive user experience.

- **Performance:** Avoid overly complex animations that could impact performance, especially on lower-end devices.

- **User Control:** Allow users to control transitions, such as providing a way to skip or speed up animations if needed.

### Practice Exercises

To reinforce your understanding of route transition animations, try implementing the following exercises:

#### Exercise 1: Slide-Up Transition for Modals

Implement a slide-up transition for a modal dialog using `PageRouteBuilder`.

#### Exercise 2: Custom Transition with Scale and Fade

Create a custom transition that scales and fades in the new route simultaneously.

### Conclusion

Customizing route transition animations in Flutter can significantly enhance the user experience of your app. By leveraging `PageRouteBuilder` and the `animations` package, you can create smooth, consistent, and visually appealing transitions that align with your app's design and branding. Remember to keep transitions subtle and performance-friendly to ensure a seamless experience for your users.

## Quiz Time!

{{< quizdown >}}

### What is the primary purpose of customizing route transition animations in Flutter?

- [x] To enhance user experience by providing smooth and engaging transitions.
- [ ] To make the app load faster.
- [ ] To increase the complexity of the app.
- [ ] To reduce the app size.

> **Explanation:** Customizing route transition animations enhances the user experience by providing smooth and engaging transitions between screens.

### Which class in Flutter is used to define custom route transitions?

- [x] PageRouteBuilder
- [ ] MaterialPageRoute
- [ ] CustomRoute
- [ ] TransitionRoute

> **Explanation:** `PageRouteBuilder` is used in Flutter to define custom route transitions.

### In the PageRouteBuilder example, what does the `transitionsBuilder` parameter do?

- [x] It defines the transition animation.
- [ ] It builds the destination page.
- [ ] It sets the duration of the transition.
- [ ] It handles user input during the transition.

> **Explanation:** The `transitionsBuilder` parameter defines the transition animation in `PageRouteBuilder`.

### Which transition widget can be used to slide a new page in from a specific direction?

- [x] SlideTransition
- [ ] FadeTransition
- [ ] ScaleTransition
- [ ] RotationTransition

> **Explanation:** `SlideTransition` is used to slide a new page in from a specific direction.

### How can you combine multiple animations in a transition?

- [x] By nesting transition widgets within the `transitionsBuilder`.
- [ ] By using multiple `PageRouteBuilder` instances.
- [ ] By setting the `animation` parameter to a list.
- [ ] By using the `combineAnimations` method.

> **Explanation:** You can combine multiple animations by nesting transition widgets within the `transitionsBuilder`.

### What package provides pre-defined Material motion transitions like the Shared Axis Transition?

- [x] animations
- [ ] material
- [ ] motion
- [ ] transitions

> **Explanation:** The `animations` package provides pre-defined Material motion transitions like the Shared Axis Transition.

### What is a best practice when implementing custom route transitions?

- [x] Ensure transitions are smooth and not overly distracting.
- [ ] Use as many animations as possible for each transition.
- [ ] Make transitions as fast as possible.
- [ ] Avoid using transitions to improve performance.

> **Explanation:** A best practice is to ensure transitions are smooth and not overly distracting.

### What should you consider to maintain consistency in transition styles throughout your app?

- [x] Use similar transition styles for similar navigation actions.
- [ ] Use different transition styles for each screen.
- [ ] Avoid using transitions for consistency.
- [ ] Only use transitions for the main navigation actions.

> **Explanation:** To maintain consistency, use similar transition styles for similar navigation actions.

### What is the effect of using a `ScaleTransition` in a route transition?

- [x] It creates a zoom effect by scaling the new page.
- [ ] It slides the new page from the bottom.
- [ ] It fades the new page in.
- [ ] It rotates the new page.

> **Explanation:** A `ScaleTransition` creates a zoom effect by scaling the new page.

### True or False: The `animations` package is included by default in Flutter projects.

- [ ] True
- [x] False

> **Explanation:** The `animations` package is not included by default and must be added to the `pubspec.yaml` file.

{{< /quizdown >}}
