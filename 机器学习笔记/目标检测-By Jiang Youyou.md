
### 关于目标检测的各版本
![image](https://note.youdao.com/yws/api/personal/file/WEB399e47e3e250fbe6df027e6e6e885dbf?method=download&shareKey=62195eab0a5b2bdce04974bc9b33c222)

- R-CNN 候选区域窗+深度学习分类;
- SPP Net
- Fast RCNN

### 基础:RCNN,分为四步实现目标检测:

![image](https://note.youdao.com/yws/api/personal/file/WEB81bc3b4eead8ae30c10921dcf84dcc38?method=download&shareKey=d980fc7e029e0c39b774bd585e878cbe)

- 生成1000+个候选区域
  - 将图像分成小区域,合并相似性高的区域
    - 相似性高:颜色相近,纹理相近
  - 输出所有曾经出现过的区域
  - 候选区域有很多方法:Selective Search(本文), objectness, Category independent object proposals, CPMC。
- 特征提取:
  - 所有候选区域归一化尺寸,227*227;
  - 提取fc7层4096维特征作为模型的输入;
- 分类:
  - 输入4096维特征
  - 输出是否属于此类
  - 由于负样本很多,实用 hard negative mining
  - 模型使用linear SVM
- 位置精修(object proposal refinement)
  - 对于xy方向的平移和缩放
  - Bounding Box Regression
- 参考文献
  - https://pdfs.semanticscholar.org/6eda/ac91a96c0adc9b6108f1a6ba8adfc1c7267d.pdf

##### 边框回归
- https://blog.csdn.net/zijin0802034/article/details/77685438
- 边框回归的目的既是：给定(Px,Py,Pw,Ph)(Px,Py,Pw,Ph)寻找一种映射ff， 使得f(Px,Py,Pw,Ph)=(Gx^,Gy^,Gw^,Gh^)f(Px,Py,Pw,Ph)=(Gx^,Gy^,Gw^,Gh^) 并且(Gx^,Gy^,Gw^,Gh^)≈(Gx,Gy,Gw,Gh)

### 升级版1: fast rcnn
同样分为四步:
- 建议候选框
- 特征提取
- 分类
- 位置调整

![image](https://note.youdao.com/yws/api/personal/file/WEBab453b21a0d829d407b1eb22879fd88f?method=download&shareKey=7c4056d6cfc42774dfc1a38a3f4ae4fb)

##### fast rcnn在 rcnn基础上进行了如下修改:
- 将整张图像归一化之后直接送入深度网络,即 roi—pooling
- 从整张图像中提取候选区域,
这些候选区域在前几层特征计算中不需要;
- 加入多任务损失函数
- fast rcnn主要在rcnn基础上采用了spp net的方法;

![image](https://note.youdao.com/yws/api/personal/file/WEBfe3b11f259a84d396f9d02305d778fd9?method=download&shareKey=e8ef6d2b7c3203027b9b127ada981362)

##### 网络结构
- 前五个阶段是conv+relu+pooling形式,在第五个阶段结尾处,输入P个候选区域(p*5,序号+位置);
  - 候选区域来源于rcnn相同的方法;
- 在第五层之后加一个roi_pool层, 将每个候选区域分成M*N块, 对每块进行max pooling, 这样将大小不一的候选区域变为大小统一的数据, 然后送入下一层;
  - roi_pooling是可微的

```
graph LR
A(roi,5,P,1+4)-->B(roi_pool)
B-->C(conv map,6_6_256_P)
```

##### 各阶段
- 候选区域: 来自于和rcnn相同的方法, 但是需要向feature map上映射;
- 特征提取: 使用roi_pool之后的conv map作为输入
- 分类: 多任务网络
- 位置调整: 多任务网络
- 多任务网络:
  - 具体见代价函数
```
graph LR
A(feature,4096 P)-->B(cls_score)
A-->C(bbox_predict)
B-->D(cls_score,K+1,P)
C-->E(bbox_predict,K+1,4,P)
```

![image](https://note.youdao.com/yws/api/personal/file/WEBc9c51dd4c642b3f273da16fb11fcae5e?method=download&shareKey=1cfe70ffd614534f3471d27b67aa911b)

##### 其他
- 全连接层提速
  - 利用了SVD分解

### 再升级版 faster rcnn
faster rcnn在 fast rcnn的基础上将候选区域生成使用RPN(region proposal network)

faster rcnn可以认为是由RPN具有共享卷积层的快速RCNN组成的统一网络.

##### 特征区域
- 在feature map上滑动窗口
- 建立一个神经网络用于物体分类+边框回归
- 滑动窗口提供了物体的大体位置信息
- 边框回归可以提供更精确的位置
比如某个卷积层特征map有`$W*H$`个点, 使用3*3的横纵比例变换,总共就有9个锚点。

![image](https://note.youdao.com/yws/api/personal/file/WEBdb98f19d5437e47fdfbf167b6542e477?method=download&shareKey=d3129a0410c5f06a40c3a9a0c5bf9751)

##### 区域生成网络和训练
- 样本:需要对于正负样本进行限定
- 代价函数
  - `$L_{cls}$` 判断9个anchor属于前景和背景的概率
  - `$L_{reg}$` 对应窗口应该平移缩放的参数
  - 同时最小化两种代价
```math
L(\{p_i\},\{t_i\})=\frac{1}{N_{cls}}\sum_{i}L(p_i,p_i^*)+\lambda\frac{1}{N_{reg}}\sum_ip_i^*L_{reg}(t_i, t_i^*)
```

![image](https://note.youdao.com/yws/api/personal/file/WEB49451ea67f17a66ac75b2e20cbbf17c3?method=download&shareKey=8319f9885cdd8d88123652d5396fdd37)
- 将整张图片输进CNN, 得到feature map;
- 卷积特征输入到RPN, 得到候选框的特征信息;
- 对候选框中提取出的特征, 使用分类器判别是否属于一个特定类;
- 对于属于某一个类别的候选框, 用回归器进一步调整其位置.


- 联合训练
  - 交替训练;
  - 近似联合训练;
  - 非近似联合训练;
  - 4-step交替训练;

### Yolo(cvpr2016)


### rcnn各版本的不同

模型 | rcnn | fast rcnn | faster rcnn
---|---|---|---
roi指什么 | selective search的结果 | 特征图上的映射 | RPN产生再映射到特征图上
结构 | 完全分离 |selective search独立| 联合训练|
![image](https://note.youdao.com/yws/api/personal/file/WEB9942601344183bbf2df250cc2c789df9?method=download&shareKey=5778b2c0200167eb94836eaed0f7997b)
