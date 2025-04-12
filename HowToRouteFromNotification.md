# Q: Can you show me an example code of a local notification in Flutter, in which a user can go to a specific page in an app from a link shown in the notification?

## A: Claude answered:

I'll create a comprehensive Flutter example demonstrating local notifications with deep linking to a specific page.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';
import 'package:go_router/go_router.dart';

class NotificationService {
  static final FlutterLocalNotificationsPlugin flutterLocalNotificationsPlugin =
      FlutterLocalNotificationsPlugin();

  // Initialize notification settings
  static Future<void> initialize() async {
    // Android notification details
    const AndroidInitializationSettings initializationSettingsAndroid =
        AndroidInitializationSettings('@mipmap/ic_launcher');

    // iOS notification settings
    const DarwinInitializationSettings initializationSettingsIOS =
        DarwinInitializationSettings(
      requestSoundPermission: false,
      requestBadgePermission: false,
      requestAlertPermission: false,
    );

    // Combined initialization settings
    const InitializationSettings initializationSettings = InitializationSettings(
      android: initializationSettingsAndroid,
      iOS: initializationSettingsIOS,
    );

    // Initialize the plugin
    await flutterLocalNotificationsPlugin.initialize(
      initializationSettings,
      onDidReceiveNotificationResponse: _onNotificationTap,
    );
  }

  // Handle notification tap
  // About below pragma annotation, see https://github.com/dart-lang/sdk/blob/master/runtime/docs/compiler/aot/entry_point_pragma.md
  @pragma('vm:entry-point')
  static void _onNotificationTap(NotificationResponse notificationResponse) {
    // Assuming you're using GoRouter for navigation
    // The payload can contain the route you want to navigate to
    final String? payload = notificationResponse.payload;
    if (payload != null) {
      // Navigate to the specific route when notification is tapped
      GoRouter.of(navigatorKey.currentContext!).push(payload);
    }
  }

  // Method to show a local notification
  static Future<void> showNotification({
    required int id,
    required String title,
    required String body,
    String? payload, // Route or additional data
  }) async {
    // Android notification details
    const AndroidNotificationDetails androidPlatformChannelSpecifics =
        AndroidNotificationDetails(
      'your_channel_id',
      'your_channel_name',
      importance: Importance.high,
      priority: Priority.high,
    );

    // iOS notification details
    const DarwinNotificationDetails iOSPlatformChannelSpecifics =
        DarwinNotificationDetails();

    // Combined notification details
    const NotificationDetails platformChannelSpecifics = NotificationDetails(
      android: androidPlatformChannelSpecifics,
      iOS: iOSPlatformChannelSpecifics,
    );

    // Show the notification
    await flutterLocalNotificationsPlugin.show(
      id,
      title,
      body,
      platformChannelSpecifics,
      payload: payload, // This will be used for navigation
    );
  }
}

// Example of how to use the notification service
class NotificationExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Notification Demo')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Show a notification that will navigate to the profile page
            NotificationService.showNotification(
              id: 1,
              title: 'Hey there!',
              body: 'Check out your profile',
              payload: '/profile', // The route to navigate to
            );
          },
          child: Text('Send Notification'),
        ),
      ),
    );
  }
}

// Global navigator key for routing
final GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

// Example router configuration
final GoRouter router = GoRouter(
  navigatorKey: navigatorKey,
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => HomeScreen(),
    ),
    GoRoute(
      path: '/profile',
      builder: (context, state) => ProfileScreen(),
    ),
  ],
);

// Sample screens
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(child: Text('Home Screen')),
    );
  }
}

class ProfileScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Profile')),
      body: Center(child: Text('Profile Screen')),
    );
  }
}

// Main app setup
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  void initState() {
    super.initState();
    // Initialize notification service
    NotificationService.initialize();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: router,
    );
  }
}

