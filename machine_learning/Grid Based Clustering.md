## Grid based clustering
* https://core.ac.uk/download/pdf/162014218.pdf
    * なんかの本のキャプチャ
* https://www.youtube.com/watch?v=EZ_RJnta-54
    * なんかの発表スライド
* https://github.com/annoviko/pyclustering
    * CLIQUEというグリッドベース手法の実装ライブラリ
* https://list01.biologie.ens.fr/wws/d_read/machine_learning/SubspaceClustering/CLIQUE_algorithm_grid-based_subspace_clustering.pdf
    * CLIQUEについて

## 
https://www.youtube.com/watch?v=T37exGEEkaw

* STING: 最も典型的なグリッドベース手法
 * statistical information sttored in grid
* WAVECLUSTER: ウェーブレット変換を行うらしい

### STING
* Gridのlayerを作る
    * 1st layer: 全領域をカバーするもの
    * 2nd layer: 4x4とか
    * layerを重ねるごとに細かくなっていくイメージ
* 各cellのcount/mean/std/min/maxなどの統計量を計算する
    * これでlowerレベルの統計量を使って統計量が計算できるので、計算量が早い
* 何個クラスタがあるとかまでは言及しない感じ。可視化するときにユーザがわかりやすいのがメリット？
* 計算ORDERは最下層レイヤのグリッド数

### CLIQUE
* 各次元の密になっている部分をグリッドで１次元づつ囲むと、グリッドで囲まれたsubspaceができる