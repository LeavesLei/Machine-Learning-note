### LeNet-5
![lenet模型](http://img.mp.itc.cn/upload/20170416/b41478d96ef340289d24dc78bdffb0f1_th.jpeg)

### AlexNet
- 八个学习层(5个卷积层，3个全连接层)
- input normalization(输入归一化)
- saturate(饱和，动词)
- ReLUs don't require input normalization but local normalization aids generation 
- - [ ] Local Response Normalization：  

在应用ReLUs层后再应用这种归一化

- overlapping pooling(重叠池化)

减小step_size,而pooling_size不变

- 减少过拟合的两种方式：Data Augmentation and dropout
- Data Augmentation(数据增强)：
1.提取碎片并反转
2.改变RGB通道强度


![dropout](https://pic3.zhimg.com/v2-82f2ee2c59a843b7ca0b0abbdeaa8923_1200x500.jpg)
- dropout降低了过拟合：dropout降低的神经元之间的相互依赖关系，因此要被迫学习更为鲁棒的特征。花费时间仅需要单模型的2倍
- ![AlexNet](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539866447073&di=0d490df48a6d37ece0e222be50660894&imgtype=jpg&src=http%3A%2F%2Fimg2.imgtn.bdimg.com%2Fit%2Fu%3D487974827%2C2339616952%26fm%3D214%26gp%3D0.jpg)


#### AlexNet网络详解
---
LRN:Local Response Normalization 局部响应归一化层
C：卷积层
MP: 最大池化层
ReLU激活函数
FC：Full Connection全连接层
##### 第一个卷积层
- input：224×224×3(RGB)
- C：11×11×96，96个conventional kernel，步长为4，后跟ReLU，输出尺寸224/4=56，输出feature map为55×55×96，同时后面跟LNR层
- MX：kernel size = 3 × 3，feature map = 27×27×96
- output：27×27×96

##### 第二个卷积层
- input: 27×27×96
- C：5×5×256，step_size=1,后接ReLu和LRN层
- MP：kernel大小3×3，step_size=2,因此feature map大小：13×13×256
- output：13×13×256

##### 第三层至第五层卷积层
- input：13×13×256
- 第三层C：3×3×384，步长为1，加上ReLU
- 第四层C：3×3×384，步长为1，加上ReLU
- 第五层C: 3×3×256，步长为1，加上ReLU
- 第五层后跟MP，kernel大小3×3，步长为2
- output：6×6×256

##### 第六层到第八层为全连接层
- FC：4096 + ReLU
- FC: 4096 + ReLU
- FC: 4096 + ReLU
- 最后一层为softmax为1000类的概率值

#### AlexNet中的一些tricks(技巧)
- ReLU：  
使用ReLU代替Sigmoid，使其能更快的训练，同时解决sigmoid在训练较深的网络中出现**梯度消失**(sigmoid导数最大值0.25，从而导致在远离输出层的神经元，学习速率特别慢)
- Dropout：随机忽略一些神经元，避免过拟合。(神来之笔，远离说不清，道不明，但就是有用)。训练时间仅仅是初始的两倍
- overlapping pooling
- 提出了LRN层
- data augmentation