#vscode #js

---
2021-06-29

# 保存時にガッと整形してほしい

prettier save で search かけて

![[Pasted image 20210629161541.png]]

Editor: Format On Save にチェックを入れる。

# 行末に;無し。文字列はシングルクォート

![[Pasted image 20210629162029.png]]

逆にかけてた。

### eslint で ; 無いの注意されてる

eslint-config-prettier を使ってたりしてると、prettier/prettier とやらが既に設定されているとのこと。

.eslintrc.js
```json
  rules: {

    ...
	  
    'prettier/prettier': ['error', { singleQuote: true, trailingComma: 'all', semi: false }],
  },
```

これで上書き出来る。

[[vue  Visual Studio Code の必須 extention]]