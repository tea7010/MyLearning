https://towardsdatascience.com/pyspark-on-google-colab-101-d31830b238be

https://www.tutorialspoint.com/pyspark/index.htm

https://qiita.com/paulxll/items/98cd3d3d8adbf6197660

# SQL

## SQLクエリを実行する
```python
spark.sql({query})
```

## 使うDBを指定
```sql
USE {dbname}
```

## Upsert処理(SQLスキル)
例: タイムスタンプが新しいものだけ書き込みたい
```sql
MERGE INTO {target_table} AS tgt
  USING {temp_view_name} AS src

  ON tgt.cat_id = src.cat_id

  WHEN MATCHED
  AND tgt.timestamp < src.timestamp
    THEN UPDATE SET *
  WHEN NOT MATCHED
    THEN INSERT *
```

# DataFrame
https://qiita.com/gsy0911/items/a4cb8b2d54d6341558e0

## I/O

### csvの読み込み
```python
df = (spark.read
    .format('csv')
    .option('header', True)  # ヘッダの読み込み
    .option('inferSchema', False) # スキーマ（データ型）推測するかどうか
    .load({path})
    )
```

### jsonの読み込み
```python
df = (spark.read
    .format('json')
    .option('primitivesAsString', True) # スキーマ（データ型）推測するかどうか(全部strで読む)
    .load({path})
    )
```

### Schemaの確認
```python
df.printSchema()
```

### 空のDataFrameをSchemaだけ指定して定義
面倒だが`sql.types`から必要なデータ型を取ってる来る必要あり
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

df = spark.createDataFrame(data=[], schema=StructType([
    StructField("person", StringType(), True),
    StructField("age", IntegerType(), True)
]))
```

### DataFrameをtableとして出力 or Insert
```python
(
    df
    .write
    .format('delta') # databricksのとき
    .mode('append')
    .saveAsTable({table name})
)
```

### 作ったテーブル or Viewを読む
```python
df = spark.read.table({table name})
```
or
```python
df = spark.table({table name})
```

### テンポラリなViewを作成
そのセッションでしか有効にならない。データ置き場として使える。
```python
df.createOrReplaceTempView({view_name})
```

### DF -> JSONのリストで出力
```python
df.toJSON().collect()
```

## ETL

### 行・列カウント
```python
df.count(), len(df.columns)
```

### Nullカウントを全カラムにやるワンライナー
```python
import pyspark.sql.functions as F

df.select([F.count(F.when(F.col(c).isNull(), c)).alias(c) for c in df.columns)])
```

### NullをDropする
```python
dropped_df = df.na.drop(subset=[{col1}, {col2}])
```

### Fill na/null
https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.fillna.html
```python
df.fillna(0, subset=['col1', 'col2'])
```

### 重複チェック
```python
df.groupBy({col}).count().filter('count > 1')
```

### Join
pandasでいうMerge

```python
df1.join(df2, on=[{keycol1}, {keycol2}], how='inner')
```

### カラムの型変更
```python
df.withColumn('colname', df.{colname}.cast({type}))
```

castの中身は'int', 'interger', IntegerType()など

### カラムの値を新しいDFとして返す
Pandasでいう`df[[col1, col2]]`みたいな操作

```python
df.slect([{col1}, {col2}])
```

### ユーザ定義関数の適用
```python
def plus_one(v):
    return v + 1

my_udf = udf(plus_one)

df.withColumn('udf_result', my_udf('use_colname'))
```

### GroupByして、平均とかを計算する
```python
df.groupBy('group_col').agg(mean('colname'))
```

### GroupByしたものに対して、複数の集計を実行する
```python
df.groupBy('cat').agg(mean('col1'), max('col1'), min('col2'))
```

### 簡単なif, when
```python
df.withColumn('judge', when('colname' > 5, True).otherwise(False))
```

### Filter
```python
df.filter(df['col'] > 5)
```

### あるカラムのUniqueな値リスト
```python
[r[0] for r in df.select('colname').distinct().collect()]
```

### 定数値のカラム作成
pandasでいう`df['new_col'] = 5`みたいな

```python
df.withColumns('STATIC_VALUE', F.lit(5))
```

### カラム名を全部大文字 or 小文字にする
```python
df.toDF(*[c.lower() for c in df.columns])
```

### Window関数
SQLでいうOVER句。pandasでいうgroupbyしてそれごとに処理するみたいなことができる

https://x1.inkenkun.com/archives/5223

### Unixtimestamp <-> Timestamp
timestamp -> unix timestamp
```python
F.unix_timestamp(F.col({timestamp}))
```

unix timestamp -> timestamp
```python
F.col({timestamp}).cast('timestamp')
```

### Timestamp -> date(string)
```python
df.withColumn('date', F.to_date('timestamp', 'yyyy-MM-DD'))
```

### GruopbyでMedianをとる
stringのcategoryカラムは、数字にエンコードしたうえで、medianをとる
```python
F.percontile_approx(F.col({cate_encoded_col}), 0.5)
```

### Groupbyのグループ内の特定の値の数を数える
```python
F.sum(F.when(F.col({categori_col}) == 'value', 1).otherwise(0))
```

### あるカラムのユニークな値を見る
```python
df.select('column1').distinct().show()
```