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
5. 將輸出的 Bounding Box 反歸一化回像素與座標

## Result
在比賽中準確率 43.5973%。
- 可能原因
1. Epoch 數不足 : 因 batch size 過小，訓練圖片有1000張，需要多訓練幾個 epoch。
2. Epoch 300 - 38.4635% / Epoch 400 - 40.4969% / Epoch 500 - 43.5973%
- 訓練時遇到的問題
1. 內存不足：因為免費的colab有限制的使用量，如果在訓練網路時，參數設定的太大會直接無法訓練，
2. 相關的參數：
-batch_size：原本為64，修改為4。
-img-size：原本為608X608，修改為416X416
-epochs：訓練回合數700，最好可以是classes*2000  
也可以使用自己的anaconda進行訓練，但是很容易超出內存容量。
- 如何減少網路使用到的內存：
1. 使用比較小的網路模型：yolov7-tiny.yaml，原本用的是yolov7.yaml
2. 更改train.py的設定：在訓練的過程中，丟棄一些梯度下降時的參數，  
dist.destroy_process_group()和torch.cuda.empty_cache()
- 後續可以增加準確率的方法：
1. 增加epoch訓練次數  
2. 使用小的網路模型yolov7-tiny.yaml，試試看增加batch_size，會不會超出內存。
- confusion_matrix
只有 car 類別有被預測出來。
![image](https://github.com/ridegeart/Yolo-v7-for-object-detection/blob/main/confusion_matrix.png)
1. Background FP：為指的是不屬於任何一個類別但被檢測為其中一個類別的背景對象。所以有很多不屬於car類別的被檢測為該類。
2. Background FN：指的是檢測器錯過的Trash或Non-trash對象，並被視為其他一些背景對象。所以檢測器無法檢測出hov和person和mortocycle。
3. 訓練使用的影像大小太小(只有416X416)造成無法辨識出太小的物體，例如：人、摩托車。
- 真實標記
![image](https://github.com/ridegeart/Yolo-v7-for-object-detection/blob/main/test_batch1_labels.jpg))
- 網路預測的標記
![image](https://github.com/ridegeart/Yolo-v7-for-object-detection/blob/main/test_batch1_pred.jpg)

[yolov7]:https://github.com/WongKinYiu/yolov7
[AI CUP 2022秋季競賽，無人機飛行載具之智慧計數競賽]:https://tbrain.trendmicro.com.tw/Competitions/Details/25
[歸一化公式]:https://www.google.com/url?sa=i&url=https%3A%2F%2Fmedium.com%2Fching-i%2F%25E5%25A6%2582%25E4%25BD%2595%25E8%25BD%2589%25E6%258F%259B%25E7%2582%25BAyolo-txt%25E6%25A0%25BC%25E5%25BC%258F-f1d193736e5c&psig=AOvVaw1TKqa81z2kTNeVUdl05oAE&ust=1690982200062000&source=images&cd=vfe&opi=89978449&ved=0CBEQjRxqFwoTCKDgsN3Fu4ADFQAAAAAdAAAAABAO
