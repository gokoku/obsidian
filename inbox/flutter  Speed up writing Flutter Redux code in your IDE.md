#flutter #js/redux

---
2021-07-06

# Speed up writing Flutter Redux code in your IDE

https://pszklarska.medium.com/speed-up-writing-flutter-redux-code-in-your-ide-e29a355684ad

Flutter application で Redux を使うことはかなりアドバンテージあるらしい。

vscode のスニペットで爆速に Redux が書ける?!


In the window that’ll appear, choose dart. That should open the editor for your snippet. Paste the following code into that editor and save it:


```json
{
  "Create new Page with Redux template": {
    "prefix": "redux",
    "body": [
      "import 'package:flutter/material.dart';",
      "import 'package:flutter_redux/flutter_redux.dart';",
      "import 'package:redux/redux.dart';",
      "class ${1:name}Page extends StatelessWidget {",
      "  @override",
      "  Widget build(BuildContext context) {",
      "    return StoreConnector<$0AppState, ${1:name}ViewModel>(",
      "      converter: (store) => ${1:name}ViewModel(store),",
      "      builder: (context, viewModel) => _${1:name}Page(viewModel),",
      "    );",
      "  }",
      "}",
      "",
      "class _${1:name}Page extends StatelessWidget {",
      "  final ${1:name}ViewModel viewModel;",
      "  _${1:name}Page(this.viewModel);",
      "  @override",
      "  Widget build(BuildContext context) {",
      "    return Container();",
      "  }",
      "}",
      "",
      "@immutable",
      "class ${1:name}ViewModel {",
      "  ${1:name}ViewModel(this._store);",
      "  final Store<AppState> _store;",
      "}"
    ],
    "description": "Create new Page with Redux template"
  }
}
```

このスニペットで dart file で redux をタイピングすると、いい感じらしい。

![](https://miro.medium.com/max/700/1*LLHDGpazI_X7laa4FMLESQ.png)

