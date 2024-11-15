---
linkTitle: "2.4.4 Mini Project: Guessing Game"
title: "Guessing Game Project: Learn Coding with Flutter"
description: "Learn to build a simple guessing game using Flutter and Dart, applying if...else statements and comparison operators."
categories:
- Coding for Kids
- Flutter Projects
- Educational Games
tags:
- Flutter
- Dart
- Coding for Beginners
- Kids Coding
- Interactive Learning
date: 2024-10-25
type: docs
nav_weight: 244000
---

## 2.4.4 Mini Project: Guessing Game

Welcome to the exciting world of coding with Flutter! In this mini-project, we will create a fun and interactive guessing game. This project will help you understand how to use if...else statements and comparison operators in Dart, the programming language used with Flutter. Let's dive in and start building!

### Objective

The goal of this project is to create a simple game where the app thinks of a number, and the player has to guess it. The app will provide hints to the player, indicating whether their guess is too high or too low.

### Project Overview

In this guessing game, the app will randomly select a secret number between 1 and 10. The player will enter their guess, and the app will respond with hints until the correct number is guessed. This project will help you practice using variables, user input, and conditional logic.

### Step-by-Step Guide

Let's break down the process of building this game into simple steps:

#### 1. Set Up Variables

First, we need to create a variable to store the secret number and another variable to hold the player's guess. We'll use Dart's `Random` class to generate a random number.

#### 2. Get User Input

We'll use a text field to allow the player to enter their guess. This input will be captured and used to compare against the secret number.

#### 3. Compare Guess to Secret Number

Using if...else statements, we'll compare the player's guess to the secret number and provide feedback.

#### 4. Display Results

Based on the comparison, we'll display messages like "Too high!", "Too low!", or "You got it!" to guide the player.

### Code Example

Here's a complete example of the guessing game code in Dart using Flutter:

```dart
import 'package:flutter/material.dart';
import 'dart:math';

void main() {
  runApp(GuessingGameApp());
}

class GuessingGameApp extends StatefulWidget {
  @override
  _GuessingGameAppState createState() => _GuessingGameAppState();
}

class _GuessingGameAppState extends State<GuessingGameApp> {
  final TextEditingController _controller = TextEditingController();
  final int secretNumber = Random().nextInt(10) + 1;
  String message = '';

  void checkGuess() {
    int guess = int.parse(_controller.text);
    setState(() {
      if (guess > secretNumber) {
        message = 'Too high!';
      } else if (guess < secretNumber) {
        message = 'Too low!';
      } else {
        message = 'You got it!';
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Guessing Game'),
        ),
        body: Padding(
          padding: EdgeInsets.all(16.0),
          child: Column(
            children: [
              Text(
                'I am thinking of a number between 1 and 10.',
                style: TextStyle(fontSize: 18),
              ),
              TextField(
                controller: _controller,
                decoration: InputDecoration(labelText: 'Enter your guess'),
                keyboardType: TextInputType.number,
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: checkGuess,
                child: Text('Guess'),
              ),
              SizedBox(height: 20),
              Text(
                message,
                style: TextStyle(fontSize: 24, color: Colors.blue),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

### Visuals

To help visualize the flow of the game, let's use a Mermaid.js diagram to outline the process from input to feedback:

```mermaid
graph TD;
    A[Start] --> B[Generate Secret Number]
    B --> C[Player Enters Guess]
    C --> D{Compare Guess}
    D -->|Too High| E[Display "Too High!"]
    D -->|Too Low| F[Display "Too Low!"]
    D -->|Correct| G[Display "You Got It!"]
    E --> C
    F --> C
    G --> H[End]
```

### Language and Engagement

This project is designed to be fun and educational. Encourage kids to experiment with different numbers and share their secret numbers with friends to play together. It's a great way to learn coding while having fun!

### Best Practices and Tips

- **Error Handling:** Consider adding error handling to manage non-numeric inputs gracefully.
- **Replay Option:** Add a feature to restart the game after a correct guess.
- **Customization:** Encourage kids to modify the range of numbers or add more features to the game.

### Conclusion

Congratulations! You've built a simple yet engaging guessing game using Flutter and Dart. This project has helped you practice using variables, user input, and conditional logic. Keep experimenting and have fun coding!

## Quiz Time!

{{< quizdown >}}

### What is the purpose of the guessing game project?

- [x] To practice using if...else statements and comparison operators
- [ ] To learn about loops
- [ ] To create a complex game
- [ ] To practice using lists

> **Explanation:** The guessing game project is designed to help you practice using if...else statements and comparison operators.

### What does the `Random().nextInt(10) + 1` code do?

- [x] Generates a random number between 1 and 10
- [ ] Generates a random number between 0 and 9
- [ ] Generates a random number between 1 and 100
- [ ] Generates a random number between 0 and 10

> **Explanation:** The `Random().nextInt(10) + 1` code generates a random number between 1 and 10.

### What is the role of the `TextEditingController` in the code?

- [x] To capture and manage user input from the text field
- [ ] To display messages to the user
- [ ] To generate random numbers
- [ ] To manage the app's state

> **Explanation:** The `TextEditingController` is used to capture and manage user input from the text field.

### How does the app provide feedback to the player?

- [x] By comparing the player's guess to the secret number and displaying messages
- [ ] By generating a new random number
- [ ] By changing the app's background color
- [ ] By playing a sound

> **Explanation:** The app provides feedback by comparing the player's guess to the secret number and displaying messages like "Too high!" or "Too low!".

### What happens when the player's guess is correct?

- [x] The app displays "You got it!"
- [ ] The app generates a new secret number
- [ ] The app closes
- [ ] The app plays a sound

> **Explanation:** When the player's guess is correct, the app displays the message "You got it!".

### What is the purpose of the `setState()` method in the code?

- [x] To update the UI with the new message
- [ ] To generate a new random number
- [ ] To reset the game
- [ ] To capture user input

> **Explanation:** The `setState()` method is used to update the UI with the new message based on the player's guess.

### How can you modify the game to allow for a replay option?

- [x] Add a button to reset the secret number and clear the input
- [ ] Change the range of random numbers
- [ ] Add more text fields
- [ ] Remove the `setState()` method

> **Explanation:** To allow for a replay option, you can add a button that resets the secret number and clears the input field.

### What is a potential improvement to handle non-numeric inputs?

- [x] Add error handling to manage non-numeric inputs gracefully
- [ ] Increase the range of numbers
- [ ] Remove the text field
- [ ] Change the app's theme

> **Explanation:** Adding error handling to manage non-numeric inputs gracefully is a potential improvement to the game.

### What is the main learning outcome of this project?

- [x] Understanding conditional logic and user input in Flutter
- [ ] Learning about loops and iterations
- [ ] Creating complex animations
- [ ] Building a multi-page app

> **Explanation:** The main learning outcome of this project is understanding conditional logic and user input in Flutter.

### True or False: The guessing game uses loops to provide feedback to the player.

- [ ] True
- [x] False

> **Explanation:** False. The guessing game uses if...else statements, not loops, to provide feedback to the player.

{{< /quizdown >}}
