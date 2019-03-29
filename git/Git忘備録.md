# Git忘備録
Gitコマンドを覚える、理解するためのメモ

###### tags: `computer` `memo` `git`

## メモ
* add <-> reset
* `git remote show origin`: リモートの状況？を調べる
* `git remote prune origin`: リモートで消されているブランチをローカルの表示から消す
* `git branch -m [name]`: ブランチ名の変更
* `git branch -c [name]`: 今のブランチのコピー
* `git reset -soft [commit hash]`: 特定コミットまでログ遡る（working tree, index fileは保持）
* pull = fetch + merge
* pull --rebase = fetch + rebase
* 筆者別コミット数: git shortlog -s -n --all --no-merges

## mergeとrebaseの違い
* それぞれ結果的には２つのブランチが１つになるもの
* ただし、マージは文字通り枝分かれしたものをどちらかに時系列そのままでくっつけるイメージ？
* rebaseはくっつけたい方のブランチを、最新のコミットとしてくっつける
* rebaseはくっつけるブランチのコミットが変わってしまうので、どうやらリモートにそのブランチがあると、リモートとローカルのコミット履歴の整合性がとれなくなって面倒なことになるらしい

## submodule
* 他のレポジトリを使いたいときに、コピペじゃなくてちゃんとそのレポジトリもgit管理したい場合につかう
* 子レポジトリみたいなイメージだが、基本はpullしかしない前提？

## sparse-checkout
* レポジトリの一部のみを使いたくて、他の部分はいらないというときに、見えなくする感じ

## 心得
* pullは理解が十分できるまで使わない
* git logもよく見る