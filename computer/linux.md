# Linuxのメモ

###### tags: `computer` `memo`

## Terminal
* `Shift + Ctl + t`: 端末の起動
* `Shift + Ctl + t`: 新しいタブ
* `Alt + {num}`: タブの切り替え

## ワークスペース
https://sicklylife.jp/ubuntu/1804/help/shell-windows.html#working-with-workspaces
* `Super + PageUp/Down`: ワークスペース切り替え
* `Super + Shift + PageUp/Down`: 今のウインドウをワークスペースに移動

## tmux
* `Ctl + b`: コマンド待ち
* ` + c`: 新しいウインドウ
* ` + p/n`: ウインドウ切り替え
* ` + [`: ウインドウ中のスクロール開始(矢印キーでカーソル移動)
* `q`: ウインドウ中のスクロール終了
* ` + d`: detach（抜ける？）
* `tmux ls`: tmuxで実行中のウインドウ？
* `tmux attach -t {セッション名}`: アタッチ（入る）

## ファイル送るとき
* sshと似たような感じでscpというコマンドがある。
* 'scp {送り元} {送り先}': ローカルとリモートでファイルの送信ができる。リモート側はsshと同じようにIPアドレスみたいなのをかく

## ファイル名の一括変換
フォルダ内の`file1.txt, file2.txt, ...`の`file`を`MyFile`に変換したいとする

```bash
rename 's/file/MyFile/' file*.txt
```

