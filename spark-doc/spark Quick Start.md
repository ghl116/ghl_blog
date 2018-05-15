# spark Quick Start

本文提供了快速指导关于使用spark。首先我们介绍了spark shell的Api，然后说明如何使用Java, Scala, and Python开发应用。

首先下载spark发行版本，由于我们不会使用hdfs，所以你可以下载一个任何hadoop版本的包。

注：在spark2.0之前，spark的主程序是Resilient Distributed Dataset (RDD)。在spark2.0之后，rdd被 dataset替代，dataset是一个很像RDD，但是在框架下已经进行了大量的优化。RDD接口仍然支持，而且可以从 RDD programming guide 中得到完整的说明。然而，我们强烈建议你可以使用dataset，会比使用rdd有一个更好的性能。从SQL programming guide 可以得到更多关于dataset的信息。

#使用spark shell进行交互式分析
##基础
spark shell提供了一个简单的方式学习API，同时提供了有力的工具进行交互式分析数据。

spark的主要抽像接口是一个分布式的数据集合叫Dataset。dataset可以从hadoop的输入（如hdfs文件）中生成，或者其他数据集。由于Python的动态特性，我们不需要在python进行强类型定义。因此，在python中定义的所有Dataset是Dataset[Row]。我们叫她DataFrame，这和pandas以及R中的数据框架一致。

##独立的应用程序 Self-Contained Applications

## Where to Go from Here

- 深度了解API，start with the RDD programming guide and the SQL programming guide, or see “Programming Guides” menu for other components.
- 在集群上运行集群，查看deployment overview.
- 最后，在examples目录包含几个样例，
























