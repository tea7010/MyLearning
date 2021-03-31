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
