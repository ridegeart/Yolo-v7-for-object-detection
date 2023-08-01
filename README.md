# Yolov7 for image object detection
An Custom Dataset implement for [yolov7]. Dataset from [AI CUP 2022秋季競賽，無人機飛行載具之智慧計數競賽]。

## Data Loader
主辦方提供的資料及內含:
1. 圖片檔 (.jpg)。
2. .txt 檔：紀錄 Bounding Box 與圖片類別的資訊。

### Bounding boxes
各類別的邊界框的座標，格式為: 類別 (人、摩托車、車、貨車) , x, y, w, h，
- x, y : 代表該 Bounding boxes 的中心座標。
- w, h : 代表該 Bounding boxes 的寬高。
- boundingBox 座標原點為左上角。
- xmin,ymin(x1, y1) : boundingBox 的左上角的座標；xmax,ymax(x1, y1) : boundingBox 的右下角的座標。
- bw,bh : boundingBox 的寬與高。
- 歸一化 Bounding boxes 座標 : w,h是照片本身的寬與高。 [歸一化公式]

## Training Setting
- flight.yaml : 訓練網路時需要的圖片路徑與類別名字。

## Training
1. weights：使用/content/yolov7/last.pt(接續上次訓練的最後一個epoch的權重(last)，或者上次訓練表現最好的權重檔(best)，也可以使用yolo v7作者訓練好的權重)
2. data：設定檔為flight.yaml(根據自己使用的資料集的類別與存放圖片的地址做更改)
3. epochs：700
4. batch-size：4
5. img-size：416X416
6. project：訓練完成的權重存放的地址

## Detect
將訓練好的網路對測試資料做預測，並輸出每張圖片 Bounding Box 的標記檔。
1. weights：訓練好的網路權重
2. source：圖片路徑
3. img-size：640*640
4. project：輸出後的圖片與標記檔存放的位置  
會輸出1.帶有 Bounding Box 標記好各類物件的圖片 2.每張圖片的 Bounding Box 的標記檔

[yolov7]:https://github.com/WongKinYiu/yolov7
[AI CUP 2022秋季競賽，無人機飛行載具之智慧計數競賽]:https://tbrain.trendmicro.com.tw/Competitions/Details/25
[歸一化公式]:https://www.google.com/url?sa=i&url=https%3A%2F%2Fmedium.com%2Fching-i%2F%25E5%25A6%2582%25E4%25BD%2595%25E8%25BD%2589%25E6%258F%259B%25E7%2582%25BAyolo-txt%25E6%25A0%25BC%25E5%25BC%258F-f1d193736e5c&psig=AOvVaw1TKqa81z2kTNeVUdl05oAE&ust=1690982200062000&source=images&cd=vfe&opi=89978449&ved=0CBEQjRxqFwoTCKDgsN3Fu4ADFQAAAAAdAAAAABAO
