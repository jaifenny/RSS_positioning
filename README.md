# RSS positioning

## :bulb: Introduce

- 實作 LBS，使用 RSS 訊號地圖的定位方法。
    - Location-based services(LBS) 是一種支援實體位置追蹤功能的技術，此項技術不斷地標誌使用者的地理位置，並提供使用者相關服務與功能。
    - 根據接收到的Wi-Fi 訊號強度(RSS)進行距離的測量。
    - 1NN、3NN、5NN 比對方法，計算定位錯誤率。並在 google map 上找出預測的座標點。

## :small_blue_diamond: Method
- 從網路上下載 UJIIndoorLoc dataset，免除 offline 資料收集步驟。
https://www.kaggle.com/datasets/giantuji/UjiIndoorLoc https://archive.ics.uci.edu/dataset/310/ujiindoorloc
- TrainingData.csv: 用來訓練之 radio database
- ValidationData.csv: 用來驗證效果之 radio database
- 使用 Building 1 Floor 1 的資料做訓練與測試。
- 撰寫程式從 TrainingData.csv 讀入 radio database，以自訂的資料結構儲存。
- 採用 **Euclidean distance** 作為 distance 計算之演算法，撰寫對應之程式。某筆資料中未偵測到的 AP 訊號可定為 100 或是-104。
- 讀入 ValiationData.csv，一筆資料做一次測試，將新的 fingerprint 與 radio database 的每項計算 distance，以距離最近的 fingerprint 其代表的位置作為預估的位置。**計算真實座標與預估座標的誤差**。最後算出 ValidationData 中的各自距離誤差與平均值，並繪製成直方圖。
- 重複步驟，改採用 KNN (K = 3 或 5)的方法，挑選最近 K 筆資料將其 K 個座標值平均作為預估位置。
- 用提供的 plot.cvs 的三組 fingerprint，分別用最準確的方式預估位置，並截圖在 google map 上的位置。
- 座標轉換: https://epsg.io/transform#s_srs=4326&t_srs=3857&x=NaN&y=NaN

## :small_blue_diamond: Result

### :small_orange_diamond:K＝1
- 誤差平均值：14.8376 

### :small_orange_diamond:K＝3
- 誤差平均值：14.8235

### :small_orange_diamond:K＝5
- 誤差平均值：12.5110
- K＝5時，所得的誤差平均值最小。

### :small_orange_diamond:Plot
- 採計K＝5
1. 第一筆在plot.csv中的fingerprint 在Google Map上的位置：轉換前座標：(X : -7461.3872, Y : 4864813.137800001)
![](https://github.com/jaifenny/RSS_positioning/blob/main/picture/1.jpg)

2. B.	第二筆在plot.csv中的fingerprint 在Google Map上的位置：轉換前座標：(X : -7534.067063200001, Y : 4864850.5098)
![](https://github.com/jaifenny/RSS_positioning/blob/main/picture/2.jpg)

3. C.	第三筆在plot.csv中的fingerprint 在Google Map上的位置：轉換前座標：(X : -7417.0628046, Y : 4864882.9088)
![](https://github.com/jaifenny/RSS_positioning/blob/main/picture/3.jpg)

          
