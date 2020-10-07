# 動画像の入出力
ここではファイルやカメラからの動画像入力、動画像の表示、動画像の保存の方法について解説します。

OpenCV (C++) では、画像を `Mat` 型として表現していましたが、
OpenCV (Python) では、動画像を NumPy 配列として扱います。
NumPy 配列は各種演算が高速にかつ簡単に行えるため、初心者にも扱いやすいと言えるでしょう。

# 入力
## 画像ファイルを入力
- `cv2.imread`: 保存されている静止画ファイルを画像データとして読み出す。
  - 引数 `str`: 読み出す静止画ファイル名
  - 戻り値 `ndarray`: 読み出した画像データ
  
```python
img = cv2.imread("in.jpg")
```

## 動画ファイルを入力
```python
cap = cv2.VideoCapture("../sample.mp4")
while(True):
   _, img = cap.read()
```

## カメラ画像を入力
```python
cap = cv2.VideoCapture(0) # 0 番目のカメラ
_, img = cap.read()       # カメラから 1 フレーム取得
cap.release()             # カメラの開放
```

## カメラ動画を入力
- VideoCaptureクラス
```python
cap = cv2.VideoCapture(0)
while(True):
   _, img = cap.read()
```

# 出力
## ウィンドウに表示
- `cv2.imshow`：画像を適当なウィンドウに表示する。
  - 第1引数 `str`: 画像を表示したいウィンドウ名
  - 第2引数 `ndarray`: 表示したい画像データ
- `cv2.waitKey`: `cv2.imshow` で開いたウィンドウがアクティブな状態のとき指定時間だけキー入力を待ち、そのキーコードを返す。
  - 引数 `int`: キー入力を待つ時間 (ms)
  - 戻り値 `int`: キーコード (キーが押されたとき) or NULL (押されなかったとき)

```python
while(True):
    cv2.imshow("Window Name", img) # 画像の表示
    
    key = cv2.waitKey(1)
    if key == ord("q"):            # q で表示終了
        break
cv2.destroyAllWindows()            # ウィンドウ破棄
```

## 画像ファイルへ出力
- `cv2.imwrite`：画像データを静止画ファイルとして保存する。
  - 第1引数 `str`: 保存したい静止画ファイル名
  - 第2引数 `ndarray`: 保存したい画像データ
  
```python
cv2.imwrite("out.jpg", img)
```


## 動画ファイルへ出力
- VideoWriterクラス: ビデオライタを表現するためのクラス。
  - 第1引数 (文字列): 書き込み先の動画ファイル名
  - 第2引数 (特定記号): 利用するビデオエンコーダの種類
    - CV_FOURCC_MACRO('M', 'J', 'P', 'G')：DIVXエンコーダ
    - CV_FOURCC_PROMPT：その環境で利用可能なエンコーダを選択可能なメニューの表示
  - 第3引数 (double型) ：フレームレート (ビデオレートにする場合は30) 
  - 第4引数(Sizeクラス) ：動画の各フレームの画像サイズ
  - 第5引数(bool型)：書き込む動画像がカラーか否か
    → true (カラーのとき) 、false (1チャネルの濃淡画像のとき) 
```python
writer = cv2.VideoWriter("out.avi")
while(True):
   writer.write(img)
```
