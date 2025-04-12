# Flutter

## 1. Dart language

## 2. Set up Flutter environment

https://codelabs.developers.google.com/codelabs/flutter-codelab-first#1

https://docs.flutter.dev/

1.  Choose a dev target
 
> But you could also choose Windows as the development target, which means your app-in-development runs as a Windows app alongside your editor.

2. Install SDK

https://docs.flutter.dev/get-started/install/windows/desktop#use-vs-code-to-install-flutter


### Start building Flutter native desktop apps on Windows

https://docs.flutter.dev/get-started/install/windows/desktop

- [Install Visual Studio 2022](https://visualstudio.microsoft.com/downloads/?cid=learn-onpage-download-install-visual-studio-page-cta) (with **Desktop development with C++**)
    
    (I also installed Mobile dev with C++ in case) 

- Install the Flutter SDK

Other requirements:
- Operating system : WIndows PowerShell 5 or later
- Git for Windows 2.27
- VSCode 2022


## 3. Create a project

https://codelabs.developers.google.com/codelabs/flutter-codelab-first#2

- `pubspec.yaml`:The pubspec.yaml file specifies basic information about your app, such as its current version, its dependencies, and the assets with which it will ship.

- `analysis_options.yaml` : This file determines how strict Flutter should be when analyzing your code. Since this is your first foray into Flutter, you're telling the analyzer to take it easy. You can always tune this later. In fact, as you get closer to publishing an actual production app, you will almost certainly want to make the analyzer stricter than this.

- `main.dart` : main

### Create a new project in CLI

Go to a directory for the project or for development,
```
> flutter create <app_name>
```



## About Dart language

- https://dart.dev/language


## Reference

### Colors

- Colors class : about colour palette
    https://api.flutter.dev/flutter/material/Colors-class.html

### Widgets

- Basic widgets :　https://docs.flutter.dev/ui/widgets/basics


### Date picker

- showDatePicker function: Shows a dialog containing a Material Design date picker.
    -  https://api.flutter.dev/flutter/material/showDatePicker.html

- showDateRangePicker: which shows a Material Design date range picker used to select a range of dates.
    - https://api.flutter.dev/flutter/material/showDateRangePicker.html

- CalendarDatePicker: which provides the calendar grid used by the date picker dialog.
    - https://api.flutter.dev/flutter/material/CalendarDatePicker-class.html

- A Deep Dive Into DatePicker In Flutter | medium
    - https://medium.com/flutter-community/a-deep-dive-into-datepicker-in-flutter-37e84f7d8d6c

### Text input field

- TextField
    - https://api.flutter.dev/flutter/material/TextField-class.html

- TextFormField
    - https://api.flutter.dev/flutter/material/TextFormField-class.html

- Build a form with validation
    - https://docs.flutter.dev/cookbook/forms/validation

- IputDecoration
    - https://api.flutter.dev/flutter/material/InputDecoration-class.html

- Handle changes to a text field
    - https://docs.flutter.dev/cookbook/forms/text-field-changes

### Text format

- TextTheme class
    - https://api.flutter.dev/flutter/material/TextTheme-class.html

### Handling multiple screens

- Navigate to a new screen and back
    - https://docs.flutter.dev/cookbook/navigation/navigation-basics


### User input variations

- https://docs.flutter.dev/get-started/fundamentals/user-input


### ListView and ListView.builder

- https://docs.flutter.dev/cookbook/lists/long-lists


### Store data locally by using Shared Preference

Store Key-Value pair in a specific place in the machine.
Supported data types are `int`, `double`, `bool`, `String` and `List<String>`.


- https://docs.flutter.dev/cookbook/persistence/key-value

- https://stackoverflow.com/questions/41369633/how-to-save-to-local-storage-using-flutter

- https://stackoverflow.com/questions/41369633/how-to-save-to-local-storage-using-flutter
  
  - We highly encourage any new users of the plugin to use the newer [SharedPreferencesAsync] or [SharedPreferencesWithCache] APIs instead.

  - SharedPreferences is the best option to store a small amount of data in flutter projects.

  To use `shared_preferences`,
  ```
   > flutter pub add shared_preferences
  ```
  Also turn the Windows to Developer Mode
  ```
   > start ms-settings:developers
  ```

### Store data in a local file

- https://docs.flutter.dev/cookbook/persistence/reading-writing-files

- https://pub.dev/packages/path_provider

- File class:
  - https://api.flutter.dev/flutter/dart-io/File-class.html


### Create JSON String from an object

- https://flutterexperts.com/convert-object-to-json-in-dart-flutter/

- https://docs.flutter.dev/data-and-backend/serialization/json


### Save a Datetime object to a file as text
```
Future<void> _saveDateTime() async {
    final file = await _getLocalFile();
    final now = DateTime.now();
    await file.writeAsString(now.toIso8601String());
    setState(() {
      _savedDateTime = now.toIso8601String();
    });
  }

  Future<void> _readDateTime() async {
    try {
      final file = await _getLocalFile();
      final contents = await file.readAsString();
      setState(() {
        _savedDateTime = contents;
      });
    } catch (e) {
      setState(() {
        _savedDateTime = 'Error reading date';
      });
    }
  
```


### Android emulator for windows

- https://docs.flutter.dev/get-started/install/windows/mobile

- Android Studio
  - https://developer.android.com/studio


**NOTE**: I couldn't install Android SDK Command-line Tools. So I manually installed this.

 1. Download .zip from "Command line tools only" section in:
   - https://developer.android.com/studio

 2. Unzip

 3. Place the unziped `cmdline-tools` folder under Android SDK location:

  - `C:\Users\chiek\AppData\Local\Android\Sdk`
  - I can check the SDK location from Android Studio
    - More Action > SDK Manager
    - Languages & Frameworks > Android SDK > Android SDK Location:

 4. Follow the instraction:
  - https://developer.android.com/tools/sdkmanager

#### NOTE: I had an issue when I first launched the app on Android emulator
```
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':connectivity_plus:compileReleaseJavaWithJavac'.
> Could not resolve all files for configuration ':connectivity_plus:androidJdkImage'.
   > Failed to transform core-for-system-modules.jar to match attributes {artifactType=_internal_android_jdk_image, org.gradle.libraryelements=jar, org.gradle.usage=java-runtime}.
      > Execution failed for JdkImageTransform: /home/miquel/Android/Sdk/platforms/android-34/core-for-system-modules.jar.
         > Error while executing process /home/miquel/.local/share/JetBrains/Toolbox/apps/android-studio/jbr/bin/jlink with arguments {--module-path /home/miquel/.gradle/caches/transforms-3/4a46fc89ed5f9adfe3afebf74eb8bfeb/transformed/output/temp/jmod --add-modules java.base --output /home/miquel/.gradle/caches/transforms-3/4a46fc89ed5f9adfe3afebf74eb8bfeb/transformed/output/jdkImage --disable-plugin system-modules}
```

I followed the following instraction, and it helped:
 - [How to fix Android Compilation issues #3303](https://github.com/fluttercommunity/plus_plugins/issues/3303)


#### NOTE2: The saved file wasn't loaded properly at first on Android emulator

Because the date file **NOT FOUND** when the app relaunched secound time and after. It seemed the path for `getApplicationDocumentsDirectory()` couldn't properly identified. It is `/data/user/0/com.example.my_second_app/app_flutter` but perhaps it wasn't created.

So I first had to create it by setting up Android Studio.

1. Launch Android Studio.
2. Open this project in Android Studio.
3. Then select an Android device (e.g. *Medium Phone API 35 2(mobile)* which I created)  
4. Run
5. Choose the device I chose in 3 in Device Explore.
6. In *Files* in Device Explorer, I can locate the target file:
    ```
    /data/data/com.example.my_second_app/app_flutter/CDStorage.txt
    ```
7. Then stop the app in Android Studio. 

After this, I can launch the app from VScode. Reading and writing the file works next time.


### How to launch Adroid Emulator on VSCode

1. Ctrl + Shift + P -> launch emulator -> select a device
2. and then run debugger

OR

1. Ctrl + Shift + P -> Select Device -> and select a device
2. and run debugger
It automatically launch a emulator.

For the first time in connecting the emulator, I have to execute Android Studio and open the Flutter project root directory:
 - `Flutter/my_second_app/my_second_app/`


### How to add Android icons

- [Create app icons](https://developer.android.com/studio/write/create-app-icons)

**NOTE:** If I can't find Asset Studio, 
1. Open `my_second_app\my_second_app\android` directly in Android studio.
2. It starts installing some gradles and then I can use Asset Studio on `res/`.

reference: https://github.com/flutter/flutter-intellij/issues/3512







# Copilot's example
**Create a bottom navigation bar with two pages.**

```
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Simple Flutter App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int _selectedIndex = 0;
  final List<Widget> _pages = [HomeScreen(), FormScreen()];

  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  void _navigateToFormPage() {
    Navigator.push(
      context,
      MaterialPageRoute(builder: (context) => FormScreen()),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home Page'),
        actions: <Widget>[
          IconButton(
            icon: Icon(Icons.menu),
            onPressed: _navigateToFormPage,
          ),
        ],
      ),
      body: _pages[_selectedIndex],
      bottomNavigationBar: BottomNavigationBar(
        items: const <BottomNavigationBarItem>[
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.note_add),
            label: 'Form',
          ),
        ],
        currentIndex: _selectedIndex,
        selectedItemColor: Colors.blue,
        onTap: _onItemTapped,
      ),
    );
  }
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text('Home Screen'),
    );
  }
}

class FormScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Form Page'),
      ),
      body: Center(
        child: Text('Form Screen'),
      ),
    );
  }
}
```