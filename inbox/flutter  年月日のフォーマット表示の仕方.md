#flutter 

[https://qiita.com/KenAra/items/0dc47751af53b22731eb](https://qiita.com/KenAra/items/0dc47751af53b22731eb)

pubspec.yaml

```bash
dependencies:

  flutter_localizations:
    sdk: flutter
```

intl.dart は flutter_localizations.dart に入ってるとのこと。

```bash
import 'package:intl/intl.dart';
import 'package:intl/date_symbol_data_local.dart';

class _MyHomePageState extends State<MyHomePage> {
  DateTime _value = DateTime.now();

  String getDate() {
    initializeDateFormatting("ja");
    return DateFormat.yMMMd('ja').format(_value).toString();
  }
```

2021年2月5日　みたいな表示になる。

---

文字列から
