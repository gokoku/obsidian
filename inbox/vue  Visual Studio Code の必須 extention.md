#js/vue  #vscode 

---
2021-06-29

# VSCode の必須 extention と設定

![[Pasted image 20210629151412.png]]

いろいろ一式。

![[Pasted image 20210629151728.png]]

Vue Peek 

使われているコンポーネントのファイルまでジャンプするようだ。

いろんな Peek がある。スゲーかも。

## 設定ファイルの設置

場所 : project/.vscode/settings.json    

settings だよ!! setting.json で頭??って捻ってたし!

```json
{
  // ESLint との重複を避けるため、既定の検証を無効にする
  "javascript.validate.enable": false,
  "html.validate.scripts": false,
  "html.validate.styles": false,

  // ESLint の codeActionsOnSave との競合を避ける
  "editor.formatOnSave": false,

  // Formatter を ESLint（Prettier） にする
  "editor.defaultFormatter": "dbaeumer.vscode-eslint",
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  // Vetur の設定
  "vetur.experimental.templateInterpolationService": true,
  "vetur.validation.script": false,
  "vetur.validation.style": false,
  "vetur.validation.template": false,

  // ESLint の設定
  "editor.codeActionsOnSaveTimeout": 2000,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.fixAll.stylelint": true,
  },
  "eslint.format.enable": true,

  // Code Spell Checker の設定
  "cSpell.words": ["vuex"],
  "cSpell.enableFiletypes": [
    "css",
    "html",
    "javascript",
    "json",
    "markdown",
    "scss",
    "text",
    "vue"
  ],
  "cSpell.languageSettings": [
    {
      "languageId": "vue",
      "dictionaries": ["html", "fonts", "typescript", "node", "css"]
    }
  ],
}
```

## prettier/prettier の設定をいじる

この設定だと、prettier/prettier 一択の設定になってしまうとのこと。

https://qiita.com/ikngtty/items/4df2e13d2fa1c4c47528

どうしても、; を取りたいし、' にしたい。こうすればいいとのこと。

.eslintrc.js
```json
  rules: {
    ..
	  
    'prettier/prettier': ['error', { singleQuote: true, semi: false }],
  },
```

これで望む動きになった。
