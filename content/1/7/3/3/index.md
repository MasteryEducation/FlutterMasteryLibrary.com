---
linkTitle: "7.3.3 SQLite Databases with `sqflite`"
title: "SQLite Databases with `sqflite` in Flutter: A Comprehensive Guide"
description: "Explore SQLite databases in Flutter using the `sqflite` package. Learn to perform CRUD operations, manage data models, and implement best practices for efficient data storage and retrieval."
categories:
- Mobile Development
- Flutter
- Database Management
tags:
- Flutter
- SQLite
- sqflite
- Mobile Apps
- Database
date: 2024-10-25
type: docs
nav_weight: 733000
canonical: "https://fluttermasterylibrary.com/1/7/3/3"
license: "© 2023 Tokenizer Inc. CC BY-NC-SA 4.0"
---

## 7.3.3 SQLite Databases with `sqflite`

In the world of mobile app development, efficient data management is crucial. SQLite, a lightweight database engine, offers a robust solution for structured data storage with complex querying capabilities. In this section, we delve into using SQLite in Flutter applications through the `sqflite` package, covering everything from setup to advanced data operations.

### Introduction to SQLite

SQLite is a self-contained, serverless, and zero-configuration database engine, making it an ideal choice for mobile applications. Its lightweight nature allows it to be embedded directly into the application, eliminating the need for a separate database server. This integration is particularly beneficial for mobile devices where resources are limited, and efficiency is paramount.

SQLite is suitable for applications requiring structured data storage, complex querying, and transactional operations. It supports a wide range of SQL features, including transactions, subqueries, triggers, and views, enabling developers to build sophisticated data-driven applications.

### Adding `sqflite` Package

To integrate SQLite into your Flutter application, the `sqflite` package is a popular choice. It provides a high-level API for interacting with SQLite databases, making it easier to perform CRUD (Create, Read, Update, Delete) operations.

#### Step 1: Update `pubspec.yaml`

To add the `sqflite` package to your Flutter project, update your `pubspec.yaml` file as follows:

```yaml
dependencies:
  sqflite: ^2.0.0
  path: ^1.8.0
```

The `path` package is also included to assist with file path manipulation, which is often necessary when working with databases.

#### Step 2: Install Packages

Run the following command to install the new dependencies:

```bash
flutter pub get
```

### Setting Up the Database

With the `sqflite` package installed, the next step is to set up the database within your Flutter application.

#### Import Packages

Begin by importing the necessary packages in your Dart file:

```dart
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';
```

#### Opening the Database

Opening a database involves specifying the database file's path and defining the schema. Here's how you can open a database and create a table:

```dart
Future<Database> get database async {
  return openDatabase(
    join(await getDatabasesPath(), 'my_database.db'),
    onCreate: (db, version) {
      return db.execute(
        'CREATE TABLE items(id INTEGER PRIMARY KEY, name TEXT)',
      );
    },
    version: 1,
  );
}
```

In this example, the `openDatabase` function is used to open a database located at `my_database.db`. The `onCreate` callback is executed when the database is created for the first time, allowing you to define the database schema.

### CRUD Operations

CRUD operations form the backbone of any database-driven application. Let's explore how to perform these operations using the `sqflite` package.

#### Create (Insert)

Inserting data into the database is straightforward. Here's how you can insert a new item:

```dart
Future<void> insertItem(Item item) async {
  final db = await database;
  await db.insert(
    'items',
    item.toMap(),
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
}
```

The `insert` method inserts a new row into the `items` table. The `conflictAlgorithm` parameter specifies how to handle conflicts, such as when a row with the same primary key already exists.

#### Read (Query)

Reading data from the database involves querying the table and mapping the results to Dart objects:

```dart
Future<List<Item>> items() async {
  final db = await database;
  final List<Map<String, dynamic>> maps = await db.query('items');
  return List.generate(maps.length, (i) {
    return Item(
      id: maps[i]['id'],
      name: maps[i]['name'],
    );
  });
}
```

The `query` method retrieves all rows from the `items` table. The results are then converted into a list of `Item` objects.

#### Update

Updating existing data requires specifying which row to update using a `WHERE` clause:

```dart
Future<void> updateItem(Item item) async {
  final db = await database;
  await db.update(
    'items',
    item.toMap(),
    where: 'id = ?',
    whereArgs: [item.id],
  );
}
```

The `update` method modifies the specified row in the `items` table. The `where` and `whereArgs` parameters are used to identify the row to update.

#### Delete

Deleting data from the database is similar to updating:

```dart
Future<void> deleteItem(int id) async {
  final db = await database;
  await db.delete(
    'items',
    where: 'id = ?',
    whereArgs: [id],
  );
}
```

