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

  
[yolov7]:https://github.com/WongKinYiu/yolov7
[AI CUP 2022秋季競賽，無人機飛行載具之智慧計數競賽]:https://tbrain.trendmicro.com.tw/Competitions/Details/25
[歸一化公式]:https://www.google.com/url?sa=i&url=https%3A%2F%2Fmedium.com%2Fching-i%2F%25E5%25A6%2582%25E4%25BD%2595%25E8%25BD%2589%25E6%258F%259B%25E7%2582%25BAyolo-txt%25E6%25A0%25BC%25E5%25BC%258F-f1d193736e5c&psig=AOvVaw1TKqa81z2kTNeVUdl05oAE&ust=1690982200062000&source=images&cd=vfe&opi=89978449&ved=0CBEQjRxqFwoTCKDgsN3Fu4ADFQAAAAAdAAAAABAE
