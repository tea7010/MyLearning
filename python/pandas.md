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

## strカラムを複数delimiterで結合したカラムを作成
```python
df['concate'] = df[['Year', 'quarter', ...]].agg('-'.join, axis=1)
```

## 複数のDFをエクセルの各シートに出力
https://note.nkmk.me/python-pandas-to-excel/

```python
with pd.ExcelWriter('excel_filename') as writer:
    df.to_excel(writer, sheet_name='sheet1')
    df2.to_excel(writer, sheet_name='sheet2')
```

## delimiterで区切られているカラムをunpivotする

```python
def unpivot_by_detemiter(df, col, delimiter) -> pd.DataFrame:
    # stack rows by split values with multi-index
    unpivots = df[col].apply(lambda x: pd.Series(x.split(delimiter))).stack()

    # level_0 will be the original indecies
    unpivots = unpivots.reset_index()
    
    # rename unpivot column name
    unpivots.rename(columns={0: col + '_unpivot'}, inplace=True)

    # merge to original table
    return unpivots.merge(df, left_on='level_0', right_index=True)
```

## dictのlist -> read_csvするときの挙動
```python
dict_list = [{
  'a': 1,
  'b': 2
}, {
  'a': 5,
  'c': 3
}]
pd.DataFrame(dict_list)
```
これでもちゃんと存在しないkey(column)に対しては、nanで読み込んでくれる