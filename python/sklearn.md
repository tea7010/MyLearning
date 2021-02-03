## Scikit learnのpiplineあたり

### pipline
https://qiita.com/shota-imazeki/items/4a69e4ef2f12e0313495
https://qiita.com/shota-imazeki/items/e09ea39660bb4d5d0a90
* steps: [('step1', class), ('step2', class)]という入力
* fit, transform, fit_transform, predictなどのメソッドがある
    * fit/transformなど、入力したclassにfit/transformのメソッドが統一されて存在
    * fit/transformしたときに、各stepの統一された入力に順次fit/transformされていく
* 引数の渡し方は少し癖あり

### Function Transformer
https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/preprocessing/_function_transformer.py

* 通常の関数をtransfromerクラスに変換する機能
* 多分これ使うと、カスタム関数をpiplineに組み込めたりする

## 活用アイデア
* 最小開発単位として、追加分のseries or dataframeを返す関数を作成
* piplineを流用して使いたい
* それをpiplineに流すためのTransformerを用意して、piplineに関数と引数だけ入れる仕様にして、あとはpipline内でTransformする?
* すべての関数をsklearn準拠にする?transformerを改造?

```python
pipe = pipline({
    'step1': [functionA, args]
    'step2': [functionB, args]
})

df = pipe.transform(df)

class ProcessPipeline(Pipline):
    def __init__(self, steps: list[(function, args)]):
        self.steps = self.Sktransform(steps)
```
