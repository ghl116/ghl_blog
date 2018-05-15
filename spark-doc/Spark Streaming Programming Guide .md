# 概览

spark streaming是spark 核心api的一个扩展，实现了可扩展，高吞吐容错的流式数据处理。数据可以从许多源如，kafka，Flume，kinesis或者TCP socket从抽取，并且可以使用高级别的函数map, reduce, join and window来进行处理。最终：处理后的数据可以保存到文件系统，数据库以及直播大屏。事实上，你可以应用spark的机器学习以及图处理算法到数据流中。

spark streaming提供的抽像称为离散流或 DStream，代表了一个持续的数据流。DStream可以从输入数据流如源kafka,flume,kinesis来创建，或者从其他DStream中创建。在内部，DStream代理了一个序列的RDD。

本文介绍了如何使用DStream来开发spark streaming程序


#基本概念

## 链接

## 实例化StreamingContext

##Input DStreams and Receivers

DStreams代表了从流式数据源收到的数据流。每个输入的DStream（除了文件流）都关联了一个Receiver对象，但表了数据从源存储到了spark 的内存中。

spark streaming提供了两种类型的流式数据源：

- 基本源：直接通过StreamingContextapi创建，如文件或者socket
- 高级源：像kakfa，flume等，需要引入外部依赖。

要点：

- 当本地运行spark streaming程序时，不要使用local或者local[1]做来master地址。这意味着只有一个线程会用来本地执行任务。
- 在集群中运行时，给应用分配的core一定要多于receiver的。否则，系统可以收到数据，但是无法处理它。

## Using Object Stores as a source of data （没看懂）

### Receiver的可靠性
基于可靠性，有两种类型的数据源。

- 可靠的Receiver：一个可靠的Receiver会发送响应到一个可靠的数据源当它收到数据保存到spark的副本中时。
- 不可靠的Receiver：不会发送响应给数据源。对于不支持响应的数据源或者对于可靠的数据源不希望引入复杂的响应机制，这是有用的。

###Design Patterns for using foreachRDD

要点：

- DStream的输出操作也是延迟执行的，就像RDD的操作也是延迟执行。特别的，在DStream输出操作内部的RDD操作是强制处理收到的数据。因此，一但你的应用没有任何输出操作，或者有输出操作像dstream.foreachRDD() 但是没有RDD操作在里面，那么任何都不会执行。系统会收到数据然后丢弃。
- 默认情况下，输出操作一次只会输出一个。它们按应用中定义的顺序执行。


## DataFrame and SQL Operations

DataFrame and SQL 操作应用到流式数据中。RDD转化为DataFrame。


## 机器学习操作

你可以轻松使用Mlib提供的机器学习算法。首先，机器学习算法可以同时从数据流中学习然后应用到模型中。此外，你也可以使用大量的算法进行离线训练模型，然后应用到在线的数据流中。

### 缓存和持久化

类似RDD，DStream也允许开发者把流式数据持久化到内存。使用persist()方法会自动持久化每个DStream的RDD到内存。如果DStream计算多次的话这是非常有用的。由基于时间窗口的Dstream会自动持久化到内存，不需要开发者调用persist().

对于通过网络接收到的数据流，默认的持久化级别是复制数据到两个结点来实现容错。

注：不像RDD，DStream的默认持久化级别保存序列化的数据到内存。这个后续会讨论。


### 检查点

流式应用会运行24*7因此一定要考虑到与应用无关的错误（如系统报错，JVM出错）因此，spark streaming使用检查点来进行故障恢复。有两种数据用来做检查点：

- 元数据检查点
- 数据检查点：



## 累加器，广播变量和检查点

在spark Streaming中，累加器和广播变量不能从检查点恢复。如果启用检查点的同时，使用了累加器和广播变量，那么你不得不为累加器和广播变量创建延迟加载单例方法，以便于故障恢复时重新实例化。


#容错算法

为了了解spark streaming的算法，我们先了解一下RDD的容错算法。

## 后台

1. RDD是一个不可变，可重新计算的分布式数据集。每个RDD都会记录创建的顺序。
2. 如果一个RDD的分区丢失由于结点故障，那么分区数据可以基于原有的数据集线性计算得出
3. 假设RDD的所有转换都是确定的，那么RDD的最终结果是一致性，即使从spark集群中进行了恢复。

spark基于容错的文件系统操作数据，生成的RDD也是容错的。但是大多数据场景下，spark streaming从网络接收数据。为了达到相同的容错效果，收到的数据会在多个spark执行结点进行复制。这导致在失败后需要恢复两种类型的数据：

1. 数据收到并复制：数据可以恢复因为另外的节点存在副本
2. 数据收到但只是缓存：数据没有副本，只能再次从源进行恢复

此外………………
看晕了，由于RDD进入维护期，暂时不看了







































