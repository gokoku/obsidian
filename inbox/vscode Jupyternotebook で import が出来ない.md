---
type: note
---

#vscode #jupyter_notebook 

---
2023-02-21  11:04

# vscode Jupyternotebook で import が出来ない

![[Pasted image 20230221110505.png]]

コンソールからパスを調べられる。
```python
$ python3
>>> print(numpy.__file__)
/Users/george/.asdf/installs/python/anaconda3-2022.05/lib/python3.9/site-packages/numpy/__init__.py
```

anaconda3 だった。anaconda3 の環境とかもよくわかってないんだけどなー。

python3.10.5 にした。
```shell
/Users/george/.asdf/installs/python/3.10.5/lib/python3.10/site-packages/numpy/__init__.py
```
https://startlab.jp/learning-python/vscode-settings/

![[Pasted image 20230221113341.png]]

ここで Add Item でこのパスを入れる。

![[Pasted image 20230221113441.png]]

なんか入れなくても、.jpynb ファイル突けば python 環境を選ばせてくれた。

![[Pasted image 20230221114804.png]]


けど、なんかsystem のpythonだ。

jupyter : filter karnel  でチェック外してやればいいのかな。
よくわからん、change karnel で指定して .asdf の python がなんとかなったぞ。

よくわからんが、これで、OK だ。

Math for Programmers の jupyter コードが .asdf python 3.10 で動くようになった。

![[Pasted image 20230221115630.png]]

![[Pasted image 20230221115426.png]]




