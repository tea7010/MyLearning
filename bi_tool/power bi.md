
# Dax

## Count系関数
https://qiita.com/ATL_Villas/items/850815fb8f66c47a5064

関数 | 説明
-|-
Count|値 (数値, 日付, 文字列) が入っている行数をカウント。空文字もカウントする。空白はカウントしない。ブール値はカウントできない。
CountA|Count All空白ではない行数をすべてカウント。空文字、ブール値もカウントする。
CountX|Count ExpressionCountの色々できる版。値 (数値, 日付, 文字列) が入っている行数をカウント。第1引数にテーブルを取る。(フィルターしたテーブルなどを使うのが基本)第2引数に式(列)を取る。空白はカウントしない。ブール値はカウントできない。
CountAX|Count All Expression CountAとCountXが合体したもの。
CountRows|テーブルの行数を数える。空白もカウント。
CountBlank|列の空白をカウント。空文字もカウント。
DistinctCount|重複した値は加算せずにカウント。空白と空文字は別としてカウント。
DistinctCountNoBlank|重複した値は加算せずにカウント。空白はカウントしない。空文字はカウント。