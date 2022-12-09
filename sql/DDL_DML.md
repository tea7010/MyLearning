* DDL(Data Definition Language)
* DML(Data Manipulation Language)


## テーブルの作成
https://docs.snowflake.com/en/sql-reference/sql/create-table.html

```sql
CREATE OR REPLACE table {table_name} ({metric1} {unit1}, {metric2}, {unit2}, ...);
```

## テーブル名の変更
https://www.geeksforgeeks.org/sql-alter-rename/

```sql
ALTER TABLE table_name
RENAME TO new_table_name
```

## テーブルへのデータインサート
```sql
insert into {table_name} ({metric1}, {metric2}) values
    ({val1}, {val2}),
    ()...
```

## 変数の宣言
```sql
declare @a int = 10
select @a + 10
```

## Temporary tableの作成と呼び出し
```sql
select ...
INTO #tmp_list
FROM ...

select * from #tmp_list
```

## Materialized viewについて
https://docs.snowflake.com/en/user-guide/views-materialized.html

* Viewだが、集計済みのデータとして保存される仕組み（キャッシュではなく消えないイメージ）
* 元のテーブルが更新されると自動的にmaterialized viewもバックグランドでsnowflakeが更新してくれる
* あんまり頻繁に更新されるテーブルが参照されていると、リソースパワーを食うので注意
* クエリに時間がかかる複雑な集計かつ、頻繁に更新されないものだとメリットある
* ただし制約が結構きつい
    * 参照先のテーブルは一つだけ（view/materialzied viewに対しては不可）
    * join不可
    * order by, having, windowなど不可
    * sum/count/avg/min/maxなど基本的な集計関数のみ（時間をさかのぼって検索が必要なものなどは基本使用不可）
* クラスタが生成可能

作り方：view系の操作に`materialized`をつけるだけ
```sql
create or replace materialzied view {mv name}
```