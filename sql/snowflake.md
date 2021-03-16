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