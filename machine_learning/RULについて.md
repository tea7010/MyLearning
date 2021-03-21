# RULについて調べた内容まとめ

###### tags: `computer`

論文とか読んで"Remaining Useful Life"について調べた結果をまとめる

## Remaining useful life estimation – A review on the statistical data
http://or.nsfc.gov.cn/bitstream/00001903-5/93202/1/1000004637516.pdf

### アブスト
いろんな研究がさかんにされているが、universallyに使えるベストなアプローチはいまのところない
このreviewでは統計的なデータドリブンアプローチについて注目する。
アプローチは２種類に分類することができ、"directly"に観測状態情報が得られている場合と、いない場合。

* このobserved state infomationとは、RULをしたい対象が直接観測されたデータがあるかどうかでobservedとなる
* たとえば、RUL対象の部品の亀裂や摩耗が直接観測されているとobserved CM、間接的にオイルや水温などしか観測できないばあいはindirect CMとみなす様子
* ただしいい感じのindirectでも特徴量やいい観測値があれば、directと同様に扱えるようになるみたい
* directlyであれば閾値とかで回帰予測モデルベースとかでRUL予測ができるので、それができると信じて回帰のアプローチもありかも
* まずアプローチとしてはシミュレーションベースと、データドリブンベースかが違うとisidのセミナーで聞いたことある

データの状態|意味|例
directly observed|RULに直接的に影響するであろう変数が観測されていること|構造物の亀裂、摩耗の量など
indirect observed|RULに間接的であろう変数が観測されていること|オイルの温度や圧力など

### 3
directlyに観測されたCMdataがあり、いいモデルが作れれば壊れるデータがほとんどない状態でもRUL予測が可能になる
他にも閾値ベースで簡単なRUL予測が可能になる。それはRULを決める必要がある。実際閾値ベースだと、熟練の経験や過去のデータでの決め打ちがよく行われる。ここではモデルベースを取り上げますね。モデルベースでもcontinues process、discrete stateの２種類に分けられる。cotinuousだとreggression, Brownina motion, Gamma processがある。discrete stateだとMarkov chain-basedになるそう

#### 3-1
よく使う回帰使うみたい。RULに確率分布を仮定しないので、PDFが利用できないというかいらない？
random coefficien regressionでRULを表すと、Y(t)を観測される劣化の指標・DをCMとしてとしてy = D(t) + e(t)みたいな感じ
このDのなかに個体差やランダム項などを表現するみたい
t_iにおけるRULはX_t_i = {x_t_i: D(t_i + x_t_i: hoge) >= w|D(t_i:hoge) < w}
x_t_iの集合それは予測の集合で、t_i+x+tiがある値以上であるような、その値はtiにおける予測がwよりも下回るようなw
結局モデルの予測がある決めたwに対して、残りどれだけかをRULと定義するっぽい。
ランダム係数に対しては仮定をおく、1. 時間に沿って指標は悪化し、悪化が可視化できること 2. 個体差が激しくないこと（悪化は他のデバイスも一緒であること） 3. デバイス間のランダムネスの分布が明らかであること
悪化はデバイス間で独立である例で扱っている論文もある

* random coefficient regression <- ランダムエフェクトのある回帰のこと？

#### 3-2
wiener processは回帰モデルのひとつである。しかしモデルにブラウン過程があることが違いとしてある。