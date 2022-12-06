# WSL2のpython環境構築

###### tags: `computer`

何かをインストールしたらとりあえずターミナルを再起動すること

## Pythonのインストール
```
sudo apt install python3.9
```

## Pip3のインストール
```
sudo apt install python3-pip
```

## Pythonのバージョン切り替え
pythonコマンドの参照先の確認
```
command -v python
```

現状のpythonalternativeの登録状況確認
```
sudo update-alternatives --config python
```

現状のpython系を探す
```
ls /usr/bin/ | grep python
```

pythonの候補を登録
```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1
```

もう一度登録状況確認から、優先したい番号を入力する
```
sudo update-alternatives --config python
```

## pipenvによるお手軽環境構築
https://qiita.com/imasaaki/items/25694783ae50dd66303e

WSL2インストール後、初回のみ
```bash
sudo apt install python3-pip
```

pipenvのインストール
```
sudo pip install pipenv
```

プロジェクトフォルダ直下での環境構築
```
pipenv --python=/usr/bin/python3
```

パッケージのinstall
```
pipenv install [package]
```

## VSCodeでWSLのJupyterにアクセスし、ソースファイルを編集
* WSL,PythonのExtension２つを入れ、使うPythonのpathを追加すればあとはipynb or pyファイルを作成すれば実行可能（なはず）

https://ylabdesk.com/python-environmental-construction#vscode%E3%81%AB%E6%8B%A1%E5%BC%B5%E6%A9%9F%E8%83%BDremote-wsl%E3%81%A8python%E3%82%92%E5%85%A5%E3%82%8C%E3%82%8B