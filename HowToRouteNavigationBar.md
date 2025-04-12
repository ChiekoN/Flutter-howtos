# Integrating BottomNavigationBar with go_router

Answered by Claude

## Q: How can I use BottomNavigationBar with go_router?

I'll help you create a Flutter implementation that combines BottomNavigationBar with go_router for seamless navigation.

```dart
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';

// Define route paths as constants
class AppRoutes {
  static const home = '/';
  static const explore = '/explore';
  static const profile = '/profile';
  static const settings = '/settings';
}

// Main app with GoRouter configuration
class MyApp extends StatelessWidget {
  MyApp({Key? key}) : super(key: key);

  late final GoRouter _router = GoRouter(
    routes: [
      GoRoute(
        path: AppRoutes.home,
        pageBuilder: (context, state) => NoTransitionPage(
          child: ScaffoldWithBottomNavBar(
            child: HomeScreen(),
            currentIndex: 0,
          ),
        ),
      ),
      GoRoute(
        path: AppRoutes.explore,
        pageBuilder: (context, state) => NoTransitionPage(
          child: ScaffoldWithBottomNavBar(
            child: ExploreScreen(),
            currentIndex: 1,
          ),
        ),
      ),
      GoRoute(
        path: AppRoutes.profile,
        pageBuilder: (context, state) => NoTransitionPage(
          child: ScaffoldWithBottomNavBar(
            child: ProfileScreen(),
            currentIndex: 2,
          ),
        ),
      ),
      GoRoute(
        path: AppRoutes.settings,
        pageBuilder: (context, state) => NoTransitionPage(
          child: ScaffoldWithBottomNavBar(
            child: SettingsScreen(),
            currentIndex: 3,
          ),
        ),
      ),
    ],
  );

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: _router,
    );
  }
}

// Custom Scaffold with BottomNavigationBar
class ScaffoldWithBottomNavBar extends StatelessWidget {
  final Widget child;
  final int currentIndex;

  const ScaffoldWithBottomNavBar({
    Key? key, 
    required this.child, 
    required this.currentIndex
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: child,
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: currentIndex,
        type: BottomNavigationBarType.fixed,
        onTap: (index) {
          switch (index) {
            case 0:
              context.go(AppRoutes.home);
              break;
            case 1:
              context.go(AppRoutes.explore);
              break;
            case 2:
              context.go(AppRoutes.profile);
              break;
            case 3:
              context.go(AppRoutes.settings);
              break;
          }
        },
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.explore),
            label: 'Explore',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.settings),
            label: 'Settings',
          ),
        ],
      ),
    );
  }
}

// Placeholder screen classes
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(child: Text('Home Screen'));
  }
}

class ExploreScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(child: Text('Explore Screen'));
  }
}

class ProfileScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(child: Text('Profile Screen'));
  }
}

class SettingsScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(child: Text('Settings Screen'));
  }
}

```

Here's a breakdown of the key components in this implementation:

1. Route Configuration:
   - `AppRoutes` class defines constant route paths
   - `GoRouter` is configured with routes for each screen
   - `NoTransitionPage` is used to prevent page transition animations

2. `ScaffoldWithBottomNavBar`:
   - A custom widget that wraps each screen with a `BottomNavigationBar`
   - `onTap` uses `context.go()` to navigate between routes
   - Passes the current index to highlight the active tab

3. Navigation:
   - Uses `context.go()` for programmatic navigation
   - Each route is mapped to a specific screen and bottom nav index

Key points to note:
- This approach ensures the bottom navigation bar is consistent across screens
- Uses `NoTransitionPage` for a smooth bottom navigation experience
- Easy to extend with more routes and navigation items

Make sure to add go_router to your `pubspec.yaml`:
```yaml
dependencies:
  go_router: ^latest_version
```

Would you like me to explain any part of the implementation in more detail?


## Q2: What is the difference between "builder" and "pageBuilder" that are parameters for  GoRouter()? Why this example uses "pageBuilder"?

Great question! Let me explain the difference between `builder` and `pageBuilder` in GoRouter:

1. `builder`:
   - Directly returns a `Widget`
   - Simpler, more straightforward
   - Provides a basic page with standard material page transitions
   - Best for simple navigation scenarios

2. `pageBuilder`:
   - Returns a `Page` object (like `MaterialPage`, `CupertinoPage`, `NoTransitionPage`)
   - Offers more control over page creation and transitions
   - Allows you to customize page transitions, animations, and other page-specific properties
   - Provides more flexibility in how pages are constructed and displayed

In the previous example, `pageBuilder` was used specifically to:
- Use `NoTransitionPage` to remove page transition animations
- Ensure consistent bottom navigation bar across screens
- Have more control over how pages are created and displayed

Here's a quick comparison to illustrate the difference:

```dart
// Using builder (simple approach)
GoRoute(
  path: '/home',
  builder: (context, state) => HomeScreen(), // Directly returns a Widget
)

// Using pageBuilder (more flexible approach)
GoRoute(
  path: '/home',
  pageBuilder: (context, state) => MaterialPage(
    key: state.pageKey,
    child: HomeScreen(), // Wrapped in a Page with additional configuration
    transitionsBuilder: (context, animation, secondaryAnimation, child) {
      // Custom transition can be added here
      return FadeTransition(opacity: animation, child: child);
    }
  ),
)

```

Use cases for `pageBuilder`:
- Custom page transitions
- Adding page-specific configurations
- Using different page types (Material, Cupertino, etc.)
- More complex navigation scenarios

In the BottomNavigationBar example, `pageBuilder` was crucial for:
1. Using `NoTransitionPage` to prevent jarring screen transitions
2. Maintaining the bottom navigation bar's state
3. Providing a more controlled page creation process

Would you like me to elaborate on any of these points?