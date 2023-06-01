#ruby #vscode

---
2021-11-25

# Rubocop の VScode エラー

![[Pasted image 20211125165502.png]]

```shell
$ gem install rubocop
```

これだとエラーが出る。

```shell
$ gem install rubocop-rails
```

出る。

rails で .rubocop.yml の中に、

```yaml
require:
  - rubocop-performance
  - rubocop-rails

```

追加。

```shell
$ gem install rubocop-performance
```

![[Pasted image 20211125170341.png]]

.rubycop.yml

```yaml
Metrics/MethodLength:
  Max: 40 # default: 10
```

これを言われたように、

```yaml
Layout/MethodLength:
  Max: 40 # default: 10
```
