# Lookup items in a Dart list by attribute

Answered by Claude

## Q: In Flutter/Dart, if I have a List of a class, how can I look up an item in a list that has a specific value in a certain attribute in the class?

I'll help you with that. In Flutter/Dart, there are several ways to find an item in a list based on a specific attribute. I'll show you multiple approaches:

```dart
// Assume we have a class Person
class Person {
  final String name;
  final int age;
  
  Person({required this.name, required this.age});
}

void listLookupExamples() {
  // Sample list of persons
  List<Person> people = [
    Person(name: 'Alice', age: 30),
    Person(name: 'Bob', age: 25),
    Person(name: 'Charlie', age: 35)
  ];

  // Method 1: Using .firstWhere()
  // Finds the first item matching the condition
  Person? findByName = people.firstWhere(
    (person) => person.name == 'Bob',
    orElse: () => null // Optional: returns null if no match
  );

  // Method 2: Using .where() 
  // Returns all items matching the condition
  List<Person> findByAge = people.where(
    (person) => person.age > 30
  ).toList();

  // Method 3: Using .indexWhere()
  // Finds the index of the first matching item
  int index = people.indexWhere(
    (person) => person.name == 'Alice'
  );

  // Method 4: Using .any() 
  // Checks if any item matches the condition
  bool hasYoungPerson = people.any(
    (person) => person.age < 30
  );
}

```

Let me break down these methods:

1. `.firstWhere()`: 
   - Returns the first item that matches your condition
   - Use `orElse` parameter to handle cases where no match is found
   - Throws an error if no match is found and `orElse` is not provided

2. `.where()`:
   - Returns a list of ALL items matching the condition
   - Useful when multiple items might match
   - Always returns an Iterable, so use `.toList()` to convert

3. `.indexWhere()`:
   - Returns the index of the first matching item
   - Returns -1 if no match is found
   - Useful when you need the position in the list

4. `.any()`:
   - Returns a boolean indicating if ANY item matches the condition
   - Useful for quick existence checks

Pro Tips:
- These methods work with any type of list, not just custom classes
- They're very efficient and part of Dart's built-in collection methods
- They support complex conditions using multiple attributes or method calls

Would you like me to elaborate on any of these methods or show an example specific to your use case?