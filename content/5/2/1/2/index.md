---
linkTitle: "2.1.2 Running the App"
title: "Running Your First Flutter App: From Code to Screen"
description: "Learn how to run your first Flutter app and see your code come to life on an emulator or physical device. Step-by-step guide with troubleshooting tips included."
categories:
- Flutter
- Coding for Kids
- App Development
tags:
- Flutter
- Mobile Development
- Coding Basics
- Kids Coding
- App Running
date: 2024-10-25
type: docs
nav_weight: 212000
---

## 2.1.2 Running the App

Congratulations on creating your first Flutter app! Now it's time to see your hard work in action by running the app and watching "Hello, World!" appear on your screen. This is an exciting moment where your code comes to life, and you'll learn how to run your app on an emulator or a physical device. Let's dive in!

### Objective

The goal of this section is to guide you through the process of running your Flutter app. You'll learn how to connect a device or start an emulator, run your app, and observe the output. We'll also cover some common troubleshooting tips to help you overcome any challenges along the way.

### What Happens When You Run the App?

When you run your Flutter app, the code you wrote is compiled and executed, transforming into a visual display on your screen. This process involves several steps:

1. **Compilation:** Your Dart code is compiled into native code that can run on your device.
2. **Execution:** The compiled code is executed, and the Flutter framework takes over to render the UI.
3. **Display:** The result is displayed on your screen, showing the "Hello, World!" message.

This magical transformation from code to a visual app is what makes programming so exciting!

### Step-by-Step Guide

#### 1. Connect a Device or Start an Emulator

Before running your app, you need to decide where you want to see it. You have two options: a physical device or an emulator.

- **Using a Physical Device:**
  - Connect your smartphone to your computer using a USB cable.
  - Ensure that USB debugging is enabled on your device. You can usually find this option in the Developer Options menu.
  - Your device should be detected by your computer. You can verify this by running the command `flutter devices` in your terminal. Your device should appear in the list.

- **Using an Emulator:**
  - Open your Android Studio or Visual Studio Code.
  - Start an Android emulator by selecting a virtual device from the AVD Manager (Android Virtual Device Manager).
  - Ensure the emulator is running and ready to accept your app.

#### 2. Run the App

Now that your device or emulator is ready, it's time to run your app!

- **Using the Command Line:**
  - Open your terminal or command prompt.
  - Navigate to your Flutter project directory.
  - Run the command `flutter run`. This command compiles your app and launches it on the connected device or emulator.

- **Using the Code Editor:**
  - In Android Studio or Visual Studio Code, open your Flutter project.
  - Locate the run button (usually a green play icon) in the toolbar.
  - Click the run button to start your app.

#### 3. Observe the Output

Once your app is running, you should see the "Hello, World!" message displayed on your screen. This is the result of the code you wrote in your Flutter app. Take a moment to appreciate your achievement—you're seeing your code come to life!

### Troubleshooting Tips

Running your app might not always go smoothly, but don't worry! Here are some common issues and how to solve them:

- **Device Not Detected:**
  - Ensure your device is properly connected via USB.
  - Check that USB debugging is enabled on your device.
  - Try restarting your device and computer.

- **Emulator Not Starting:**
  - Make sure your emulator is properly configured in the AVD Manager.
  - Check that your computer meets the system requirements for running an emulator.
  - Restart the emulator or your computer if necessary.

- **App Not Running:**
  - Verify that you are in the correct project directory.
  - Check for any error messages in the terminal or code editor.
  - Ensure that all necessary dependencies are installed.

### Visuals

Here's what your app should look like when running on an emulator and a physical device:

- **Emulator Screenshot:**

  ![Emulator Screenshot](emulator_screenshot.png)

- **Physical Device Screenshot:**

  ![Physical Device Screenshot](device_screenshot.png)

### Engagement

Running your first app is a big milestone, and it's worth celebrating! Share your "Hello, World!" app with friends or family. Show them what you've created and explain how you did it. This is a great way to inspire others and share your excitement about coding.

Remember, running your app is just the beginning. There are endless possibilities for what you can create with Flutter. Keep experimenting, learning, and having fun!

## Quiz Time!

{{< quizdown >}}

### What is the first step in running your Flutter app?

- [x] Connect a device or start an emulator
- [ ] Write the code
- [ ] Install Flutter
- [ ] Open a web browser

> **Explanation:** Before running your app, you need to have a device or emulator ready to display it.


### What command do you use to run your Flutter app from the terminal?

- [x] `flutter run`
- [ ] `flutter start`
- [ ] `flutter execute`
- [ ] `flutter launch`

> **Explanation:** The `flutter run` command compiles and runs your app on a connected device or emulator.


### What should you do if your device is not detected?

- [x] Ensure USB debugging is enabled
- [ ] Restart your computer
- [ ] Open a web browser
- [ ] Install a new app

> **Explanation:** USB debugging must be enabled for your computer to detect your device.


### What message should appear on your screen when you run your first Flutter app?

- [x] "Hello, World!"
- [ ] "Welcome to Flutter!"
- [ ] "App Running"
- [ ] "Flutter is Fun!"

> **Explanation:** The "Hello, World!" message is the output of your first Flutter app.


### What tool can you use to start an Android emulator?

- [x] AVD Manager
- [ ] USB Debugger
- [ ] Flutter Console
- [ ] Device Manager

> **Explanation:** The AVD Manager is used to create and manage Android emulators.


### What is the purpose of the `flutter devices` command?

- [x] To list all connected devices
- [ ] To run the app
- [ ] To compile the code
- [ ] To open the code editor

> **Explanation:** The `flutter devices` command lists all devices connected to your computer.


### Which of the following is NOT a troubleshooting tip for running your app?

- [ ] Ensure USB debugging is enabled
- [ ] Restart your device
- [x] Open a web browser
- [ ] Check for error messages

> **Explanation:** Opening a web browser is not related to troubleshooting app running issues.


### What should you do if your emulator is not starting?

- [x] Check the emulator configuration
- [ ] Install a new app
- [ ] Open a web browser
- [ ] Write more code

> **Explanation:** Ensuring the emulator is properly configured can help resolve startup issues.


### What is the role of the Flutter framework when running your app?

- [x] To render the UI
- [ ] To compile the code
- [ ] To connect devices
- [ ] To open the terminal

> **Explanation:** The Flutter framework is responsible for rendering the user interface of your app.


### True or False: Running your app is the final step in app development.

- [ ] True
- [x] False

> **Explanation:** Running your app is an important step, but app development involves many other stages, including design, testing, and iteration.

{{< /quizdown >}}
