# Storing Data Locally in Flutter

Asked Claude 3.7 Sonnet (12/04/2025)

## Q: In Flutter, which package or techinque is good to implement saving data in local storage? Can you suggest some options?

In Flutter, there are several excellent options for local storage. Here are some of the most popular and effective packages:

1. **shared_preferences** - Lightweight key-value storage perfect for simple data like settings and preferences. It's a Flutter plugin that wraps platform-specific persistent storage (NSUserDefaults on iOS, SharedPreferences on Android).

2. **sqflite** - SQLite implementation for Flutter that provides a robust relational database solution when you need structured data storage with queries and relationships.

3. **Hive** - Lightweight and blazingly fast key-value database written in pure Dart. It's known for its exceptional performance and doesn't require native dependencies.

4. **Moor/drift** (formerly Moor) - Type-safe reactive persistence library for Flutter built on top of SQLite, offering compile-time verification and reactive streams.

5. **ObjectBox** - NoSQL, object-oriented database with excellent performance, especially for larger datasets.

6. **isar** - Fast cross-platform database with great Flutter integration, supports complex queries, and has good performance characteristics.

For simple use cases like storing user preferences or small amounts of data, shared_preferences or Hive is usually sufficient. For more complex data relationships or larger datasets, sqflite, drift, or ObjectBox would be more appropriate.

Which type of data are you planning to store locally? That would help me recommend the best option for your specific needs.

## Q: I would like to save an item list a user creates. Assume it is similar to Todo list. But I want to keep completed items(not active items) as a log/history, so it is expected to get larger over time (but not so intensive).

For your todo list application with history tracking, I'd recommend either **Hive** or **sqflite/drift** based on your requirements:

### Hive
Hive would be an excellent choice for your use case because:
- It's very fast and lightweight
- Simple to implement for a todo list structure
- Works well for growing datasets
- Pure Dart implementation (no native dependencies)
- Strong Flutter integration

### Sqflite/Drift
If you anticipate needing more complex queries or relationships in the future:
- Drift (formerly Moor) provides type safety on top of SQLite
- Better for complex filtering (like searching completed items by date ranges)
- Good for structured relational data

### Implementation Example with Hive

Since your requirements mention a todo list that will grow over time but isn't expected to be extremely large, I'd personally lean toward Hive for its simplicity and performance. Here's how you could implement it:

1. First, add Hive to your `pubspec.yaml`:
```yaml
dependencies:
  flutter:
    sdk: flutter
  hive: ^2.2.3
  hive_flutter: ^1.1.0

dev_dependencies:
  hive_generator: ^2.0.0
  build_runner: ^2.3.3
```

2. Create a Todo model class:

```dart
import 'package:hive/hive.dart';
part 'todo_item.g.dart';

@HiveType(typeId: 0)
class TodoItem {
  @HiveField(0)
  final String id;

  @HiveField(1)
  String title;

  @HiveField(2)
  String? description;

  @HiveField(3)
  bool isCompleted;

  @HiveField(4)
  DateTime createdAt;

  @HiveField(5)
  DateTime? completedAt;

  TodoItem({
    required this.id,
    required this.title,
    this.description,
    this.isCompleted = false,
    required this.createdAt,
    this.completedAt,
  });
}
```

3. Initialize Hive in your main.dart:

```dart
import 'package:flutter/material.dart';
import 'package:hive_flutter/hive_flutter.dart';
import 'models/todo_item.dart';

void main() async {
  await Hive.initFlutter();
  Hive.registerAdapter(TodoItemAdapter());
  await Hive.openBox<TodoItem>('todoBox');
  runApp(MyApp());
}
```

4. Create a simple repository class to manage todos:

```dart
class TodoRepository {
  final Box<TodoItem> _todoBox = Hive.box<TodoItem>('todoBox');

  // Get all todos
  List<TodoItem> getAllTodos() {
    return _todoBox.values.toList();
  }

  // Get active todos
  List<TodoItem> getActiveTodos() {
    return _todoBox.values.where((todo) => !todo.isCompleted).toList();
  }

  // Get completed todos (history)
  List<TodoItem> getCompletedTodos() {
    return _todoBox.values.where((todo) => todo.isCompleted).toList();
  }

  // Add new todo
  Future<void> addTodo(TodoItem todo) async {
    await _todoBox.put(todo.id, todo);
  }

  // Mark todo as complete
  Future<void> completeTodo(String id) async {
    final todo = _todoBox.get(id);
    if (todo != null) {
      todo.isCompleted = true;
      todo.completedAt = DateTime.now();
      await _todoBox.put(id, todo);
    }
  }

  // Delete todo
  Future<void> deleteTodo(String id) async {
    await _todoBox.delete(id);
  }
}
```

This implementation would handle your requirements efficiently while allowing for future growth. Hive's performance is excellent even with thousands of records, which should be more than sufficient for a todo list with history.

Would you like me to elaborate on any particular aspect of this solution or would you prefer seeing an example with one of the other options?

## Q: I searched up about hive and I noticed hive/isar is getting a concern that the author might have stopped maintaining it. Can you check it? Is it still safe to use Hive when thinking of releasing the app as a production and maintaining it for a long time?

You're right to be concerned about long-term maintenance when choosing a package for production apps. Let me check on the current status of Hive and Isar:

### Hive Status
Looking at Hive's GitHub repository:
- The main Hive package has seen limited activity in recent times
- There have been concerns in the community about its maintenance status
- The last significant updates were quite a while ago
- Some open issues and pull requests haven't received attention

This does raise legitimate concerns about using Hive for new production applications that you plan to maintain long-term.

### Isar Status
Regarding Isar:
- Isar is actually developed by the same creator as Hive (Simon Leier)
- Isar appears to be the spiritual successor to Hive with more active development
- It has more recent commits and releases
- The developer seems to be focusing more on Isar than Hive now

### Safe Alternatives for Production

Given these concerns, here are safer alternatives for a production app:

1. **Drift (formerly Moor)** - Very actively maintained, with regular updates and a strong community. It's built on SQLite and provides type safety.

2. **SQLite using sqflite** - The Flutter sqflite plugin is well-maintained and SQLite itself is extremely stable. This is probably the safest option for long-term support.

3. **ObjectBox** - Good activity, commercial backing which often implies better long-term support.

4. **Isar** - If you like Hive's approach, Isar is the more actively maintained successor by the same developer.

For a production app that you'll maintain for years, I'd recommend either **Drift** or **sqflite** as they have the strongest maintenance records and community support. SQLite in particular has been around for decades and will almost certainly continue to be supported.

Would you like me to provide implementation examples for either of these more actively maintained options?


## Additional info

### Comparison
**sqflite** is more straightforward and requires less setup, but you need to handle type conversion manually.

**Drift** requires more initial setup with code generation, but provides:

 - Type safety
 - Reactive streams out of the box
 - Auto-completion in your IDE
 - Compile-time SQL validation
 - More maintainable as your app grows

For a production app that will grow over time, Drift is generally the better choice as it prevents many common errors through its type system and makes maintenance easier. However, if you prefer simplicity, sqflite is also a solid choice.
