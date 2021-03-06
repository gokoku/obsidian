#flutter

---
2021-06-17

# Push → Pop

![図4:Navigatorのpush/popを使った画面遷移](https://cz-cdn.shoeisha.jp/static/images/article/14271/14271_004.png)

```js
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(home: HomePage()));
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Home"),
      ),
      body: Center(
        child: TextButton(
          child: Text("To 1 page"),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => FirstPage(),
              ),
            );
          },
        ),
      ),
    );
  }
}

class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (1)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("Back to first page."),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}
```

# Replace


![図5:NavigatorのpushReplacement/popUntilを使った画面遷移](https://cz-cdn.shoeisha.jp/static/images/article/14271/14271_005.png)

```js
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(home: HomePage()));
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Home"),
      ),
      body: Center(
        child: TextButton(
          child: Text("To 1 page"),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => FirstPage(),
              ),
            );
          },
        ),
      ),
    );
  }
}

class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (1)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("Go to Second page."),
          onPressed: () {
            Navigator.pushReplacement(
              context,
              MaterialPageRoute(
                builder: (context) => SecondPage(),
              ),
            );
          },
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (2)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("push & Replacement"),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => ThirdPage(),
              ),
            );
          },
        ),
      ),
    );
  }
}

class ThirdPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (3)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("Back to first page."),
          onPressed: () {
            Navigator.popUntil(context, (route) => route.isFirst);
          },
        ),
      ),
    );
  }
}

```

「このサンプルコードの場合には、最初の画面まで戻ることができます。任意の場所まで戻るためには、第2引数に独自の[RoutePredicat](https://api.flutter.dev/flutter/widgets/RoutePredicate.html "RoutePredicat")実装を指定する必要があります。これは少々、面倒になるので、後述するように画面名を用いた画面管理を用いる方法をおすすめします。」とのこと。

# Named Routes
```js
// （1） 指定した画面に遷移する
Navigator.pushNamed(context, '/second');
// （2） 指定した画面と置き換える
Navigator.pushReplacementNamed(context, '/second');
// （3） 指定した画面（ここでは/first）まで削除した上で、指定した画面（ここでは/second）に遷移する
Navigator.pushNamedAndRemoveUntil(context, '/second',ModalRoute.withName('/first'));
// （4） 指定した画面まで戻る
Navigator.popUntil(context, ModalRoute.withName('/first'));
```


```js
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        initialRoute: '/first',
        routes: {
          '/first': (context) => FirstPage(),
          '/second': (context) => SecondPage(),
          '/third': (context) => ThirdPage(),
        });
  }
}

class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (1)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("Go to Second page."),
          onPressed: () {
            Navigator.pushNamed(context, '/second');
          },
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (2)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("push & Replacement"),
          onPressed: () {
            Navigator.pushReplacementNamed(context, '/third');
          },
        ),
      ),
    );
  }
}

class ThirdPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Page (3)"),
      ),
      body: Center(
        child: TextButton(
          child: Text("Back to first page."),
          onPressed: () {
            Navigator.popUntil(context, ModalRoute.withName('/first'));
          },
        ),
      ),
    );
  }
}

```