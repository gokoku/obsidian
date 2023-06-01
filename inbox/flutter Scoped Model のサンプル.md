#flutter 


pubspec.yaml
```yaml
dependencies:
  scoped_model: ^1.0.1
  
```

lib/model/CounterModel.dart

```dart
import 'package:scoped_model/scoped_model.dart';

class CounterModel extends Model {
  int _counter = 0;
  
  int get counter => _counter;
  
  void increment() {
    _counter++;
    notifyListeners();
  }
}
```

model を使うところで使えるように渡すのか。

ScopedModel で包んでやる。

ScopeModelDescendant を介して get したり、 increment したりすると、
Model と全部同期すると。


lib/main.dart
```dart
import 'dart:developer';

import 'package:flutter/material.dart';
import 'package:scoped_model/scoped_model.dart';
import './model/CounterModel.dart';


void main() {
  runApp(MyApp(CounterModel()));
}

class MyApp extends StatelessWidget {
  final CounterModel counterModel;
  
  MyApp(this.counterModel);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: ScopedModel<CounterModel>(
        model: counterModel,
        child: MyHomePae(title: 'Flutter Demo Home Page'),
      ),
    );
  }
}

class MyHomePage extends StatelessWidget {
  final String title;
  
  MyHomePage({Key key, this.title}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: new Text(title)),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text('You hav pushes the button this many times:'),
            ScopedModelDescendant<CounterModel>(
              builder: (context, child, model) => Text('${model.counter}',
              style: Theme.of(context).textTheme.headline1),
            ),
          ],
        ),
      ),
      floatingActionButton: 
        ScopedModelDescendant<CounterModel>(
          builder: (context, child, model) => FloatingActionButton(
              onPressed: () => model.increment(),
              tooltip: 'Increment',
              child: Icon(Icons.add),
            );
          }
        ),
    );
  }
}
  
```

