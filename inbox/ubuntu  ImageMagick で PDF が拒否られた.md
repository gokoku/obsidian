#ubuntu

---
2022-03-23  11:32

# ubuntu  ImageMagick で PDF が拒否られた

rails で carrier-wave がエラーで PDF が処理できなかった。

サムネールを作る処理でエラーになってるようだった。

```shell
convert-im6.q16: attempt to perform an operation not allowed by the security policy `PDF' @ error/constitute.c/IsCoderAuthorized/408.
```

### 回避

/etc/ImageMagick-6/policy.yml

```yml
  <!-- <policy domain="coder" rights="none" pattern="PDF" /> -->
```

