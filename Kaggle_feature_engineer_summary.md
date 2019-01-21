### Kaggle特征处理

- EDA
  - 看每个特征的分布(value_counts)，分析其与label之间的关系
  - 清除unique data
  - fillna
  - date转换成 year month day weekday isWeekends
  - Unix时间戳转换成hour
  - 类型转换To int
  - 对于一些长字符、多类的feature，可以考虑使用length、depth、complexity来进行特征提取
  - 对于含有date的数据，求previous interval与next interval很有必要
  - 简单特征交叉，NMI高(大约0.03以上)的交叉特征投入使用
  - 投入lightGBM模型后设置category变量
  - drop list, category list, crossing feature list 列出来，便于增减

- 必要模块
  - 缺失值填充模块
  - 贝叶斯模块
  - 日期转换模块
  - remove模块(丢掉某个feature)
  - 类型转换模块(toInt64)
  - length、depth、complexity转换模块
  - groupby, merge模块(根据xx(feature)使用xx(count,sum)来group byxx(feature))
  - feature-label NMI计算模块
  - feature crossing模块
- 特征顺序
  - 按最大difference排序，看哪些feature最容易引起错误。
  - pipe line

- 注意事项

  - discussion区内其他人踩雷的点

  - 是否有人or official document对每个feature做仔细分析

  - value_counts以及plot看看每个feature的分布情况，初步观察哪些feature对于label影响较大

  - 看有没有拒绝类特征(bounces)

  - 对于每个feature进行分析后要知道如何去使用：

    - 取log
    - 切箱

  - 分析一个feature就记录一个feature

  - 分析两个or多个feature之间的逻辑关系(eg: hits and pageviews)

  - 每次添加feature后对最后结果影响，记录下来
