# yolov5-fall

运行步骤：

1、数据集处理

  将百度网盘上下载来的图片放入mydata/images文件夹中,标注labels放入mydata/labels文件夹内
  
  labels文件夹内的标注是yolo使用的txt格式，但是由于平时我使用的都是xml格式，然后运行mydata文件夹中的split_train_val.py进行数据集划分，运行voc_label.py生成训练预测测试集的txt文件用于mydata.yaml读取
  
  所以在重现的时候要麻烦亲自编写划分数据集与生成路径文件的代码了
  
  mydata.yaml内容如下:
  
  {
  
  train: mydata/img_lab/train.txt # train.txt内的是训练集图片的路径,最好是绝对路径,linux系统也最好从根目录开始写,可能是我的操作问题,相对路径没跑起来过
  
  val: mydata/img_lab/val.txt
  
  test: mydata/img_lab/test.txt
  
  nc: 3  # 修改为自己类别
  
  names: ['stand', 'sit', 'fall']# class name
  
  }
  
  train.txt格式如下：
  
  {
  
  /root/fall2222/mydata/images/people(1).png
  
  /root/fall2222/mydata/images/people(10).png
  
  /root/fall2222/mydata/images/people(100).png
  
  /root/fall2222/mydata/images/people(1000).png
  
  /root/fall2222/mydata/images/people(1002).png
  
  /root/fall2222/mydata/images/people(1006).png
  
  /root/fall2222/mydata/images/people(1007).png
  
  /root/fall2222/mydata/images/people(1008).png
  
  /root/fall2222/mydata/images/people(1009).png
  
  /root/fall2222/mydata/images/people(101).png
  
  /root/fall2222/mydata/images/people(1010).png
  
  /root/fall2222/mydata/images/people(1012).png
  
  /root/fall2222/mydata/images/people(1019).png
  
  ...
  
  }
  
2、修改train.py的配置文件

  找到train.py下的修改参数部分主要修改:
  
  1)修改--weights,需要去yolo官方项目下载权重yolov5m等,新建weights文件夹放入其中,即效果为:
  
  parser.add_argument('--weights', type=str, default='weights/yolov5m.pt')
  
  2)parser.add_argument('--cfg', type=str, default='models/yolov5m.yaml')
  
  3)parser.add_argument('--data', type=str, default='mydata/mydata.yaml')
  
  4)parser.add_argument('--epochs', type=int, default=70)
  
  5)parser.add_argument('--batch-size', type=int, default=4)
  
 3、运行train.py
 
  终端输入python train.py
  
 4、运行完后即在runs文件夹内产生结果,内的weights存储best与last的pt权重
 
 5、pt权重转onnx
 
  首先要下载onnx库
  
  在终端输入python export.py --weights best.pt --include onnx --img 640 --batch 1
  
 6、使用onnx进行预测
 
  使用test3.py可以对图片进行预测并画出框
  
  test3.py只是一个轻量的预测代码，一次只能对一张图片预测，平台上的代码在此基础上可批量预测
 
  
  
  
  
  
  
