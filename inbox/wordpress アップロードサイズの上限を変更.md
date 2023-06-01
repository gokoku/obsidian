---
type: note
---

#wordpress 

---
2022-12-16  10:13

# wordpress アップロードサイズの上限を変更

https://www.vektor-inc.co.jp/post/upload_max_filesize/

/etc/php.ini

```
memory_limit = 350M
post_max_size = 300M
upload_max_filesize = 300M
```
みたいな。
