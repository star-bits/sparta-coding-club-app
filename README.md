# 앱개발 종합반

## Install Flutter
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
Stateless:
```dart
class Feed extends StatelessWidget {
  const Feed({
    Key? key,
  }) : super(key:key);

  @override
  Widget build(BuildContext context) {
    return Row()
  }
}
```
Stateful:
```dart
class Feed extends StatefulWidget {
  const Feed({
    Key? key,
  }) : super(key:key);

  @override
  State<Feed> createState() => _FeedState();
}

class _FeedState extends State<Feed> {
  @override
  Widget build(BuildContext context) { // build가 State 안에 있음
    return Row()
  }
}
```
`setState()` = Hot Reload (on HomePage)
```dart
...
return Scaffold(
  appBar:
  body:
  floatingActionButton: FloatingActionButton(
    onPressed: () {
      String memo = ''
      setState(() {
        memoList.add(memo);
      });
      Navigator.push(
        context,
        MaterialPageRoute(
          builder: (_) => DetailPage(
            index: memoList.indexOf(memo),
            memoList: memoList,
          ),
        ),
      );
    },
  ),
);
            
```
## Provider
- HomePage에서 새로 메모를 만드는 경우 빈 스트링을 만들고 setState로 hot reload를 해줬음.
- DetailPage에서 발생한 변화를 HomePage에 반영하는 경우 State Management를 대신해주는 Provider 패키지를 사용함.


## shared_preferences

## async
