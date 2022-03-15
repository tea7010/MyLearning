https://camp.trainocate.co.jp/magazine/rest-api/
https://qiita.com/Saku731/items/6ae290f72e98723f165d

* REST: Representational State Tranfer
    * HTTPプロトコル
    * URLでしているするサーバとの間
    * テキスト情報をやりとりする
    * HTTPリクエストをまずクライアントが投げて、どんな情報がほしいのかをAPIの変数として渡す
* API End Point: サービスに接続するためのURL
* API Key: 接続に必要なパスワード
* Method: Post or Get
* Query Parameter: 使用するAPI・サービスの種類を指定する
* Header: データの種類やキーなどが入るところ(Post)
* Body: APIと送受信したい情報・変数が入るところ(Post)

例
```python
url = "http://zip.cgis.biz/xml/zip.php"
params = {"zn": "100-2101"}

r = requests.get(url, params=params)
print(r.ok, r.status_code, r.reason)
r.text
```