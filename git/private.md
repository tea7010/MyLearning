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
