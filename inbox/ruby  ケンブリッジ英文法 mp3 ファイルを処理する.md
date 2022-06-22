#ruby #english #command 

---
2021-05-28

# マーフィーのケンブリッジ英文法音声ファイルをまとめる

```
cambridge ──┬──Unit 1──┬──BGU4_001A_01.mp3
            |          ├──BGU4_001B_01.mp3
		|	     .....
            ├──Unit 2──┬──BGU4_001A_01.mp3
            |          ├──BGU4_001B_01.mp3
		|	     .....
            ├──Unit 2──┬──BGU4_001A_01.mp3
            |          ├──BGU4_001B_01.mp3
		|	     .....
            ....
```

Unit フォルダーだけで 130くらいある。

なので、Unit ごとに連結した mp3 をつくり、
その連結した mp3 を 10 Unit 分連結して一つの mp3 にする。

そんなことする script を作りたい。

[[# 複数の mp3 ファイルを結合する]]

mp3 の連結はこのshell script でやるとして、ruby で変数を用意してやる方向で。

## ruby script

```ruby
require 'fileutils'

Dir.glob(File.expand_path("cambridge/*")) do |path|
  # dir path like this. "/Users/george/Desktop/ruby/english/cambridge/Unit 1"
  # build a input list file
  puts("\n\n#{path}\n\n")
  mp3 = Dir.glob("#{path}/*.mp3")
  text = mp3.map{|f| "file '#{f}'\n"}.join
  File.write("#{path}/mylist.txt", text)

  # concat mp3 with input list file
  system("ffmpeg -f concat -safe 0 -i '#{path}/mylist.txt' -c copy '#{path}.mp3'")
  # rm directory
  system("rm -rf '#{path}'")
end
```

![[Pasted image 20210528223027.png]]

ディレクトリの中の mp3 を連結して一つの mp3 にする。

これをディレクトリに 10 個づづ cambridge に入れて 10個の mp3 ファイルにするを 10 回やる。

フォルダ名を変えて退避する。

退避した 10 個の新しく出来たフォルダで同じことをする。

10個くらいのファイルが出来上がった。



