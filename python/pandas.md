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
