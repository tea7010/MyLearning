# アルゴリズムの教科書

## Memo
* input(): ユーザ入力
* 17 ÷ 8
    * quotinent = 2 `17//8`
    * remainder = 1 `17%8`
* グローバル変数: 関数の外で定義した変数
    * 関数内でグローバル変数使うときは`global {var}`と宣言する
* ローカル変数: 関数の中で定義した変数
* format
    * `f'{1:2d}'` -> ` 2`
    * `f'{1:02d}'` -> `02`
    * `f'{1:05d}'` -> `00002`
    * `f'{1:2b}'` -> `10`(バイナリ)
* print( ,end): endはデフォルトだと`\n`になってるので、これ変えるとコンマ区切りとかできる
* Stack: 本を積み上げてくような感じで、下に積んだものは上のものを全部取らないと取れない
    * Last in First out
    * Push: 積み
    * Pop: 取り出し
    * Pythonのqueueモジュールにある`queue.LifoQueue`
* Queue: 待ち行列
    * First in First Out
    * enqueue: queueに入ること
    * dequeue: queueから抜けること
    * Pythonのqueueモジュールにある`queue.Queue`
* List: グラフの最小単位。各ノードとどのノードに繋がるかのエッジ情報（ポインタ）を対に管理する
    * 双方向リスト, 循環リスト, 片方向リスト（線形)
    * メモリ上のデータの位置をStack/Queueのようにずらさずに、任意のデータを追加・削除可能（高速）
    * O(1)で計算可能
* Tree: Listの一方向からの拡張な感じ
    * Rootから複数のNodeがBranch(Edge)として分岐する
    * Leafは末尾Nodeのこと
    * depth: Rootからの分岐回数的な
    * 閉路(分岐Node同士が繋がること？)はダメ(グラフになる)
* Graph: 有向グラフと無向グラフ
    * List/Treeもグラフの一例の概念と捉えられる
    * 隣接行列: nodeの隣接行列、ノード数nのn*n行列
* 線形探索: 一個一個愚直に調べてくイメージ
* 二分探索: 半分・半分にしていって絞り込む
    * ソートされている前提
* 二分探索木: 左 < 親 < 右となっている木構造のデータを探索すること
    * 深さ優先探索: ノードをある方向に進めるだけ進んで、行ききったら他の枝に行く感じ
    * 幅優先探索: 同じ深さのノードを探索していく
    * 深さ優先探索は、行きがけ順・通りがけ順・帰りがけ順の巡回方法がある
* 計算量（計算オーダ）: データ量nに対するアルゴリズムに必要なループの数
    * forだとO(n), for forだとO(n^2)
    * 必ずしもfor文のネストの深さに依存するわけではない
    * 2分探索はO(log2n)となる
* 選択ソート: 昇順に並べるとして、最小値を見つけて、次に残った中からまた最小値を見つけて、というふうな並べ替え方法
* バブルソート: 隣の値と比較して大きい or 小さいものを左に入れ替えれていく方法
* 挿入ソート: 左から順番に子ソートを１つづつ要素増やしながら繰り返していくイメージ
* クイックソート: 基準となる真ん中っぽい値を選んで、その値以下か以上かでデータを左右のグループに分けることを繰り返すもの
    * 一般的には再帰関数でやる

## Progress
until 5-4