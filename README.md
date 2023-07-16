# 앱개발 종합반

## Instal Flutter
```shell
flutter doctor
```
- `Flutter: New Project` -> `Application`
- `Flutter: Launch Emulator` -> `Start iOS Simulator` -> `Run Without Debugging`

## StatelessWidget
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar:
        body:
      )
    )
  }
}
```

## Refactor
(Right Click) -> `Refactor...` -> `Extract Widget`

## Nagivation
```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondPage()),
);
```
```dart
Navigator. pop(context);
```

## ListView
```dart
ListView.builder(
  itemCount: 100,
  itemBuilder: (context, index) {
    return Text("$index");
  }
)
```

## StatefulWidget

## Provider

## shared_preferences

## async
