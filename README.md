# Helmet-Detection
华为智慧工地-安全帽检测

## 介绍

A Keras implementation of YOLOv3 (Tensorflow backend)，参考：https://github.com/DataXujing/YOLO-V3-Tensorflow 


---
安全帽检测系统通过深度学习的目标检测模型来对图片中出现的安全帽和人头部进行检测，可以近似看作是分类为'hat'和'person'的目标检测问题。

## 环境
    - Python 3.5.2
    - Keras 2.1.5
    - tensorflow 1.6.0

## 数据集
本项目使用开源的安全帽检测数据集[SafetyHelmetWearing-Dataset, SHWD](https://github.com/njvisionpower/Safety-Helmet-Wearing-Dataset)主要通过爬虫拿到，总共有7581张图像，包含9044个佩戴安全帽的bounding box（正类），以及111514个未佩戴安全帽的bounding box(负类)，所有的图像用labelimg标注出目标区域及类别。其中每个bounding box的标签：“hat”表示佩戴安全帽，“person”表示普通未佩戴的行人头部区域的bounding box。标注使用xml形式。
坑：读入图片时会报错没有该图片，因为部分图片扩展名为JPG，需提前检测文件是否存在，不存在则将扩展名改为JPG  

## 使用
1. 使用训练好的权重trained_weights_stage_1.h5
2. 运行检查部分
```
python yolo_video.py [OPTIONS...] --image, for image detection mode, OR
python yolo_video.py [video_path] [output_path (optional)]
```
## 训练
1. 下载YOLOv3权重[YOLO website](http://pjreddie.com/darknet/yolo/).
2. 将权重转换为keras格式
```
python convert.py yolov3.cfg yolov3.weights model_data/yolo.h5
```

3. 生成自己的注释文件和类名称文件。
一张图片一排；
行格式：image_file_path box1 box2 ... boxN;
框格式：x_min,y_min,x_max,y_max,class_id（无空格）。
对于xml类型标注，请尝试python voc_annotation.py
以下示例：
```
path/to/img1.jpg 50,100,150,200,0 30,50,200,120,3
path/to/img2.jpg 120,300,250,600,2
..
```

4. 修改train.py并开始训练
```
python train.py --model model_file。 
```
5. 测试 
python yolo_video.py，输入图片路径，可获取检测图片。
yolo.py 中detect_image函数中可以获取图片中的类别及坐标（集成到项目中会用）

6. input文件夹+mAP.py+scripts文件夹 计算mAP，参考：https://github.com/Cartucho/mAP
## 效果
方案效果：目前34.74% = 0 AP ，82.76% = 1 AP ，mAP = 58.75%
本机电脑下FPS=2，一个GPU下FPS=25

