#command 

---
2021-12-21

# Tesseract で OCR

https://www.teamxeppet.com/tesseract-intro/


```shell
$ brew install tesseract
```

他の言語もインストール

```shell
$ brew install tesseract-lang

$ tesseract --list-langs | grep 'jpn'  # 日本語があるか確認する
jpn
jpn_vert
```

jpnz_vert は縦書き日本語とのこと。vertical ?


![[test.png]]

これを日本語と英語で読み取らせます。

```shell
$ tesseract test.png out -l ipn+eng

Tesseract Open Source OCR Engine v4.1.3 with Leptonica
Warning: Invalid resolution 0 dpi. Using 70 instead.
Estimating resolution as 180
❯ cat out.txt
tesseractコマンドの構文は以下となります。

tesseract imagename outputbase [options.…] [configfil



画像を用意してTesseractコマンドで画像を読み込んでみましょう。

tesseract test.png out



imagenameで画像ファイルを指定し、outputbaseに出力するテキストファイル名を入力し
+t.
```

凄すぎ!!

縦書きに対応してみる。

![[test 1.png]]

```shell
❯ tesseract test.png out -l jpn+jpn_vert
Tesseract Open Source OCR Engine v4.1.3 with Leptonica
Warning: Invalid resolution 0 dpi. Using 70 instead.
Estimating resolution as 297
❯ cat out.txt
縦書きをしてみました
左には横書きのテキスト

横書きをしてみました
右には縦書きのテキスト
```

すげ〜!!!!!!

# Qiita
https://qiita.com/tags/tesseract-ocr

