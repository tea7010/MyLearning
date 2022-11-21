## フォルダ階層が下のプライベートライブラリのImport
syspathに追加する。absoluteでもrelativeでもOK

```python
import sys
sys.path.append('..')
```

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

## lower/upper caseの両方を許すUNIX形式のパターンマッチ
* https://stackoverflow.com/questions/10148215/accounting-for-lower-and-uppercase-in-glob
* https://jdhao.github.io/2019/06/24/python_glob_case_sensitivity/

```python
def foo(extension):
     return '*.' + ''.join('[%s%s]' % (e.lower(), e.upper()) for e in extension)

foo('mov')
>>> '*.[mM][oO][vV]'
```

## zipfileをインメモリで読み込む
```python
from zipfile import ZipFile

def extract_zip_in_memory(input_zip) -> dict:
    input_zip = ZipFile(input_zip)
    return {name: input_zip.read(name) for name in input_zip.namelist()}
```

## pickleでObjectを保存・読み込み
```python
import pickle

class PickleUtils:
    @staticmethod
    def write(obj, dest):
        with open(dest, 'wb') as p:
            return pickle.dump(obj, p)
    
    @staticmethod
    def load(src):
        with open(src, 'rb') as p:
            return pickle.load(p)
```

## List/Tuple/Set/Dictの違い
https://qiita.com/ishida330/items/9692836aa860b2d0c36c

* A "set" doesn't accepts duplicated value
    * Mutable
    * doesn't have an order of values