## 教材
https://www.kaggle.com/learn/intro-to-sql

## 2. Select, From&Where
* select + {columns}
* from + {data table}
* where + condition

## 3. Group By, Having & Count
* Count: 要素数を返す集計関数
  * Count(1): 無駄なスキャンが減るテクのひとつ。とにかく要素数を返してくれる
* Group by: カテゴリ毎にグループとする
  * selectのカラムはbyのカラム以外は、集計関数を通して渡す必要あり
* Having: Group byでグループに対する条件を記述

## 4. ORDER BY
* Order by: ソートするもの。最後に記述する
* 逆順にしたいときは`DESC`を隣に記述
  * `ORDER BY {colname} DESC`
* EXTRACT: date/datetime型から、year/dayなどを抜き出す関数
  * `EXTRAXT(week from DateCol)`

## 5. As & With
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