The `delete` method removes the specified row from the `items` table.

### Defining Data Models

Data models are essential for representing database records as Dart objects. Here's how you can define an `Item` class with a `toMap()` method:

```dart
class Item {
  final int id;
  final String name;

  Item({required this.id, required this.name});

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
    };
  }
}
```

The `Item` class represents a row in the `items` table. The `toMap()` method converts an `Item` object into a map, which is required for database operations.

### Transactions and Batch Operations

Transactions allow you to perform multiple database operations atomically, ensuring data integrity. Here's how you can use transactions in `sqflite`:

```dart
Future<void> performTransaction() async {
  final db = await database;
  await db.transaction((txn) async {
    await txn.insert('items', {'id': 1, 'name': 'Item 1'});
    await txn.insert('items', {'id': 2, 'name': 'Item 2'});
  });
}
```

In this example, two insert operations are performed within a transaction. If any operation fails, the entire transaction is rolled back.

Batch operations allow you to execute multiple SQL statements in a single call, improving performance:

```dart
Future<void> performBatch() async {
  final db = await database;
  Batch batch = db.batch();
  batch.insert('items', {'id': 3, 'name': 'Item 3'});
  batch.insert('items', {'id': 4, 'name': 'Item 4'});
  await batch.commit();
}
```

### Best Practices

To ensure efficient and reliable database operations, consider the following best practices:

- **Indexing:** Use indexes on columns that are frequently queried to improve performance.
- **Database Versioning:** Increment the database version number whenever you change the schema.
- **Closing the Database:** Always close the database when it's no longer needed to free up resources.

### Practice Exercises

To reinforce your understanding of SQLite and `sqflite`, try building a simple task management app. This app should allow users to add, update, delete, and view tasks. Implement search and filter functionalities to enhance the user experience.

### Conclusion

SQLite, combined with the `sqflite` package, provides a powerful solution for managing structured data in Flutter applications. By mastering the concepts and techniques covered in this section, you'll be well-equipped to build robust, data-driven mobile apps.

## Quiz Time!

{{< quizdown >}}

### What is SQLite?

- [x] A lightweight database engine embedded in mobile devices.
- [ ] A cloud-based database service.
- [ ] A programming language.
- [ ] A type of file storage system.

> **Explanation:** SQLite is a lightweight, embedded database engine that is commonly used in mobile applications for local data storage.

### Which package is used to integrate SQLite in Flutter?

- [x] sqflite
- [ ] firebase
- [ ] http
- [ ] dio

> **Explanation:** The `sqflite` package is used to integrate SQLite databases in Flutter applications.

### How do you open a database using `sqflite`?

- [x] Using the `openDatabase` function.
- [ ] Using the `connectDatabase` function.
- [ ] Using the `initializeDatabase` function.
- [ ] Using the `setupDatabase` function.

> **Explanation:** The `openDatabase` function is used to open a database in `sqflite`.

### What is the purpose of the `onCreate` callback in `openDatabase`?

- [x] To define the database schema when the database is created for the first time.
- [ ] To close the database.
- [ ] To update the database.
- [ ] To delete the database.

> **Explanation:** The `onCreate` callback is used to define the database schema when the database is created for the first time.

### Which method is used to insert data into a table?

- [x] insert
- [ ] add
- [ ] put
- [ ] save

> **Explanation:** The `insert` method is used to add new rows to a table in `sqflite`.

### How do you perform a transaction in `sqflite`?

- [x] Using the `transaction` method.
- [ ] Using the `batch` method.
- [ ] Using the `commit` method.
- [ ] Using the `execute` method.

> **Explanation:** The `transaction` method is used to perform multiple operations atomically in `sqflite`.

### What is the purpose of the `toMap()` method in a data model?

- [x] To convert a Dart object into a map for database operations.
- [ ] To convert a map into a Dart object.
- [ ] To save the object to a file.
- [ ] To send the object over the network.

> **Explanation:** The `toMap()` method is used to convert a Dart object into a map, which is necessary for database operations.

### Why is indexing important in databases?

- [x] To improve query performance.
- [ ] To increase storage space.
- [ ] To simplify database schema.
- [ ] To enhance data security.

> **Explanation:** Indexing improves query performance by allowing the database to quickly locate and access the data.

### What should you do when the database is no longer needed?

- [x] Close the database to free up resources.
- [ ] Delete the database file.
- [ ] Reopen the database.
- [ ] Backup the database.

> **Explanation:** Closing the database when it's no longer needed helps free up system resources.

### True or False: `sqflite` supports transactions.

- [x] True
- [ ] False

> **Explanation:** `sqflite` supports transactions, allowing multiple operations to be performed atomically.

{{< /quizdown >}}