```

This example demonstrates a comprehensive approach to local notifications in Flutter with deep linking. Let me break down the key components:

1. `NotificationService` class:
   - Handles initialization of local notifications
   - Provides a method to show notifications
   - Implements navigation when a notification is tapped

2. Navigation Setup:
   - Uses `go_router` for handling navigation
   - Passes a route as a payload when creating a notification
   - When the notification is tapped, it navigates to the specified route

3. Key Dependencies:
   - `flutter_local_notifications`: For creating and managing local notifications
   - `go_router`: For handling app navigation

To use this in your project, you'll need to:
1. Add dependencies to your `pubspec.yaml`:
   ```yaml
   dependencies:
     flutter_local_notifications: ^latest_version
     go_router: ^latest_version
   ```
2. Configure notification permissions in your Android and iOS native projects
3. Call `NotificationService.initialize()` in your app's startup process

In the example, tapping the button creates a notification that, when tapped, will navigate to the profile screen. The payload (`'/profile'`) determines the destination.

Would you like me to elaborate on any part of the implementation?


# Q: Thanks! Is it possible to put a button in the notification so the user is navigated to the page when they press that button?

## A: Claude answered:

I'll modify the previous example to include an action button in the notification.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';
import 'package:go_router/go_router.dart';

class NotificationService {
  static final FlutterLocalNotificationsPlugin flutterLocalNotificationsPlugin =
      FlutterLocalNotificationsPlugin();

  // Initialize notification settings
  static Future<void> initialize() async {
    // Android notification details with action
    const AndroidInitializationSettings initializationSettingsAndroid =
        AndroidInitializationSettings('@mipmap/ic_launcher');

    // iOS notification settings
    const DarwinInitializationSettings initializationSettingsIOS =
        DarwinInitializationSettings(
      requestSoundPermission: false,
      requestBadgePermission: false,
      requestAlertPermission: false,
    );

    // Combined initialization settings
    const InitializationSettings initializationSettings = InitializationSettings(
      android: initializationSettingsAndroid,
      iOS: initializationSettingsIOS,
    );

    // Initialize the plugin with action handling
    await flutterLocalNotificationsPlugin.initialize(
      initializationSettings,
      onDidReceiveNotificationResponse: _handleNotificationResponse,
    );
  }

  // Handle notification response (tap or action button)
  static void _handleNotificationResponse(NotificationResponse notificationResponse) {
    final String? payload = notificationResponse.payload;

    // Check the type of interaction
    switch (notificationResponse.notificationResponseType) {
      case NotificationResponseType.selectedNotification:
        // Notification body was tapped
        _navigateToRoute(payload);
        break;
      case NotificationResponseType.changedNotificationActionButton:
        // Action button was pressed
        _navigateToRoute(payload);
        break;
    }
  }

  // Helper method to navigate to a specific route
  static void _navigateToRoute(String? payload) {
    if (payload != null && navigatorKey.currentContext != null) {
      GoRouter.of(navigatorKey.currentContext!).push(payload);
    }
  }

  // Method to show a local notification with an action button
  static Future<void> showNotificationWithAction({
    required int id,
    required String title,
    required String body,
    String? payload, // Route or additional data
  }) async {
    // Android notification details with action button
    final AndroidNotificationDetails androidPlatformChannelSpecifics =
        AndroidNotificationDetails(
      'your_channel_id',
      'your_channel_name',
      importance: Importance.high,
      priority: Priority.high,
      // Add an action button
      actions: <AndroidNotificationAction>[
        AndroidNotificationAction(
          'profile_action', // Unique identifier for the action
          'View Profile', // Button text
          icon: AndroidResourceIcon('@drawable/ic_profile'), // Optional icon
          // You can add a custom input if needed
          // inputs: <AndroidNotificationActionInput>[
          //   AndroidNotificationActionInput(
          //     label: 'Enter a comment',
          //   ),
          // ],
        ),
      ],
    );

    // iOS notification details
    const DarwinNotificationDetails iOSPlatformChannelSpecifics =
        DarwinNotificationDetails(
      // iOS has different ways of handling action buttons
      // You might need to configure these differently
      categoryIdentifier: 'profile_category',
    );

    // Combined notification details
    final NotificationDetails platformChannelSpecifics = NotificationDetails(
      android: androidPlatformChannelSpecifics,
      iOS: iOSPlatformChannelSpecifics,
    );

    // Show the notification
    await flutterLocalNotificationsPlugin.show(
      id,
      title,
      body,
      platformChannelSpecifics,
      payload: payload, // This will be used for navigation
    );
  }
}

// The rest of the code remains the same as in the previous example
// (HomeScreen, ProfileScreen, router setup, etc.)

// Example usage in a widget
class NotificationExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Notification Demo')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Show a notification with an action button
            NotificationService.showNotificationWithAction(
              id: 1,
              title: 'Profile Update',
              body: 'You have a new profile notification',
              payload: '/profile', // The route to navigate to when action is pressed
            );
          },
          child: Text('Send Notification with Action'),
        ),
      ),
    );
  }
}

```

Key changes in this updated version:

1. Added `showNotificationWithAction` method that creates a notification with an action button
2. Implemented `_handleNotificationResponse` to handle both notification body taps and action button presses
3. Added Android-specific action button configuration
   - `AndroidNotificationAction` allows creating buttons with custom text and optional icons
4. Included a basic handling for iOS notifications (note that iOS action button handling can be more complex)

A few important notes:
- The action button will navigate to the same route specified in the payload
- For iOS, action button handling is more complex and might require additional configuration
- You'll need to add appropriate icons (like `@drawable/ic_profile`) in your Android project resources

Potential enhancements you might consider:
- Add different actions for different buttons
- Implement custom input fields in notification actions
- Add more specific iOS notification action handling

Would you like me to elaborate on any part of the implementation or discuss platform-specific nuances?
