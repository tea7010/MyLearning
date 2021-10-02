# DNN用PCの構築メモ

###### tags: `computer` `memo` `hobby`

## コンセプト
用途で仕様を決めるといいらしい
1. RAW現像
1. ネットサーフィン
1. DNN勉強用
1. DMM英会話

* CPU、GPU共にそこそこでいい
* メモリは16はあったほうがいいかな
* CPUは最新のi5が昔のi7並でコスパいいらしい
* DNNするにはintelのCPU、GPUはNVIDIAのGTXシリーズ, OSはLINUXがデファクトスタンダードらしい
* GPUは調べた感じ1060クラスがいいらしい
* OSは今ubuntuで困ってないのでubuntuでOK。windowsはノートPCでいいと思う
* トータルで10万円以下にしたい
* HDD/SSDはもらったPC用に買ったやつを流用

## 構成
トータル: 92,882

CPU
* i5-9400F
* https://www.amazon.co.jp/INTEL-%E3%82%A4%E3%83%B3%E3%83%86%E3%83%AB-9MB%E3%82%AD%E3%83%A3%E3%83%83%E3%82%B7%E3%83%A5-LGA1151-BX80684I59400F/dp/B07MRCGQQ4/ref=pd_sim_147_2/358-5209762-9799323?_encoding=UTF8&pd_rd_i=B07MRCGQQ4&pd_rd_r=658271cd-89c9-11e9-9fc9-cd4e394b4b57&pd_rd_w=qb7ml&pd_rd_wg=sttom&pf_rd_p=b88353e4-7ed3-4da1-bc65-341dfa3a88ce&pf_rd_r=RGZPY100NHBP6D03Z4J0&psc=1&refRID=RGZPY100NHBP6D03Z4J0
* 21150

GPU
* GTX 1660
* https://www.amazon.co.jp/gp/product/B07PM52WPH/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1
* 27356

MB
* MSI MPG Z390M GAMING EDGE AC
* https://www.amazon.co.jp/MSI-MPG-Z390M-GAMING-EDGE/dp/B07J9XHWC9/ref=pd_yo_rr_psims_3/358-5209762-9799323?_encoding=UTF8&pd_rd_i=B07J9XHWC9&pd_rd_r=ba3a4ec7-e185-4c15-8bdc-e188f347e217&pd_rd_w=l73EH&pd_rd_wg=gZIEr&pf_rd_p=0aa8b886-d917-4d5f-ad88-2abde115f6fe&pf_rd_r=MW2QF2MBCVC30HTHDN8C&psc=1&refRID=MW2QF2MBCVC30HTHDN8C
* 19770

Mem
* DDR4-2666 8Gx2
* https://www.amazon.co.jp/gp/product/B077S8WPWZ/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1
* 8970

Sup
* 玄人志向 500W 80Plus Platinum
* https://www.amazon.co.jp/gp/product/B00IIF9KD2/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1
* 7538

Case
* Fractal Design Define Mini C
* https://www.amazon.co.jp/gp/product/B01N44IE48/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1
* 8090

 
## 設定

### 1. Install OS
1. USBメモリのインストールディスクを他のPCで作成
2. BIOSの起動PriorityをUSBメモリを一番にする
3. あとは手順に従ってSSDにインストール

* OS: ubuntu 18.04(最初) -> Elementary OS 5.0
* Language: Japanese 
* Keyboad: US 
* インストールオプションみたいなの: 最小限
 
### 2. Install GPU driver 
1. NVIDIAのページからドライバをダウンロード 
1. X server止めろエラーが出たので、ググって対処

* https://www.geforce.com/drivers
* https://askubuntu.com/questions/149206/how-to-install-nvidia-run　
* `Ctrl + Alt + F1`: CLIモード
* `Alt + F7`: GUIモード
 
### 3. 前のPCのHDDを削除
* diskからHDDを選択してフォーマット
* 新しくどんなファイルシステム？にするかを選択する必要がある
* Ext4というlinux上で扱える形式にしておく
* https://eng-entrance.com/linux-format#i-2
 
### 4. 細かい設定
ホームdir下のダウンロード等のフォルダ名を英語表記にする
* `LANG=en_US.utf8 xdg-user-dirs-gtk-update`
* https://www7390uo.sakura.ne.jp/wordpress/archives/690

キーボード・日本語入力
* https://do-you-linux.com/blog/2018/05/03/ubuntu18-04-lts%E3%81%A7%E3%80%8C%E6%97%A5%E6%9C%AC%E8%AA%9E%E5%85%A5%E5%8A%9B%E3%80%8D%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95-%E6%97%A5%E8%8B%B1%E3%82%AD%E3%83%BC%E3%83%9C%E3%83%BC%E3%83%89%E5%AF%BE/ 

Tab補完で大文字小文字の区別をしない
* e-OS不要
* ~/.imputrcに`set completion-ignore-case on`
* http://neos21.hatenablog.com/entry/2018/10/04/080000
 
Tab補完を候補自動入力
* https://stackoverflow.com/questions/7179642/how-can-i-make-bash-tab-completion-behave-like-vim-tab-completion-and-cycle-thro

HDDを自動マウント
* https://qiita.com/Reizouko/items/276741f7fbfa02e9b7e9
 

### 5. Install software 
* chrome
* darktable

### 6. 機械学習環境の構築(失敗)
* とりあえずTensorflow(Keras)がGPUで動くようにしたい
* なるべくGUI環境でやりたいけど、面倒そうならCUIでもおｋ

いろんなバージョン
もの|バージョン|
-|-
nvidia driver|418(上記)
CUDA|10.0|
CUDNN|7.4.2.24

https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html

nvidia-dockerについて
* https://qiita.com/legokichi/items/7bf3862569cfca122d73 

#### Pythonの環境構築
Pipenv
https://pipenv-ja.readthedocs.io/ja/translate-ja/index.html

Homebrew
https://docs.brew.sh/Homebrew-on-Linux

* 基本的にはディレクトリ単位で環境を作るイメージ
* `pipenv install python=hoge`で新規作成
* `pipenv install {pakagename}`でpipみたいにパッケージをインストール
* Pipfileみたいなのに環境の設定が保存される様子
* `pipenv shell`で、インタプリンタを起動 

### 7. 機械学習環境の構築（Docker）
* https://docs.docker.com/install/linux/docker-ce/ubuntu/
* https://qiita.com/DQNEO/items/da5df074c48b012152ee 

#### nvidia-docker2
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=ubuntu18.04
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
```

## windows11
https://www.msi.com/blog/How-to-Enable-TPM-on-MSI-Motherboards-Featuring-TPM-2-0