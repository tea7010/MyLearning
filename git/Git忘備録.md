# Git忘備録
Gitコマンドを覚える、理解するためのメモ

###### tags: `computer` `memo` `git`

# メモ

## コマンドメモ
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


# GithubのSSH認証
https://qiita.com/shizuma/items/2b2f873a0034839e47ce

> GitHubと実際にやりとりするときにID・パスワードを聞かれるときssh接続がうまくいっていません。レポジトリのディレクトリに入り

`git config remote.origin.url` or `git remote -v`

> で確認し　https://github.com/[ユーザID]/[リポジトリ]となっていたら

`git remote set-url origin git@github.com:[ユーザID]/[リポジトリ].git`

> AzureDevOpsの場合はSSHでクローンしたいときのURLを、`git remote set-url origin {SSH_URL}`とすればOK.


# Githubにプライベートアドレスを晒さない方法

## Private e-mail addressのダミー化
https://qiita.com/sta/items/982ab68e8220a81d485c

1. ダミーのメールアドレスの取得: https://github.com/settings/emails
1. メルアドを設定: `git config --global user.email "USERNAME@users.noreply.github.com"`

## Author/Commiterの一括変更
https://qiita.com/sea_mountain/items/d70216a5bc16a88ed932

1. コミットの全書き換え
```
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='tea7010'; GIT_AUTHOR_EMAIL='tea7010@users.noreply.github.com'; GIT_COMMITTER_NAME='tea7010'; GIT_COMMITTER_EMAIL='tea7010@users.noreply.github.com';" HEAD 
```
1. リモートブランチへの書き換えpush
```
git push --force origin master
```