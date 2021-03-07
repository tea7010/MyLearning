# 半教師あり学習

* 分類と生成で分かれる
* 生成はGANなど
* 分類は、識別とクラスタリング
    1. 中途半端にあるラベルを使って、暫定モデルを作成
    2. 暫定モデルにラベルなしのデータを突っ込んで予測
    3. リスクを計算して、モデルを更新。１に戻る？
* EMアルゴリズム（最尤推定？）も鍵っぽい

* アクティブラーニング: モデル学習に使うデータを能動的に選ぶ
    * オンライン学習とは別
    * 予測精度（正解確率）の低いデータをあえて選ぶと、境界がはっきりするイメージ

* モデル作るだけならKNNでいけるかも
    * クラスタリングの結果をY、データをXとして、KNNに食わせる

## 論文&参考
* https://library.naist.jp/dspace/bitstream/handle/10061/13117/Classification_Method20190215.pdf?sequence=1&isAllowed=y
    * ちょい違うかな
* https://www.wantedly.com/companies/optimind/post_articles/184481
    * なんか似てることしてそう
* https://qiita.com/omiita/items/07e69aef6c156d23c538
* https://deepsquare.jp/2020/07/transformer/#outline__1_1
    * Transformer: どうやら要素同士の連続性も考慮されるらしい。
        * 画像系より前にNLPで2017から流行。