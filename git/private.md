* Author/Commiterの一括変更
https://qiita.com/sea_mountain/items/d70216a5bc16a88ed932

```
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='tea7010';GIT_COMMITTER_NAME='tea7010';GIT_AUTHOR_EMAIL='tea7010@users.noreply.github.com';GIT_COMMITTER_EMAIL='tea7010@users.noreply.github.com'" -- --all
```

```
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='sea_mountain'; GIT_AUTHOR_EMAIL='valid_email@example.com'; GIT_COMMITTER_NAME='sea_mountain'; GIT_COMMITTER_EMAIL='valid_email@example.com';" HEAD 
```

```
git push --force origin master
```


* Private e-mail address
https://qiita.com/sta/items/982ab68e8220a81d485c