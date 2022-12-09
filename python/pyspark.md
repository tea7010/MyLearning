https://towardsdatascience.com/pyspark-on-google-colab-101-d31830b238be

https://www.tutorialspoint.com/pyspark/index.htm

https://github.com/databricks-academy/INT-JEPFS-V2-IL

https://github.com/databricks-academy/apache-spark-programming-with-databricks/tree/published/Apache-Spark-Programming-with-Databricks

# Install Pyspark on local
* Java
* pyspark
* findspark
https://sparkbyexamples.com/pyspark/install-pyspark-in-anaconda-jupyter-notebook/


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

### DDL(Data Definition Lnaguage)を出力する
scalaのみ
```scala
df.schema.toDDL
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

### Joinを複雑な条件で行う
```python
df1.join(df2, on=[
    df1.{keycol1} == df2.{keycol1},
    df1.{keycol2} >= df2.{keycol2}
    ], how='inner')
```

### カラムの型変更
```python
df.withColumn('colname', df.{colname}.cast({type}))
```

castの中身は'int', 'interger', IntegerType()など

### カラムの値を新しいDFとして返す
Pandasでいう`df[[col1, col2]]`みたいな操作

```python
df.select([{col1}, {col2}])
```

### ユーザ定義関数の適用
```python
def plus_one(v):
    return v + 1

my_udf = udf(plus_one)

df.withColumn('udf_result', my_udf('use_colname'))
```

一気にudfを定義する方法
```python
struct_schema = StructType([
    StructField('size', IntegerType(), nullable=True),
    StructField('elements', ArrayType(IntegerType()), nullable=True)
])

@udf(returnType=struct_schema)
def calc_len(arr):
    if arr:
        return {'size': len(arr), 'elements': arr}
```

### Pandas UDF
普通のUDFより速いらしい
https://spark.apache.org/docs/3.0.0/sql-pyspark-pandas-with-arrow.html


```python
def plus_one(a: pd.Series) -> pd.Series:
    return a + 1

my_udf = pandas_udf(plus_one, returnType=LongType())

df.withColumn('udf_result', my_udf('use_colname'))
```

#### Windowのpandas udf
https://stackoverflow.com/questions/48160252/user-defined-function-to-be-applied-to-window-in-pyspark
```python
from pyspark.sql import Window

@pandas_udf("double")
def mean_udf(v: pd.Series) -> float:
    return v.mean()

df = spark.createDataFrame(
    [(1, 1.0), (1, 2.0), (2, 3.0), (2, 5.0), (2, 10.0)], ("id", "v"))
w = Window.partitionBy('id').orderBy('v').rowsBetween(-1, 0)
df.withColumn('mean_v', mean_udf("v").over(w)).show()
```

#### グループごとに統計・機械学習モデルを適用
https://qiita.com/paulxll/items/98cd3d3d8adbf6197660

```python
from pyspark.sql.functions import col, pandas_udf, PandasUDFType

schema = StructType([
  StructField("group", StringType(), False),
  StructField("v", DoubleType(), False),
  StructField("v2", DoubleType(), False),
  StructField("cluster", IntegerType(), False)
])

@pandas_udf(schema, PandasUDFType.GROUPED_MAP)
def grouped_km(pdf):
    """
    groupごとにKMeansによるクラスタリングを実施しデータを5つに分ける。
    """
    from sklearn.cluster import KMeans
    km = KMeans(n_clusters=5, random_state=71)
    pdf.loc[:, "cluster"] = km.fit_predict(pdf.loc[:, ["v", "v2"]])
    return pdf

df.groupBy("group").apply(hml_rank).show(10,False)
```


### GroupByして、平均とかを計算する
```python
df.groupBy('group_col').agg(mean('colname'))
```

### GroupByしたものに対して、複数の集計を実行する
```python
df.groupBy('cat').agg(mean('col1'), max('col1'), min('col2'))
```

### Gruop内で、カテゴリカルな値のカウントをJsonにする
```python
df = df.groupby("id", "type").count()
df = df.groupby("id").agg(map_from_entries(collect_list(struct("type","count"))).alias("count"))
```
https://blog.kozakana.net/2020/12/count-the-number-of-specific-columns-and-put-them-together-in-a-map-format/

### カテゴリカラムをPivotして1 or 0をする
```python
pivot_df = (
  df
  .groupBy('{row_column}')
  .pivot("{category}")
  .count()
  .fillna(0)
)
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

## Map/Struct系

### Map型のカラムの最大の値のものを抜き出す
```python
@udf(returnType=StructType([
StructField('argmax_key', StringType(), nullable=True),
StructField('max_val', LongType(), nullable=True)])
)
def udf_keywithmaxval(d):
  if d is None:
    return {
      'argmax_key': None,
      'max_val': None
    }
  else:
    k = list(d.keys())  
    v = list(d.values())
    return {
      'argmax_key': k[v.index(max(v))], 
      'max_val': max(v)
    }
```