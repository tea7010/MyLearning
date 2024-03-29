# Data Streamとは
* https://takuti.me/ja/note/data-stream-mining/

* https://direct.mit.edu/books/book/4475/Machine-Learning-for-Data-Streamswith-Practical

## Data Stream Clustering
* https://en.wikipedia.org/wiki/Data_stream_clustering
    * Wikipedia
* https://www.dbs.ifi.lmu.de/Lehre/KDD_II/WS1213/skript/KDD2-4-StreamClustering.pdf
    * どこかの授業の資料。背景や目的が分かりやすい

# Stream Clustering
https://www.matthias-carnein.de/streamclustering
* 手法がまとまっている

1. Distance-Based Algorithms
    - Clustering Feature: BIRCH, etc.
    - Extended Clustering Feature: CluStream, etc.
    - Time-Faded Clustering Feature: DenStream, etc.
    - Medoids: RepStream
    - Centroids: STREAM
    - Competitive Learning: SOStream, DBSTREAM

2. Grid-Based Algorithms
    - One-Time Partitioning: D-Stream
    - Recursive Partitioning: - Stats-Grid
    - Hybrid Grid-Approaches: HDCStream

3. Model-Based Approaches
    - CluDistream

4. Projected Approaches
    - HPStream

# OVERVIEW OF STREAMING-DATA ALGORITHMS
https://arxiv.org/ftp/arxiv/papers/1203/1203.2000.pdf

## Introduction
* 継続的に取得されるtime series dataを、data streamsと呼ぶ
* time seriesの手法には全データを読み込むような前提がある手法など、data streamsでは現実的でないものもある
* クラスタリングは５つの種類に一般的にカテゴライズされている
    * partitioning
    * hierarchical
    * density-based
    * grid-based
    * model-based

## Definitions
* Sを時系列の集合とし、s_iを時刻iの記録時間とする
* S_jは1<=j<=mの、mは記録されているセンサ値などの時系列項目数とする
* T1からTnまでの時刻で、n個のデータが観測されているイメージ
* nはtime seriesだと固定で、data streamだと増加していくイメージ

## Related Algotithms
* Partitioning methodはデータをk個のサブグループに分けるもの
* Partitioningの代表的なものはK-means, K-medoidsがある
* K-medoidsはCGアルゴリズムというものがベースになっており、これにはクラスタ数kをユーザが決める必要がある
* また任意の点のmedoidを計算する必要がある
* この問題に対処するためにCLARAと呼ばれる、計算に使うサンプル数をへらすアルゴリズムがある
* CLARANSはフレッシュなサンプルを重視するみたいな？
* しかし、K-medoidベースの手法はscalableでないということが一般に分かってきており、data streamsには応用が難しい

### k-Medoid
* k-medoidsは、PAM(Partitioning aroud Medoids)のアルゴリズムで最初に提唱された

* K-medoidのアルゴリズム
    1. ランダムにk個データを選び、それを初期medoidsとする
    1. 一番近いmedoidにデータを紐付ける。距離関数はユークリッド距離が一般的だが他でもOK
    1. 各medoid mに対して
        1. 任意の非medoid点 o とmedoid点を入れ替えたときのロスを計算
    1. ロスが最小となるmの選び方を選択する

* K-medoidsの実装・アルゴリズム例
https://towardsdatascience.com/k-medoids-clustering-on-iris-data-set-1931bf781e05

### k-means
* 今でも人気の手法
* centroidというクラスタの中心点のようなものを定義する
* controidはデータの中心座標を求めるイメージで、medoidは中心に近いデータを選ぶイメージ

### CLARA
* Clustering for Large Applications alorithm
* k-medoidsの計算コストがパねえので、サーチするサンプル数を減らす工夫をしたアルゴリズム
* 間引きすぎて重要なデータが使われない可能性がある？

### CLARANS
* よくわからない

### BIRCH
* 構造データをユーザのインプットを少なくメモリを有効に使って、処理することを考えたもの
* 計算オーダがO(N)で軽いが、複数回計算させると結果が改善する？
* CF tree: Clustering Feature treeが、動的にデータを階層分けする

### CURE
* HAC/CUREもBIRCHのようにデータセットを木分割するような手法
* クラスタの形が球状であることが仮定されているSSQを必要としない。非球状で分散が広いようなクラスタを特定可能
* ランダムサンプリングされる

### STREAM

### RESEARCH

# BIRCH
https://scikit-learn.org/stable/modules/generated/sklearn.cluster.Birch.html

# DENGRIS-Stream
http://psrcentre.org/images/extraimages/45.%201312568.pdf

# DenStream
https://github.com/issamemari/DenStream

# D-Stream
https://github.com/ogeagla/dstream

# EvoStream
https://github.com/MatthiasCarnein/evoStream_python


# CluStream
* https://www.dbs.ifi.lmu.de/Lehre/KDD_II/WS1213/skript/KDD2-4-StreamClustering.pdf
* K-Means baseの手法で、micro-cluster(online)とmacro-cluster(offline)の2フェイズ
    * Onlineフェイズではリアルタイムに要約するだけで、時間横断したmacro-clusteringは行わない
    * Offlineで、ユーザが見たいある期間に関して、その時初めてmacro-clusteringを行う
* Online phaseでは、新しいデータが、既存のモデルに対して、近傍のクラスタが存在するかどうかを判定
    * 存在してある半径内であるなら、その点をCFに足し算して追加する
    * 存在しないなら新しいクラスとして登録(CFを新規作成)
    * micro-clusterのSnapshotが完成
* Offline phaseでは、ある期間内のmicro-clusterをすべて持ってきて、それらのCFに対してK-Meansを適用する
    * Evolvingしても、そのCF群は密になってるはずなのでクラスタリングできるはず（CFは徐々にズレるだろうという仮定）

![](img/2021-09-13-13-14-31.png)
