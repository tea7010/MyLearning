# Pythonのクラスについて

###### tags: `computer` `python`

## メモ
* クラスはオブジェクト・概念として設計する
* クラスの継承は、上位概念を下位？階層のクラスに継承させるイメージ
* 例えば人間クラスがあったとして、女性クラスは人間クラスを引き継ぐみたいな感じ

## 実践レベル
```python
# 親クラス
class animal:
    def __init__(self, name):
        self.name = name
        self.spicies = 'mammal'
```
があったとして、これを引き継いだ子クラスの例としては

```python

class dog(animal):
    def __init__(self, name, color):
        super().__init__(name)
        self.color = color
```

みたいな感じでsuperの部分でメンバ変数を引き継いで、新しいメンバ変数もこれをしないと定義できないっぽい
* メソッドも同様
* 子クラスから上書きが出来る様子

## pytestと相対import
* この親子クラスは個人的にはプログラムの可読性を上げるために必要なこと
* 実際はフォルダ構造でクラスを定義していくのでimportが必須
* そのときにどこをrootにしてみたいなのが問題になる

基本的にはsys.pathに場所を教えてあげてそこから絶対importをする構造にするべきっぽい
```python

module_path = 'absolute/hoge'
sys.path.append(module_path)

from {mofule_pathからの絶対import} import hoge
```

なので、各module内でのmoduleは相対パスでないと上が動かない

まとめると、
* moduleを作る時は相対importで作る
* 動作確認は直接そのファイルでは行わず、sys.path.appendでテストファイルや実行mainから呼び出す

## 相対パスで上位moduleをimport
* sklearnのソースコードを見てみたら、

```
utils - hoge
LinearModel - logistic.py
```
みたいな構成上で、logistic.pyからutilsへ相対パスでimportしていた
```
from ..utils.hoge
```
こんなのが出来るっぽいと、共通的な部分はutilに投げることが可能そう