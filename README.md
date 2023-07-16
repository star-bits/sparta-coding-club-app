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
        appBar: ...
        body: ...
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
    return Row(...)
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
    return Row(...)
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
- HomePage에서 FloatingActionButton으로 새 메모를 만드는 경우 빈 스트링을 만들고 setState로 hot reload를 해줬음.
- DetailPage에서 발생한 변화를 HomePage에 반영하는 경우 State Management를 대신해주는 Provider 패키지를 사용함.

Provider 적용 전:
```dart
void main() {
  runApp(const MyApp());
}
```

Provider 적용 후:
```dart
import 'package:provider/provider.dart';
import 'memo_service.dart';

void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (context) => MemoService()),
      ],
      child: const MyApp(),
    ),
  );
}
```

`Consumer`로 `MemoService`에 접근하기:
```dart
class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Consumer<MemoService>(
      builder: (context, memoService, child) {
        List<Memo> memoList = memoService.memoList;

        return Scaffold(
          appBar: ...
          body: ...
        )
      }
    )
  }
}
```

`memo_service.dart`의 구조:
```dart
import 'main.dart';

class Memo {
  Memo({
    required this.content,
  });

  String content;
}

class MemoService extends ChangeNotifier {
  List<Memo> memoList = [
    Memo(content: '메모 예시'),
  ];
}
```

`context.read`로 `MemoService`에 접근하기:
```dart
class DetailPage extends StatelessWidget {
  DetailPage({super.key, required this.index});

  final int index;

  TextEditingController contentController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    MemoService memoService = context.read<MemoService>();
    Memo memo = memoService.memoList[index];
    contentController.text = memo.content;

    return Scaffold(
      appBar: ...
      body: ...
    )
  }
}
```

`notifyListeners()`로 `Consumer<MemoService>`의 `builder`함수 호출해 화면 갱신하기:
```dart
floatingActionButton: FloatingActionButton(
  child: Icon(Icons.add),
  onPressed: () {
    memoService.createMemo(content: ''); // createMemo 안에 notifyListeners()가 있음
      Navigator.push(
        context,
        MaterialPageRoute(
          builder: (_) => DetailPage(
            index: memoService.memoList.length - 1,
          ),
        ),
      );
    }
  },
)
```

`memo_service.dart` 속 `notifyListeners()`:
```dart
class MemoService extends ChangeNotifier {
  List<Memo> memoList = [
    Memo(content: '메모 예시'),
  ];

  createMemo({required String content}) {
    Memo memo = Memo(content: content, updatedAt: DateTime.now());
    memoList.add(memo);
    notifyListeners();
  }
}
```

## SharedPreferences
SharedPreferences 적용 전:
```dart
void main() {
  runApp(...)
}
```

SharedPreferences 적용 후:
```dart
late SharedPreferences prefs;

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  prefs = await SharedPreferences.getInstance();
  runApp(...)
}
```

## async
