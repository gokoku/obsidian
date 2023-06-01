---
type: note
---

#blender

---
2022-10-05  22:34

# blender.  Python スクリプトの標準出力

Mac はターミナルからの起動で標準出力が見られるようになるとのこと。

brew で入れてるから

```shell
$ blender
```

これでOKだ。

```pyrhon
import sys

print(sys.copyright)
```


```shell
Copyright (c) 2001-2022 Python Software Foundation.
All Rights Reserved.

Copyright (c) 2000 BeOpen.com.
All Rights Reserved.

Copyright (c) 1995-2001 Corporation for National Research Initiatives.
All Rights Reserved.

Copyright (c) 1991-1995 Stichting Mathematisch Centrum, Amsterdam.
All Rights Reserved.
Saved session recovery to '/var/folders/nb/8p29yz6s5wqgg9h3q2yxlc1m0000gn/T/quit.blend'
```

バッチリ!!