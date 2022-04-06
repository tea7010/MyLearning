https://python.kirikutitarou.com/2019/06/20-python.html

* ThreadPoolExecutorとProcessPoolExecutorの２種類がある
    * ProcessPoolExecutorは、並列でpythonインタプリンタを立ち上げるイメージ
    * ThreadPoolExecutorは、pythonはインタプリンタひとつで、処理コードをCPUの各スレッドに渡してくれるイメージ
* ThreadPoolの方が１台で動かす分には無駄がない
* ProcessPoolは分散コンピュータ処理で使うようなもの