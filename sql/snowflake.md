## テーブルの作成
https://docs.snowflake.com/en/sql-reference/sql/create-table.html

```sql
CREATE OR REPLACE table {table_name} ({metric1} {unit1}, {metric2}, {unit2}, ...);
```

## テーブルへのデータインサート
```sql
insert into {table_name} ({metric1}, {metric2}) values
    ({val1}, {val2}),
    ()...
```

## ARRAYのカラム作成
https://docs.snowflake.com/en/sql-reference/functions/array_construct.html
```sql
select array_construct(valu, value, ...);
```

## JSONカラムの作成
https://docs.snowflake.com/en/sql-reference/functions/object_construct.html
```sql
object_construct(key, value, key2, value2, ...);
```

## Groupbyのとき、Groupの行の値をリストで格納させる
https://docs.snowflake.com/en/sql-reference/functions/listagg.html
```sql
select ..., listagg({value_column}, {deliminater})
from ...
group by g
```

## Timestampの丸め
https://docs.snowflake.com/en/sql-reference/functions/time_slice.html

例: 10分間隔の丸め
```sql
TIME_SLICE({DATETIME_COL}, 10, 'MINUTE')
```

## グループの最初の値を取得
group byしたときの最初の値がほしい（`pandasでいうdf.groupby('group')[col].first()`)

https://docs.snowflake.com/en/sql-reference/functions/first_value.html

```sql
select 
    ...
    first_value({カラム名}) over (partition by {group byしたいカラム} order by {firstの順番をどうしたいか})
...
```

## Fillnaする
https://docs.snowflake.com/en/sql-reference/functions/coalesce.html

特定の値で置換したいとき
```sql
    coalesce({カラム名}, {置換したい値})
```

nullのところを前の行の値で置換したいとき
```sql
    coalesce({カラム名}, lag({カラム名}, 1) ignore nulls over (partition by {} order by {}))
```