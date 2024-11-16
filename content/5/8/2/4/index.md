---
linkTitle: "8.2.4 Mini Project: News Reader"
title: "Build a News Reader App with Flutter: Fetching Data Online"
description: "Learn how to create a simple news reader app using Flutter and a public news API. This step-by-step guide will teach you how to fetch and display news articles in a user-friendly interface."
categories:
- Flutter Development
- Mobile Apps
- Kids Coding
tags:
- Flutter
- News Reader
- API Integration
- Mobile Development
- Kids Coding
date: 2024-10-25
type: docs
nav_weight: 824000
canonical: "https://fluttermasterylibrary.com/5/8/2/4"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 8.2.4 Mini Project: News Reader

In this exciting mini-project, we'll guide you through building a simple news reader app using Flutter. This app will fetch the latest news articles from an online API and display them in a user-friendly list. By the end of this project, you'll have a functional app that keeps you updated with the latest headlines!

### Project Overview

The News Reader app will connect to a public news API to retrieve the latest news articles. We'll display these articles in a scrollable list, complete with titles and brief descriptions. This project will introduce you to the world of APIs and how to use them to fetch data for your apps.

### Step-by-Step Instructions

#### 1. Setting Up the Project

Let's start by setting up our Flutter project:

- **Create a New Flutter Project:** Open your terminal or command prompt and run the following command:

  ```bash
  flutter create news_reader
  ```

- **Add the `http` Package:** Open the `pubspec.yaml` file in your project and add the `http` package under dependencies:

  ```yaml
  dependencies:
    flutter:
      sdk: flutter
    http: ^0.13.3
  ```

  Run `flutter pub get` to install the package.

- **Set Up Internet Permissions:** Ensure your app has permission to access the internet. For Android, check the `AndroidManifest.xml` file in `android/app/src/main/` and ensure it includes:

  ```xml
  <uses-permission android:name="android.permission.INTERNET"/>
  ```

#### 2. Choosing a News API

We'll use a kid-friendly news API like [NewsAPI.org](https://newsapi.org/). Follow these steps:

- **Sign Up for an API Key:** Visit NewsAPI.org and sign up for a free developer account to obtain an API key.

- **Understand the API:** Familiarize yourself with the API documentation to understand how to make requests and what data you can retrieve.

#### 3. Creating Data Models

Define Dart classes to represent the news articles. This helps in organizing the data we fetch:

```dart
class Article {
  final String title;
  final String? description;

  Article({required this.title, this.description});

  factory Article.fromJson(Map<String, dynamic> json) {
    return Article(
      title: json['title'],
      description: json['description'],
    );
  }
}
```

#### 4. Fetching Data from the API

Write functions to send GET requests to the API and retrieve news data:

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<List<Article>> fetchNews() async {
  final response = await http.get(Uri.parse('https://newsapi.org/v2/top-headlines?country=us&apiKey=YOUR_API_KEY'));

  if (response.statusCode == 200) {
    final data = jsonDecode(response.body);
    return (data['articles'] as List).map((json) => Article.fromJson(json)).toList();
  } else {
    throw Exception('Failed to load news');
  }
}
```

#### 5. Parsing and Storing Data

Parse the JSON response and convert it into Dart objects, storing them in a list:

```dart
List<Article> articles = await fetchNews();
```

#### 6. Building the User Interface

Use `ListView.builder` to create a scrollable list of news articles:

```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text('News Reader'),
    ),
    body: ListView.builder(
      itemCount: articles.length,
      itemBuilder: (context, index) {
        return Card(
          child: ListTile(
            title: Text(articles[index].title),
            subtitle: Text(articles[index].description ?? ''),
          ),
        );
      },
    ),
  );
}
```

#### 7. Styling the App

Apply basic styling to make the app visually appealing:

- Use `AppBar` for the app's title.
- Use `Card` and `Padding` for layout.

#### 8. Testing the App

Run the app on an emulator or device to ensure it correctly fetches and displays news articles. Debug any issues that arise during testing.

#### 9. Enhancing the App

Consider adding features like:

- **Pull-to-Refresh:** Allow users to refresh the news list by pulling down.
- **Search Functionality:** Implement a search bar to look for specific news topics.
- **Categories:** Organize news into sections like Sports, Technology, etc.

### Complete Code Example

Here's the complete code for your News Reader app:

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() => runApp(NewsReaderApp());

class NewsReaderApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'News Reader',
      home: NewsHomePage(),
    );
  }
}

class NewsHomePage extends StatefulWidget {
  @override
  _NewsHomePageState createState() => _NewsHomePageState();
}

class _NewsHomePageState extends State<NewsHomePage> {
  List<Article> articles = [];

  @override
  void initState() {
    super.initState();
    fetchNews();
  }

  Future<void> fetchNews() async {
    final response = await http.get(Uri.parse('https://newsapi.org/v2/top-headlines?country=us&apiKey=YOUR_API_KEY'));
    if (response.statusCode == 200) {
      final data = jsonDecode(response.body);
      setState(() {
        articles = (data['articles'] as List).map((json) => Article.fromJson(json)).toList();
      });
    } else {
      throw Exception('Failed to load news');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('News Reader'),
      ),
      body: ListView.builder(
        itemCount: articles.length,
        itemBuilder: (context, index) {
          return Card(
            child: ListTile(
              title: Text(articles[index].title),
              subtitle: Text(articles[index].description ?? ''),
            ),
          );
        },
      ),
    );
  }
}

class Article {
  final String title;
  final String? description;

  Article({required this.title, this.description});

  factory Article.fromJson(Map<String, dynamic> json) {
    return Article(
      title: json['title'],
      description: json['description'],
    );
  }
}
```

