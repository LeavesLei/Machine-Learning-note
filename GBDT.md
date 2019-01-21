### GBDT(Gradient Boosting Decision Tree)

---

#### Regression Decision Tree(回归决策树)与Classification Decision Tree的区别

- 回归树的每个节点得到的是一个预测值而非分类树式的样本计数

  	for example：假如在某一棵树的某一节点使用了年龄进行分支，那么这个预测值就是属于这个节点的所有人年龄的平均值

-  **WRONG**！！！（ 在分节点的选取上，回归树并没有选用最大熵值来作为划分标准，而是使用了 **MSE**）**WRONG**！！！



#### 梯度提升(Gradient Boosting)

- GB本身是一种理念而非一个具体的算法，基本思想：沿着梯度方向，构造一系列的弱分类器函数，并以一定权重组合起来，形成最终决策的强分类器。
- 每一棵树所学习的是之前所有树结论和的残差，这个残差就是一个加预测值后能得到真实值的累加量。

#### GBDT的优点

为什么不适用负梯度调节而使用多分类器拟合？

​	多分类器拟合更具有泛化能力。

[GBDT具体步骤详细说明](https://blog.csdn.net/google19890102/article/details/51746402/)

[GBDT简单实例](https://github.com/liudragonfly/GBDT)

### XGBoost(eXtreme Gradient Boost) 

- XGBoost训练了很多棵DT，最后根据每个输入的得分和（sum score）来回归。

[XGBoost论文翻译](https://blog.csdn.net/u014411730/article/details/78796890)

---

- XGBoost的正则化目标

    DT的集成模型（多个模型相加的过程）使用K个相加函数预测结果



### 一些GBDT和XDBoost的资料

---

[GBDT算法原理以及实例理解](https://blog.csdn.net/zpalyq110/article/details/79527653)

[GBDT详解](https://www.cnblogs.com/peizhe123/p/5086128.html)

[XGBoost入门以及实战](https://blog.csdn.net/sb19931201/article/details/52557382)


- [ ] 算法伪代码还是不太懂
- [XGBoost入门以及实战](https://blog.csdn.net/sb19931201/article/details/52557382)
- GBDT例子找一个，看是否跟自己的理解一样
- [ ] LightGBT 牺牲精度换区速度