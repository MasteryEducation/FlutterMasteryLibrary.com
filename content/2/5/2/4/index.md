---
linkTitle: "5.2.4 Displaying Fetched Data"
title: "Displaying Fetched Data in Flutter: FutureBuilder and ListView"
description: "Learn how to display data fetched from the internet in your Flutter app using FutureBuilder and ListView, handling asynchronous updates, loading states, and errors effectively."
categories:
- Flutter Development
- Mobile App Development
- UI/UX Design
tags:
- Flutter
- FutureBuilder
- ListView
- Asynchronous Programming
- Mobile UI
date: 2024-10-25
type: docs
nav_weight: 524000
canonical: "https://fluttermasterylibrary.com/2/5/2/4"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 5.2.4 Displaying Fetched Data

Building a mobile application often involves fetching data from the internet and displaying it to users in a responsive and user-friendly manner. In Flutter, this can be efficiently achieved using the `FutureBuilder` widget, which allows you to build widgets based on the state of a `Future`. This section will guide you through the process of displaying fetched data in your Flutter app, focusing on using `FutureBuilder` and `ListView` to handle asynchronous data, loading states, and errors.

### Understanding FutureBuilder

The `FutureBuilder` widget is a powerful tool in Flutter for handling asynchronous operations. It allows you to build a widget tree based on the state of a `Future`, which represents a potential value or error that will be available at some point in the future. The `FutureBuilder` widget listens to the `Future` and rebuilds its widget tree whenever the `Future` completes, either with data or an error.

#### Key Components of FutureBuilder

- **Future**: The `Future` that the `FutureBuilder` is listening to. This is typically an asynchronous operation, such as a network request.
- **Builder Method**: A function that is called whenever the `Future` completes. It receives a `BuildContext` and an `AsyncSnapshot`, which contains the state of the `Future`.
- **ConnectionState**: An enumeration that represents the current state of the connection to the asynchronous computation. It can be one of the following:
  - `none`: No connection has been made yet.
  - `waiting`: The `Future` is still running.
  - `active`: The `Future` is active and may produce data at any time.
  - `done`: The `Future` has completed, either with data or an error.

#### Using FutureBuilder in Practice

Let's explore how to use `FutureBuilder` to display a list of posts fetched from the internet. We'll assume you have a `Future<List<Post>>` that fetches posts from an API.

```dart
class PostsPage extends StatelessWidget {
  final Future<List<Post>> posts;

  PostsPage({Key? key, required this.posts}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Posts')),
      body: FutureBuilder<List<Post>>(
        future: posts,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('An error occurred: ${snapshot.error}'));
          } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return Center(child: Text('No posts found.'));
          } else {
            return ListView(
              children: snapshot.data!.map((post) => ListTile(
                title: Text(post.title),
                subtitle: Text(post.body),
              )).toList(),
            );
          }
        },
      ),
    );
  }
}
```

### Displaying Data in a ListView

The `ListView` widget in Flutter is a scrollable list of widgets. It is perfect for displaying a list of items fetched from the internet. In the example above, we use `ListView` to display each post as a `ListTile`.

#### Populating ListView with Fetched Data

When the `Future` completes successfully, and we have data to display, we use the `ListView` widget to display each item. The `ListView` is populated by mapping over the list of posts and creating a `ListTile` for each one.

```dart
ListView(
  children: snapshot.data!.map((post) => ListTile(
    title: Text(post.title),
    subtitle: Text(post.body),
  )).toList(),
)
```

### Handling Loading and Error States

Handling loading and error states is crucial for providing a good user experience. Users should be informed when data is being loaded and when an error occurs.

#### Showing a Loading Indicator

While the `Future` is running, we display a `CircularProgressIndicator` to indicate that data is being loaded. This is done by checking if the `connectionState` is `ConnectionState.waiting`.

```dart
if (snapshot.connectionState == ConnectionState.waiting) {
  return Center(child: CircularProgressIndicator());
}
```

#### Displaying an Error Message

If the `Future` completes with an error, we display an error message. This is done by checking if `snapshot.hasError` is true.

```dart
else if (snapshot.hasError) {
  return Center(child: Text('An error occurred: ${snapshot.error}'));
}
```

### Example Implementation: Displaying Posts

Let's continue with our example of displaying a list of posts. Assume you have a `Post` class and a function `fetchPosts()` that returns a `Future<List<Post>>`.

```dart
class Post {
  final String title;
  final String body;

  Post({required this.title, required this.body});
}

Future<List<Post>> fetchPosts() async {
  final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));

  if (response.statusCode == 200) {
    List jsonResponse = json.decode(response.body);
    return jsonResponse.map((post) => Post(title: post['title'], body: post['body'])).toList();
  } else {
    throw Exception('Failed to load posts');
  }
}
```

