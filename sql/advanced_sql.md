## 教材
https://www.kaggle.com/learn/advanced-sql


### 1. Join/Union
* Join: Horizontally combine
    * inner/left/right/fullが存在
    * innner: 共通して存在する行（key）のみJoin
    * left: 左のテーブルの行はすべて、Joinは存在するもののみ
    * right: leftの逆（これはどっちにJoinするかを書き換えれば不要?)
    * full: どっちの行もすべて返す(ない場合はnull)
```sql
left_table AS L INNER JOIN right_table AS R
```

* Union: Vertically combine
    * カラムは一緒でないといけない
    * union allでユニークなものだけ返すことが可能
```sql
select key_col, ... from table_a
UNION ALL
select key_col, ... from table_b
```

### 2. Analytic Functions
```sql
SELECT bike_number,
    TIME(start_date) AS trip_time,
    FIRST_VALUE(start_station_id)   -- 集計関数の定義
        OVER (  -- OVER節
            PARTITION BY bike_number    -- パーティションのカラム
            ORDER BY start_date     -- 並び替え
            ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING    -- ウインドウ範囲
            ) AS first_station_id,
    start_station_id,
    end_station_id
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
WHERE DATE(start_date) = '2015-10-25' 
```
* OVER: 計算に用いる行・計算を宣言するもの
    * ３つの節に分けられる
    * PARTITION BY: グループ分けをどうするか
    * ORDER BY: 分けたグループの並べ替えをどうするか
    * （名前難しい）: 計算に使うWindowのスケールなどの定義
        * `ROWS BETWEEN 1 PRECEDING AND CURRENT ROW`: １行前~その行
        * `ROWS BETWEEN 3 PRECEDING AND 1 FOLLOWING`: 3行前~その行+1行
        * `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`: 過去全て~その行移行 = そのパーティション内の全行
* 最初の集計関数はいろいろ使える
    * avg/min/max
    * first_value
    * rank