---
linkTitle: "7.3.4 Text Input Enhancements"
title: "Text Input Enhancements: Auto-Completion, Filtering, and More"
description: "Explore advanced text input enhancements in Flutter, including auto-completion, input filtering, and visual aids for improved user experience."
categories:
- Flutter Development
- User Interface Design
- Mobile App Development
tags:
- Flutter
- Text Input
- Auto-Completion
- Input Filtering
- User Experience
date: 2024-10-25
type: docs
nav_weight: 734000
---

## 7.3.4 Text Input Enhancements

In the world of mobile app development, user input is a critical component that can significantly impact the user experience. Flutter provides a robust set of tools and packages to enhance text input fields, making them more intuitive and user-friendly. This section delves into various techniques to improve text input fields, including auto-completion, input filtering, masking, and visual enhancements.

### Auto-Completion and Suggestions

Auto-completion and suggestions can greatly enhance the user experience by reducing the amount of typing required and helping users make accurate entries. The `flutter_typeahead` package is a powerful tool for implementing these features in Flutter applications.

#### Implementing Auto-Completion with `flutter_typeahead`

The `flutter_typeahead` package provides a simple way to add auto-completion to your text fields. Here's a basic example of how to use it:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_typeahead/flutter_typeahead.dart';

class AutoCompleteExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Auto-Completion Example')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TypeAheadFormField(
          textFieldConfiguration: TextFieldConfiguration(
            decoration: InputDecoration(labelText: 'City'),
          ),
          suggestionsCallback: (pattern) {
            return CitiesService.getSuggestions(pattern);
          },
          itemBuilder: (context, String suggestion) {
            return ListTile(
              title: Text(suggestion),
            );
          },
          onSuggestionSelected: (String suggestion) {
            // Handle the selected suggestion
            print('Selected: $suggestion');
          },
        ),
      ),
    );
  }
}

class CitiesService {
  static List<String> getSuggestions(String query) {
    List<String> cities = ['New York', 'Los Angeles', 'Chicago', 'Houston', 'Phoenix'];
    return cities.where((city) => city.toLowerCase().contains(query.toLowerCase())).toList();
  }
}
```

**Explanation:**

- **TypeAheadFormField**: This widget provides a text field with auto-completion capabilities.
- **suggestionsCallback**: This function is called whenever the user types into the text field. It returns a list of suggestions based on the input pattern.
- **itemBuilder**: This function builds the UI for each suggestion.
- **onSuggestionSelected**: This callback is triggered when a user selects a suggestion.

### Filtering Input

Input filtering is essential for ensuring that users enter data in the correct format. Flutter's `FilteringTextInputFormatter` allows you to restrict input to specific characters.

#### Example: Allowing Only Hexadecimal Characters

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

class HexInputExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Hexadecimal Input Example')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          decoration: InputDecoration(labelText: 'Enter Hex Value'),
          inputFormatters: [
            FilteringTextInputFormatter.allow(RegExp(r'[0-9a-fA-F]')),
          ],
        ),
      ),
    );
  }
}
```

**Explanation:**

- **FilteringTextInputFormatter.allow**: This formatter restricts input to characters that match the provided regular expression. In this case, it allows only hexadecimal characters (0-9, a-f, A-F).

### Masking Input Fields

Input masks are useful for formatting inputs like phone numbers, credit card numbers, or dates. The `mask_text_input_formatter` package can be used to apply masks to text fields.

#### Example: Phone Number Masking

```dart
import 'package:flutter/material.dart';
import 'package:mask_text_input_formatter/mask_text_input_formatter.dart';

class PhoneNumberMaskExample extends StatelessWidget {
  final maskFormatter = MaskTextInputFormatter(mask: '(###) ###-####');

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Phone Number Mask Example')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          decoration: InputDecoration(labelText: 'Phone Number'),
          keyboardType: TextInputType.phone,
          inputFormatters: [maskFormatter],
        ),
      ),
    );
  }
}
```

**Explanation:**

- **MaskTextInputFormatter**: This formatter applies a mask to the input field. The mask `(###) ###-####` formats the input as a phone number.

### Adding Prefixes and Suffixes

Enhancing text fields with prefixes and suffixes can provide additional context or functionality. For example, you might add an icon or text to indicate the type of input expected.

#### Example: Adding Icons and Text

```dart
import 'package:flutter/material.dart';

class PrefixSuffixExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Prefix and Suffix Example')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TextField(
          decoration: InputDecoration(
            labelText: 'Username',
            prefixIcon: Icon(Icons.person),
            suffixText: '@example.com',
          ),
        ),
      ),
    );
  }
}
```

**Explanation:**

- **prefixIcon**: Adds an icon before the input field.
- **suffixText**: Adds text after the input field, useful for domains or units.

### Visual Aids

Visual aids can significantly enhance the usability of input fields. Here are some examples of how you can apply these enhancements:

- **Auto-Completion**: Use a dropdown list to show suggestions as the user types.
- **Input Filtering**: Highlight invalid input with a red border or error message.
- **Input Masking**: Display the mask pattern as a placeholder to guide the user.

### Best Practices

When enhancing text input fields, it's important to ensure that these enhancements improve usability without confusing the user. Here are some best practices:

