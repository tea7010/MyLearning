## アニメーションプロット
* https://qiita.com/osanshouo/items/3c66781f41884694838b

## 衛星画像を重ねる方法
1. 座標の最大最小をまず計算して、それをプロットの上限下限にしておく。
```python
fig, ax = plt.subplots()

# Axis setting
ax.set_xlim((x_min, x_max))
ax.set_ylim((y_min, y_max))
```
 

2. https://docs.mapbox.com/playground/static/ にアクセスして、座標の四隅の値を入力。

3. 画像を保存

4. imshowのレイヤーを0にして、その上から重ね合わせる
```python
img_path = 'satellite.png'
my_map = plt.imread(img_path)
ax.imshow(my_map, zorder=0, extent=(x_min, x_max, y_min, y_max), aspect='equal')
```