In your `PostsPage`, you can use the `fetchPosts()` function to get the posts and display them using `FutureBuilder`.

```dart
class PostsPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Posts')),
      body: FutureBuilder<List<Post>>(
        future: fetchPosts(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('An error occurred: ${snapshot.error}'));
          } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return Center(child: Text('No posts found.'));
          } else {
            return ListView(
              children: snapshot.data!.map((post) => ListTile(
                title: Text(post.title),
                subtitle: Text(post.body),
              )).toList(),
            );
          }
        },
      ),
    );
  }
}
```

### Visual Aids: Screenshots of the App

To better understand how the app looks, here are some screenshots of the app displaying the list of posts:

- **Loading State**: A screen with a circular progress indicator.
- **Error State**: A screen displaying an error message.
- **Data Display**: A screen showing a list of posts with titles and bodies.

### Best Practices and Optimization Tips

- **Responsive UI**: Ensure that your UI responds quickly to user interactions and updates as soon as data is available.
- **Error Handling**: Always handle errors gracefully and provide feedback to the user.
- **Performance**: Consider using `ListView.builder` for large lists to improve performance by lazily building list items.
- **User Experience**: Think about the user's experience during data loading and errors. Provide clear and concise messages.

### Common Pitfalls

- **Not Handling All States**: Ensure that you handle all possible states of the `Future`, including loading, error, and empty data states.
- **Blocking the UI**: Avoid blocking the UI thread with long-running operations. Always perform network requests asynchronously.
- **Ignoring Errors**: Failing to handle errors can lead to a poor user experience. Always provide error messages and consider retry mechanisms.

### Conclusion

Displaying fetched data in a Flutter app involves understanding how to work with asynchronous operations and updating the UI accordingly. By using `FutureBuilder` and `ListView`, you can create responsive and user-friendly interfaces that handle loading and error states effectively. Remember to always consider the user's experience and handle all possible states of the `Future`.

## Quiz Time!

{{< quizdown >}}

### What widget in Flutter helps in building widgets based on the state of a `Future`?

- [x] FutureBuilder
- [ ] StreamBuilder
- [ ] ListView
- [ ] Scaffold

> **Explanation:** `FutureBuilder` is used to build widgets based on the state of a `Future`.

### Which `ConnectionState` indicates that the `Future` is still running?

- [ ] none
- [x] waiting
- [ ] active
- [ ] done

> **Explanation:** `ConnectionState.waiting` indicates that the `Future` is still running.

### How do you display a loading indicator while data is being fetched?

- [x] Use CircularProgressIndicator
- [ ] Use LinearProgressIndicator
- [ ] Use a Text widget
- [ ] Use a Placeholder widget

> **Explanation:** `CircularProgressIndicator` is commonly used to display a loading indicator.

### What should you do if the `Future` completes with an error?

- [x] Display an error message
- [ ] Ignore the error
- [ ] Retry the operation without informing the user
- [ ] Display a success message

> **Explanation:** Displaying an error message informs the user about the issue.

### Which widget is used to display a scrollable list of items in Flutter?

- [ ] Column
- [ ] Row
- [x] ListView
- [ ] Stack

> **Explanation:** `ListView` is used to display a scrollable list of items.

### What method can improve performance when displaying a large list of items?

- [ ] ListView
- [x] ListView.builder
- [ ] ListView.custom
- [ ] ListView.separated

> **Explanation:** `ListView.builder` improves performance by lazily building list items.

### What should you check to determine if the `Future` has completed with data?

- [ ] snapshot.hasError
- [x] snapshot.hasData
- [ ] snapshot.connectionState
- [ ] snapshot.data

> **Explanation:** `snapshot.hasData` checks if the `Future` has completed with data.

### What is a common pitfall when using `FutureBuilder`?

- [ ] Handling all states
- [x] Not handling all possible states of the `Future`
- [ ] Using `ListView.builder`
- [ ] Displaying a loading indicator

> **Explanation:** Not handling all possible states of the `Future` is a common pitfall.

### What should you consider for a good user experience during data loading?

- [x] Provide clear loading indicators and error messages
- [ ] Ignore loading states
- [ ] Display only error messages
- [ ] Use complex animations

> **Explanation:** Providing clear loading indicators and error messages improves user experience.

### True or False: `FutureBuilder` can only be used with network requests.

- [ ] True
- [x] False

> **Explanation:** `FutureBuilder` can be used with any asynchronous operation, not just network requests.

{{< /quizdown >}}
