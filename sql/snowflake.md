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

## Timestamp -> epoch time
```sql
DATEDIFF(second, '1970-01-01'::DATE, {timestamp});
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

## 方位の差分を0~180で表現する
単位円状の２つのベクトルの内積が、cos(sita)となることを利用

```sql
degrees(
    acos(
      sin(radians(degree_A))*sin(radians(degree_B)) + cos(radians(degree_B))*cos(radians(degree_B))
    ))
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

## 2つのテーブル間で、keyが存在しないデータを抽出
https://www.tutorialgateway.org/sql-not-exists-operator/

```sql
SELECT Employ1.[EmpID]
      ,Employ1.[FirstName] + ' ' + Employ1.[LastName] AS [Full Name]
      ,Employ1.[Education]
      ,Employ1.[Occupation]
      ,Employ1.[YearlyIncome]
      ,Employ1.[Sales]
      ,Employ1.[HireDate]
  FROM [Employee] AS Employ1
  WHERE NOT EXISTS( SELECT * FROM [Employee] AS Employ2 
		    WHERE Employ1.[EmpID] = Employ2.[EmpID] 
			 AND [Sales] > 1000
		  )
```