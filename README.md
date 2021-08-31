# 貓的辨識

## 環境建構
由於個人電腦執行Yolo會需要很長的時間(GTX650)，所以使用google colab。

為了在colab上使用個人的雲端硬碟，需先連上雲端，並建立短捷徑。

在登入時會需要個人授權碼，貼上後並登入。

![1](https://user-images.githubusercontent.com/64704410/131439125-305f2b0c-1150-467b-9560-95be40da0a92.png)
```
from google.colab import drive

drive.mount('/content/gdrive', force_remount=True)
!ln -fs /content/gdrive/MyDrive /app
```

在colab上執行yolo，需要先將yolo載下來，並修改設定，完成後便可編譯檔案。

```ini
# 抓取yolov4
!git clone https://github.com/AlexeyAB/darknet darknet

# 修改Darknet設定，符合Colab環境
%cd /content/darknet/
!sed -i "s/GPU=0/GPU=1/g" Makefile
!sed -i "s/CUDNN=0/CUDNN=1/g" Makefile
!sed -i "s/OPENCV=0/OPENCV=1/g" Makefile
!apt update
!apt-get install libopencv-dev

# 編譯
!make clean
!make
```
編譯完成後，將權重下載。

下載完成後，為了測試yolo是否安裝成功，進行簡單的測試。
```ini
# 抓取預先訓練完的權重
!wget https://pjreddie.com/media/files/yolov3.weights
!wget https://pjreddie.com/media/files/darknet53.conv.74
!wget https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.weights
!wget https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137

#測試
!./darknet detect cfg/yolov4.cfg yolov4.weights data/person.jpg

#展示圖片
import cv2
from google.colab.patches import cv2_imshow

img = cv2.imread("predictions.jpg", cv2.IMREAD_UNCHANGED)
cv2_imshow(img)
```
![image](https://user-images.githubusercontent.com/64704410/131439805-76bcf2ec-132c-4ad1-b7ed-c32c3a5215c0.png)

沒有問題的話，變準備將安裝好的環境製作成壓縮檔，以便往後可以快速利用。
```ini
%cd /content/
!zip -r darknet.zip darknet
!cp darknet.zip /app/darknet.zip
```

## 環境下載
在有yolo環境壓縮檔的情況下，可以直接連到雲端硬碟下載壓縮檔來使用yolo。
```ini
from google.colab import drive
import os

# 製作捷徑
drive.mount('/content/gdrive', force_remount=True)
!ln -fs /content/gdrive/MyDrive /app

#複製darknet壓縮檔
!cp /app/darknet.zip /content/darknet.zip
#解壓縮
!unzip /content/darknet.zip

%cd darknet
```

## 模型訓練

## 參考資料
