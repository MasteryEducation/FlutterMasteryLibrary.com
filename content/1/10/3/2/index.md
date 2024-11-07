---
linkTitle: "10.3.2 Push Notifications with FCM"
title: "Mastering Push Notifications with Firebase Cloud Messaging (FCM) in Flutter"
description: "Learn how to implement push notifications in your Flutter app using Firebase Cloud Messaging (FCM). This comprehensive guide covers setup, handling notifications, sending messages, and best practices."
categories:
- Mobile Development
- Flutter
- Firebase
tags:
- Flutter
- Firebase Cloud Messaging
- Push Notifications
- Mobile App Development
- FCM
date: 2024-10-25
type: docs
nav_weight: 1032000
---

## 10.3.2 Push Notifications with FCM

Push notifications are a crucial component of modern mobile applications, providing a direct line of communication between your app and its users. Firebase Cloud Messaging (FCM) is a powerful tool that allows you to send notifications and messages across different platforms, including Android, iOS, and the web. In this section, we will explore how to integrate FCM into your Flutter app, handle notifications, and implement best practices for effective messaging.

### Introduction to Firebase Cloud Messaging (FCM)

Firebase Cloud Messaging (FCM) is a cross-platform messaging solution that lets you reliably deliver messages at no cost. With FCM, you can notify a client app that new email or other data is available to sync. You can send notifications to drive user re-engagement and retention. FCM provides a reliable and battery-efficient connection between your server and devices that allows you to deliver and receive messages and notifications on iOS, Android, and the web at no cost.

### Setting Up FCM in Your Flutter App

Setting up FCM in your Flutter app involves several steps, including adding the necessary dependencies, configuring your Android and iOS projects, and initializing FCM in your app.

#### Adding `firebase_messaging` Package

To start using FCM in your Flutter app, you need to add the `firebase_messaging` package to your project. This package provides the necessary tools to handle FCM notifications.

1. Open your `pubspec.yaml` file and add the `firebase_messaging` dependency:

    ```yaml
    dependencies:
      firebase_messaging: ^14.2.5
    ```

2. Run the following command to install the package:

    ```bash
    flutter pub get
    ```

#### Android Setup

For Android, you need to configure your project to use Firebase services.

1. **Add `google-services.json`:** Download the `google-services.json` file from your Firebase project settings and place it in the `android/app/` directory.

2. **Update `AndroidManifest.xml`:** Add the necessary permissions and services to your `AndroidManifest.xml` file:

    ```xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.app">

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

        <application
            android:name=".MyApplication"
            android:label="app_name"
            android:icon="@mipmap/ic_launcher">
            
            <service
                android:name="com.google.firebase.messaging.FirebaseMessagingService"
                android:exported="true">
                <intent-filter>
                    <action android:name="com.google.firebase.MESSAGING_EVENT"/>
                </intent-filter>
            </service>

            <receiver
                android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
                android:exported="true"
                android:permission="com.google.android.c2dm.permission.SEND">
                <intent-filter>
                    <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                    <category android:name="com.example.app"/>
                </intent-filter>
            </receiver>

        </application>
    </manifest>
    ```

3. **Apply the Google Services Plugin:** In your `android/build.gradle` file, add the Google services classpath:

    ```groovy
    buildscript {
        dependencies {
            classpath 'com.google.gms:google-services:4.3.10'
        }
    }
    ```

   Then, apply the plugin in your `android/app/build.gradle` file:

    ```groovy
    apply plugin: 'com.google.gms.google-services'
    ```

#### iOS Setup

For iOS, the setup involves configuring your Xcode project to handle push notifications.

1. **Add `GoogleService-Info.plist`:** Download the `GoogleService-Info.plist` file from your Firebase project settings and add it to your Xcode project. Ensure that it is included in the target's build settings.

2. **Enable Push Notifications and Background Modes:** In your Xcode project, navigate to the **Signing & Capabilities** tab and enable **Push Notifications** and **Background Modes**. Under Background Modes, check **Remote notifications**.

3. **Request Permissions:** Ensure your app requests notification permissions from the user. This is crucial for iOS devices.

### Requesting Permissions

To receive notifications, your app must request permission from the user. This is particularly important for iOS, where users must explicitly grant permission for notifications.

#### For iOS

Use the following code to request notification permissions:

```dart
FirebaseMessaging messaging = FirebaseMessaging.instance;

await messaging.requestPermission(
  alert: true,
  announcement: false,
  badge: true,
  carPlay: false,
  criticalAlert: false,
  provisional: false,
  sound: true,
);
```

This code requests permission for various types of notifications, such as alerts, badges, and sounds.

### Handling Notifications in the App

Once your app is set up to receive notifications, you need to handle them appropriately. This involves listening for incoming messages and responding to them.

#### Receiving Messages

To handle incoming messages while the app is in the foreground, use the `onMessage` stream:

```dart
FirebaseMessaging.onMessage.listen((RemoteMessage message) {
  print('Received message: ${message.notification?.title}');
  // Handle the message
});
```

This listener will be triggered whenever a message is received while the app is in the foreground.

#### Background Messages (Android)

For Android, you need to implement a background message handler to process messages when the app is not in the foreground:

```dart
Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  print('Handling a background message: ${message.messageId}');
}

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
  runApp(MyApp());
}
```

This handler will be called whenever a message is received while the app is in the background or terminated.

### Sending Notifications

There are several ways to send notifications using FCM, including the Firebase Console and programmatically using Cloud Functions.

#### Using Firebase Console

The Firebase Console provides a simple interface for sending test messages to your app:

