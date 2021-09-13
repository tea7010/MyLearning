# K-Means v.s. K-Medoids
https://www.researchgate.net/publication/301234359_Analysis_of_K-Means_and_K-Medoids_Algorithm_For_Big_Data
* K-Meansでは、Centoroid(p個の近傍点の平均ベクトル)を近さを定義する距離として用いる -> 外れ値が合った場合、平均がズレる
* K-Medoidsでは、Medoid(p個の近傍点の最も中心にある実際のデータベクトル)を用いる -> 外れ値が多すぎない限り、ズレない