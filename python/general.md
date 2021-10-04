## １桁の数字を0付きの数字にしたい

4 -> 04みたいな

```python
str(4).zfill(2)
```
* 多分Zerofillの略
* 最初から二桁の数字はそのままになる
* zfillの中の数字で0をつける桁数が変わる


## FutureWarning出てるけど何処や?
```python
import warnings
with warnings.catch_warnings():
     warnings.simplefilter("error")
     
     {warningを吐くコード}
```

## Listの共通しない要素
* https://note.nkmk.me/python-list-common/

```python
list(set(l1) ^ set(l2))
```