1. Navigate to **Cloud Messaging** in the Firebase Console.
2. Click on **Send your first message**.
3. Enter the message details and select the target audience.
4. Click **Send message** to deliver the notification.

#### Using Cloud Functions

For more advanced use cases, you can send notifications programmatically using Firebase Cloud Functions. This allows you to trigger notifications based on events in your app, such as a new message being added to a Firestore collection.

```javascript
const admin = require('firebase-admin');
admin.initializeApp();

exports.sendPushNotification = functions.firestore.document('messages/{messageId}')
  .onCreate((snapshot, context) => {
    const messageData = snapshot.data();
    const payload = {
      notification: {
        title: 'New message',
        body: messageData.text,
      },
    };

    return admin.messaging().sendToTopic('all', payload);
  });
```

This function listens for new documents in the `messages` collection and sends a notification to all subscribed users.

### Managing Subscriptions

FCM allows you to manage subscriptions to topics, enabling you to send targeted messages to groups of users.

#### Topics

You can subscribe users to topics and send messages to all devices subscribed to a particular topic:

```dart
await FirebaseMessaging.instance.subscribeToTopic('all');
```

This code subscribes the current device to the `all` topic, allowing it to receive messages sent to this topic.

#### Device Tokens

Each device that registers with FCM receives a unique token. You can use these tokens to send targeted messages to specific devices.

```dart
String? token = await FirebaseMessaging.instance.getToken();
print('FCM Token: $token');
```

Store these tokens in your database to send personalized messages to individual users.

### Handling Notification Clicks

When a user clicks on a notification, you may want to navigate them to a specific screen in your app. You can handle this using the `onMessageOpenedApp` stream:

```dart
FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
  print('Notification clicked!');
  // Navigate to specific screen
});
```

This listener will be triggered whenever the app is opened from a notification click.

### Best Practices

To make the most of push notifications, consider the following best practices:

- **Personalize Notifications:** Tailor messages to individual users based on their preferences and behavior to increase engagement.
- **Respect User Preferences:** Provide users with options to customize their notification settings and opt-out if desired.
- **Monitor Delivery and Open Rates:** Use analytics to track the effectiveness of your notifications and optimize your messaging strategy.

### Exercise

To reinforce your understanding of FCM, try implementing push notifications in your app. Follow these steps:

1. Set up FCM in your Flutter app as described above.
2. Send a test notification using the Firebase Console.
3. Handle the notification in your app and navigate to a specific screen when the notification is clicked.

By completing this exercise, you'll gain hands-on experience with FCM and be better prepared to implement push notifications in your own projects.

## Quiz Time!

{{< quizdown >}}

### What is Firebase Cloud Messaging (FCM) used for?

- [x] Sending notifications and messages across different platforms
- [ ] Storing user data in the cloud
- [ ] Analyzing app performance
- [ ] Managing user authentication

> **Explanation:** FCM is primarily used for sending notifications and messages to users across different platforms such as Android, iOS, and the web.

### Which package is required to use FCM in a Flutter app?

- [x] `firebase_messaging`
- [ ] `firebase_core`
- [ ] `firebase_auth`
- [ ] `firebase_storage`

> **Explanation:** The `firebase_messaging` package is specifically designed for handling Firebase Cloud Messaging in Flutter apps.

### Where should the `google-services.json` file be placed in an Android project?

- [x] `android/app/`
- [ ] `android/`
- [ ] `lib/`
- [ ] `assets/`

> **Explanation:** The `google-services.json` file should be placed in the `android/app/` directory to configure Firebase services for the Android app.

### What is the purpose of the `onMessage` stream in FCM?

- [x] To handle incoming messages while the app is in the foreground
- [ ] To send messages to the server
- [ ] To initialize Firebase services
- [ ] To manage user authentication

> **Explanation:** The `onMessage` stream is used to handle incoming messages while the app is in the foreground.

### How can you send notifications programmatically using Firebase?

- [x] Using Firebase Cloud Functions
- [ ] Using Firebase Authentication
- [ ] Using Firebase Storage
- [ ] Using Firebase Analytics

> **Explanation:** Firebase Cloud Functions can be used to send notifications programmatically based on events in your app.

### What is the purpose of subscribing to topics in FCM?

- [x] To send targeted messages to groups of users
- [ ] To store user preferences
- [ ] To authenticate users
- [ ] To analyze app performance

> **Explanation:** Subscribing to topics allows you to send targeted messages to groups of users who have subscribed to a particular topic.

### How can you get the FCM token for a device?

- [x] Using `FirebaseMessaging.instance.getToken()`
- [ ] Using `FirebaseAuth.instance.currentUser()`
- [ ] Using `FirebaseStorage.instance.ref()`
- [ ] Using `FirebaseAnalytics.instance.logEvent()`

> **Explanation:** The `getToken()` method of `FirebaseMessaging` is used to retrieve the FCM token for a device.

### Which capability must be enabled in Xcode for iOS push notifications?

- [x] Push Notifications
- [ ] Background Fetch
- [ ] App Groups
- [ ] HealthKit

> **Explanation:** The Push Notifications capability must be enabled in Xcode for the app to receive push notifications on iOS.

### What should you do when a notification is clicked to navigate the user?

- [x] Use the `onMessageOpenedApp` stream
- [ ] Use the `onMessage` stream
- [ ] Use the `onBackgroundMessage` handler
- [ ] Use the `onNotificationReceived` method

> **Explanation:** The `onMessageOpenedApp` stream is used to handle actions when a notification is clicked and the app is opened.

### True or False: FCM can be used to send notifications to web applications.

- [x] True
- [ ] False

> **Explanation:** FCM supports sending notifications to web applications in addition to Android and iOS platforms.

{{< /quizdown >}}