- **Clarity**: Ensure that input fields are clearly labeled and provide hints where necessary.
- **Accessibility**: Make sure that enhancements do not hinder accessibility. Use labels and hints that can be read by screen readers.
- **Consistency**: Maintain consistency in input field design across your application.

### Exercise

As a practical exercise, try creating an email input field that automatically appends a domain name and provides suggestions from a list of contacts. Here's a starting point:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_typeahead/flutter_typeahead.dart';

class EmailInputExample extends StatelessWidget {
  final List<String> contacts = ['alice@example.com', 'bob@example.com', 'charlie@example.com'];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Email Input Example')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: TypeAheadFormField(
          textFieldConfiguration: TextFieldConfiguration(
            decoration: InputDecoration(
              labelText: 'Email',
              suffixText: '@example.com',
            ),
          ),
          suggestionsCallback: (pattern) {
            return contacts.where((contact) => contact.contains(pattern)).toList();
          },
          itemBuilder: (context, String suggestion) {
            return ListTile(
              title: Text(suggestion),
            );
          },
          onSuggestionSelected: (String suggestion) {
            // Handle the selected suggestion
            print('Selected: $suggestion');
          },
        ),
      ),
    );
  }
}
```

### Conclusion

Enhancing text input fields in Flutter can greatly improve the user experience by making data entry more intuitive and efficient. By implementing features like auto-completion, input filtering, and visual aids, you can create a more engaging and user-friendly application. Remember to always consider usability and accessibility when designing your input fields.

## Quiz Time!

{{< quizdown >}}

### What package can be used for auto-completion in Flutter?

- [x] flutter_typeahead
- [ ] flutter_autocomplete
- [ ] flutter_suggestions
- [ ] flutter_input

> **Explanation:** The `flutter_typeahead` package is used for providing auto-completion and suggestions in Flutter applications.

### How can you restrict input to hexadecimal characters in a TextField?

- [x] Using FilteringTextInputFormatter.allow(RegExp(r'[0-9a-fA-F]'))
- [ ] Using TextInputFormatter.deny(RegExp(r'[^0-9a-fA-F]'))
- [ ] Using TextInputFormatter.allow(RegExp(r'[a-zA-Z]'))
- [ ] Using FilteringTextInputFormatter.deny(RegExp(r'[0-9a-fA-F]'))

> **Explanation:** The `FilteringTextInputFormatter.allow` method with a regular expression allows only specified characters, in this case, hexadecimal characters.

### Which package can be used to apply input masks in Flutter?

- [x] mask_text_input_formatter
- [ ] input_mask_formatter
- [ ] text_mask_formatter
- [ ] mask_input_formatter

> **Explanation:** The `mask_text_input_formatter` package is used to apply input masks to text fields in Flutter.

### What is the purpose of adding a prefixIcon to a TextField?

- [x] To provide additional context or functionality before the input area
- [ ] To add a suffix after the input area
- [ ] To restrict input to certain characters
- [ ] To apply an input mask

> **Explanation:** Adding a `prefixIcon` to a `TextField` provides additional context or functionality before the input area, such as indicating the type of input expected.

### What is a best practice when enhancing text input fields?

- [x] Ensure enhancements improve usability without confusing the user
- [ ] Add as many enhancements as possible for a rich experience
- [ ] Avoid using labels and hints
- [ ] Use complex regular expressions for input filtering

> **Explanation:** Enhancements should improve usability without confusing the user, and input fields should be clearly labeled and provide hints where necessary.

### How can you provide suggestions from a list of contacts in a TypeAheadFormField?

- [x] Using the suggestionsCallback to filter contacts based on the input pattern
- [ ] Using a static list of suggestions
- [ ] Using a prefixIcon to display suggestions
- [ ] Using a suffixText to display suggestions

> **Explanation:** The `suggestionsCallback` function is used to filter and provide suggestions based on the user's input pattern.

### What is the role of the itemBuilder in a TypeAheadFormField?

- [x] To build the UI for each suggestion
- [ ] To filter suggestions based on the input pattern
- [ ] To handle the selected suggestion
- [ ] To apply input masks

> **Explanation:** The `itemBuilder` function is responsible for building the UI for each suggestion in a `TypeAheadFormField`.

### How can you ensure accessibility when enhancing text input fields?

- [x] Use labels and hints that can be read by screen readers
- [ ] Avoid using labels and hints
- [ ] Use complex input masks
- [ ] Add as many visual aids as possible

> **Explanation:** Ensuring accessibility involves using labels and hints that can be read by screen readers, making the input fields accessible to all users.

### What is a common use case for input masking?

- [x] Formatting inputs like phone numbers or credit card numbers
- [ ] Providing auto-completion suggestions
- [ ] Adding prefixes and suffixes
- [ ] Filtering input to specific characters

> **Explanation:** Input masking is commonly used for formatting inputs like phone numbers, credit card numbers, or dates.

### True or False: Enhancements should always prioritize aesthetics over functionality.

- [ ] True
- [x] False

> **Explanation:** Enhancements should prioritize functionality and usability over aesthetics to ensure a user-friendly experience.

{{< /quizdown >}}
