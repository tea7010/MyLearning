## JoinとMergeの違い
https://towardsdatascience.com/pandas-join-vs-merge-c365fd4fbf49#:~:text=We%20can%20use%20join%20and,join%20on%20for%20both%20dataframes.

* テーブルL: 左側のテーブル、テーブルRは右側のテーブルとする
* Joinは、Lについてはkeyのindex or columnを選択できるが、Rはindexしか指定できない
```python
joined = L_df.join(R_df, how='left')
```

* Mergeは左右どちらもcolumnをkeyとして指定可能
    * Joinを内包している
```python
merged = L_df.join(R_df, how='left', left_on='L_key', right_on='R_key')
```

## DictをMappingしたいとき
https://stackoverflow.com/questions/64952346/map-value-to-a-column-if-key-isin-value

例: dfに、dt.hoursなどで時間を抜き出したhoursカラムがあるとする。それに時間帯リストとhoursを照合させてラベルをマッピングしたい
```python
# 時間帯のリスト
hours = {
    'ランチ営業': [11, 12, 13],
    '夜営業': [18, 19, 20]
}

def map_hours_chunk(x, hours):
  for i, v in hours.items():
    if x in v:
      return i

df['hours'].map(lambda x: map_hours_chunk(x, hours))
```

## Groupbyのグループのイテレーション

groupbyのオブジェクトをそのままイテレーションに突っ込むと、group名とそれに該当するデータがイテレーション可能
```python
for name, group_data in data.groupby('group'):
  ...
```

## describeの出力のexponential表示を、見やすい表示に変える
https://qiita.com/sussan0416/items/811508eff0fb53a16eed
```python
df['some_column'].describe().apply(lambda x: format(x, 'f'))
```


## 高速化Tips
https://shinyorke.hatenablog.com/entry/pandas-tips
* applyは可読性が上がるが、series(単一カラム)のループ高速化はfor < apply < mapで強い
* やはりnumpy配列でベクトル演算するのが一番早い
