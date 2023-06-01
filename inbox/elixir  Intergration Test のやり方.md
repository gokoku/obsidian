#elixir 


[https://qiita.com/blendthink/items/96e3948e9f1d0d86b1f1](https://qiita.com/blendthink/items/96e3948e9f1d0d86b1f1)

## dependency flutter_driver を追加する

```bash
./pubspec.yaml

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_driver:
    sdk: flutter
  test: any
```

## テストファイルを作る

ドライバー？

```bash
./test_driver/app.dart

import 'package:flutter_driver/driver_extension.dart';
import 'package:my_calc/main.dart' as app;

void main() {
  enableFlutterDriverExtension();

  app.main();
}
```

```bash
./test_driver/app_test.dart

import 'package:flutter_driver/flutter_driver.dart';
import 'package:test/test.dart';

/*
  $ flutter driver --target=test_driver/app.dart
*/

void main() {
  FlutterDriver driver;

  setUpAll(() async => driver = await FlutterDriver.connect());

  tearDownAll(() async => {if (driver != null) driver.close()});

  test(
    'input 33+3= then result 36',
    () async => {
      await driver.tap(find.byValueKey('3')),
      await driver.tap(find.byValueKey('3')),
      await driver.tap(find.byValueKey('+')),
      await driver.tap(find.byValueKey('3')),
      await driver.tap(find.byValueKey('=')),
      expect(await driver.getText(find.byValueKey('result')), "36")
    },
  );
  test(...

```

テストの実行

```bash
$ flutter driver --target=test_driver/app.dart
```

test 一つ一つが tear down するので、独立してできる。

