DQL (Data Query Language)

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