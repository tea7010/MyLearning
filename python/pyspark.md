https://towardsdatascience.com/pyspark-on-google-colab-101-d31830b238be

https://www.tutorialspoint.com/pyspark/index.htm

https://qiita.com/paulxll/items/98cd3d3d8adbf6197660

# SQL

## SQLクエリを実行する
```python
spark.sql({query})
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

## ETL

### Nullカウントを全カラムにやるワンライナー
```python
import pyspark.sql.functions as F

df.select([F.count(F.when(F.col(c).isNull(), c)).alias(c) for c in df.columns)])
```

