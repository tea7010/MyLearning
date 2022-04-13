https://towardsdatascience.com/pyspark-on-google-colab-101-d31830b238be

https://www.tutorialspoint.com/pyspark/index.htm

https://qiita.com/paulxll/items/98cd3d3d8adbf6197660

# SQL

## SQLクエリを実行する
```python
spark.sql({query})
```

# DataFrame
https://qiita.com/gsy0911/items/a4cb8b2d54d6341558e0


## 空のDataFrameをSchemaだけ指定して定義
面倒だが`sql.types`から必要なデータ型を取ってる来る必要あり
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

df = spark.createDataFrame(data=[], schema=StructType([
    StructField("person", StringType(), True),
    StructField("age", IntegerType(), True)
]))
```

## DataFrameをtableとして出力 or Insert
```python
df.write.saveAsTable({table name}, mode={'append', 'overwrite', etc.})
```
upsertはない様子

## 作ったテーブルを読む
```python
df = spark.read.table({table name})
```
or
```python
df = spark.table({table name})
```