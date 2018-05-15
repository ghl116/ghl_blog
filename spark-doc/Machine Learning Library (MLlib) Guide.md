# Machine Learning Library (MLlib) Guide
mlib是spark的机器学习库，目标是为了使机器学习实践简单可扩展。更高级别，它提供了：

- 机器学法算法：常见的机器学习算法如分类，聚合，集群，过滤
- 特色功能：特色抽取，转换，降维和选择
- 管道处理：构造，评估和优化机器学习管道工具
- 持久化：保存和加载算法，模型和管理
- 工具：线性代数，数据，数据处理等


##注意：基于DataFram api是主要的Api

MLib的基于RDD api现在进入维护模式。

为什么Mlib选择基于Dataframe的API

- DataFrame比RDD提供了更加用户友好的Api。Dataframe对于spark数据源，sql查询，Tungsten and Catalyst optimizations，跨语言的统一Api。
- Mlib 提供了统一的API跨算法及语言
- DataFrame加快了实际的ML 管理处理，特色转换。


# 基本统计

##相关性
计算两类数据的相关性是数据统计中的常见操作。在spark.ml中，我们提供了弹性机制来计算多个序列中的相关性。支持的相关性方法目前是Pearson’s and Spearman’s.

## 假设验证

假设验证在数据统计中是一个有效的工具来判断是一个结果是否正确，无论这个结果是否偶然发生。spark.ml目前支持 Pearson’s Chi-squared ( χ2) 的依赖验证。

ChiSquareTest为每个标签特征进行Pearson’s依赖验证。对于每个特征，(feature, label) 转换成矩阵Chi-squared 可计算的。所有




































