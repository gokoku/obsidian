#js/vue #github 


# Nuxt アプリ

.github/workflows/gh-pages.yml を作る。

```yaml
name: github pages

on:
  push:
    branches:
      - master
jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1

    - name: setup node
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'

    - name: install
      run: yarn install

    - name: test
      run: yarn test

    - name: generate
      run: yarn generate

    - name: deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

これを push すると自動的に Action が動く仕組み。

このままでは vue は動かない。
GitHub Page ではコンテンツ URL がルートでないゆえ。

nuxt.config.js

```js
  router: {
    base: "/my-memo/",
  },
```

リポジトリ名をベースにすること。
<https://ja.nuxtjs.org/faq/github-pages/>

# GitHub サイト

Github リポジトリの setting で下にスクロールすると

Action が成功したら、

Branch を新たにつくられた gh-pages にする。

publish された。

 Your site is published at <https://gokoku.github.io/my-memo/>
