#python 



仮想環境の作り方。

```bash
$ cd project
$ python3 -m venv .venv
```

.venv という名前の仮想環境が出来た。

activate する。

```bash
$ source .venv/bin/activate
(env) prompt

$ which python3
/..../env/bin/python3
```

環境がシステムから切り離されるのか。

deactivate する。

```bash
(env)$ deactivate
$
```

