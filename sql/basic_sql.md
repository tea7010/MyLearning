## 教材
https://www.kaggle.com/learn/intro-to-sql

* With: Create a statement body which can be used later in the statement
    * 変数っぽい使い方ができる
    * クエリのネストを深くしないのが便利
    * 他にも再帰関数の定義にも使える
    * https://docs.snowflake.com/en/sql-reference/constructs/with.html
```sql
with
  albums_1976 as (select * from music_albums where album_year = 1976)
select album_name from albums_1976 order by album_name;
+----------------------+
| ALBUM_NAME           |
|----------------------|
| Amigos               |
| Look Into The Future |
+----------------------+
```