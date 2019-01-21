### 参考资料
[知乎LightGMB讲解](https://zhuanlan.zhihu.com/p/36914194)

[LightGBM中文文档](http://lightgbm.apachecn.org/cn/latest/Parameters.html#id5)
### 特征分桶

--- 

### 树的生长方式
- XGBoost和GBDT采用Levelwise的生长方式，对当前层的所有叶子节点均进行分裂
- LightGBM采用的是Leafwise的树生长方式。计算当前树的所有叶子的分裂，选择最大分裂增益的叶子进行分裂。
![分裂方式对比图](https://pic3.zhimg.com/80/v2-fba4bc4d4d094c7b440825264ff8f9be_hd.jpg)

### category特征处理
LightGBM可以直接使用category特征，无需对其进行one hot 编码。

### LightGBM对于NA的处理
LIghtGBM可以自动处理缺失值，在计算分裂增益的过程中，当前特征为缺失值的样本先放到左叶子计算分裂增益，然后再放到右叶子计算分裂增益，从中选择增益更大的方式进行分裂。从而得到缺失值是往左或者往右分裂。

### parameter
- max_depth, default=-1限制树的模型的最大深度，<0则代表没有限制
- min_data_in_leaf, defaut=20 一个叶子上数据的最小数量，可以用来处理过拟合
- - [ ] min_sum=hessian_in_leaf,defaut=1e-3,一个叶子上的最小hessian和，处理过拟合
- feature_fraction, defaut=1.0, LightGBM会在每次迭代中随机选择部分特征。例如如果设成0.8，将会在每棵树训练之前选择80%的特征。 可以用来加速训练 ＆ 处理过拟合
- feature_fraction_seed, default=2, 随机数种子
- bagging_fracttion_seed, bagging_freq, bagging_seed
- - [ ] early_stopping_round, default=0. 如果一个验证集在early_stoping_round循环中没有提升，将停止训练
- - [ ] lambda_l1, lambda_l2
- min_split_gain, default=0 执行切分的最小增益
- 使用GBDT，不使用dart、goss
- min_data_per_group, default=100 每个分类组的最小数据量
- max_cat_threshold, default=32, 用于分类特征，限制分类特征的最大阈值
- cat_smooth, default=10，降低噪声在分类特征中的影响，尤其是对数据很少的类别
- cat_l2 分类切分中的L2正则
- max_cat_to_onehot, default=4, 当一个feature的类别数小于or等于max_cat_to_onehot时，one-vs-other切分算法将会被使用
- top_k, default=20, 将它设置为更大的值可以获得更精确的结果，但是会减慢训练速度。


### 调参步骤
1. 选定learning_rate = 0.1 , boosting_type = gbdt
2. max_depth, 过大 可能会过拟合；num_leaves < 2^(max_depth),引入sklearn中的GridSearchCV()来精调
3. min_data_in_leaf 和 min_sum_hessian_in_leaf , 设置较大可以避免生成一个过深的树，但有可能导致欠拟合
4. feature_fraction 和 bagging_fraction 降低过拟合
5. 正则化参数lambda_l1, lambda_l2
6. 再次降低learning_rate

### speed up
1. use bagging by **feature_fraction** **bagging_fraction** 
2. use small **max_bin**
3. use **save_binary**(if Ture, save file as binary)

### precision
1. large **max_bin**
2. small **learning_rate** with large **num_iterations**
3. large **num_leaves**(over fitting)
4. try **dart**???

### handle over fitting
1. small **max_bin**
2. small **num_leaves**
3. setting **min_data_in_leaf** and **min_sum_hessian_in_leaf**(important)
4. use bag by setting **feature_feaction** and **bagging_fraction**
5. **lambda_l1** **lambda_l2** **min_gain_to_split**
6. **max_depth**

### about missing value
- use_massing = false means droping NA
- zero_as_missing = true can transfer NA to zero. Be careful: if you set zero_as_missing = true, both NA and 0 are consider as missing value.

### note
- 结果复现：gpu_use_dp = true
- num_leaves 与 max_depth的关系
- min_data_in_leaf 是处理过拟合的较为重要的parameter