### Interactive Exercise

Encourage readers to customize their news reader app by:

- **Adding Images:** Display images for each news article.
- **Implementing a Search Bar:** Allow users to search for specific news topics.
- **Categorizing News:** Organize articles into sections like Sports, Technology, etc.

### Visual Aids

Include screenshots of the app at different stages to illustrate progress. Show the initial setup, after fetching data, and with added features.

## Quiz Time!

{{< quizdown >}}

### What is the main purpose of the News Reader app?

- [x] To fetch and display the latest news articles from an online API.
- [ ] To create a chat application.
- [ ] To play music from an online source.
- [ ] To edit photos.

> **Explanation:** The News Reader app is designed to fetch and display news articles from a public news API.

### Which package is essential for making HTTP requests in Flutter?

- [x] http
- [ ] flutter_http
- [ ] requests
- [ ] network

> **Explanation:** The `http` package is used in Flutter to make HTTP requests to fetch data from the internet.

### What is an API key used for?

- [x] To authenticate requests to an API.
- [ ] To style the app's user interface.
- [ ] To store data locally.
- [ ] To debug the app.

> **Explanation:** An API key is used to authenticate requests to an API, ensuring that the requests are valid and authorized.

### What Flutter widget is used to create a scrollable list of items?

- [x] ListView.builder
- [ ] Column
- [ ] Row
- [ ] Container

> **Explanation:** `ListView.builder` is a Flutter widget used to create a scrollable list of items efficiently.

### What should you do if the API request fails?

- [x] Handle the error gracefully and inform the user.
- [ ] Ignore the error and continue.
- [ ] Restart the app.
- [ ] Remove the API call.

> **Explanation:** It's important to handle errors gracefully and inform the user if an API request fails.

### How can you enhance the News Reader app?

- [x] Add pull-to-refresh functionality.
- [x] Implement a search bar.
- [x] Categorize news into sections.
- [ ] Remove the API integration.

> **Explanation:** Enhancements like pull-to-refresh, search functionality, and categorizing news can improve the app's usability.

### What is the purpose of the `Article` class in the app?

- [x] To represent news articles as Dart objects.
- [ ] To handle user authentication.
- [ ] To manage app settings.
- [ ] To store images.

> **Explanation:** The `Article` class is used to represent news articles as Dart objects, making it easier to manage and display the data.

### What is JSON used for in this project?

- [x] To format the data received from the API.
- [ ] To style the app's UI.
- [ ] To store user preferences.
- [ ] To manage app navigation.

> **Explanation:** JSON (JavaScript Object Notation) is used to format the data received from the API, which is then parsed into Dart objects.

### Which widget is used to display each news article in the list?

- [x] ListTile
- [ ] Text
- [ ] Image
- [ ] Button

> **Explanation:** `ListTile` is used to display each news article in the list, showing the title and description.

### True or False: The News Reader app can be enhanced with additional features like search and categorization.

- [x] True
- [ ] False

> **Explanation:** True. The News Reader app can be enhanced with features like search and categorization to improve its functionality and user experience.

{{< /quizdown >}}
