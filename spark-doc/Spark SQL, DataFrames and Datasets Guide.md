# Spark SQL, DataFrames and Datasets Guide

##Overview

spark SQl是用来进行结构化数据处理的模块。不像基本的Spark RDD API，spark sql 接口提供了关于结构化数据以及数据计算的信息。在内部，Spark SQL使用额外的信息来进行额外的优化。有多种方式使用spark sql进行交互，包含sql和Dataset API。当计算一个结果，相同的执行引擎被使用，和你使用哪种语言API来表达这个计算无关。这个统一的方式意味着开发者可以很容易的在不同的API进行切换。

这个页面的所有的样例使用相同的数据在spark发行版中。

##SQL

用户使用Spark Sql来执行SQL查询。 spark sql用来从已安装的Hive中读取数据。关于如何进行配置这个功能，请查看 Hive Tables section。当使用其他编程语言来执行SQL，那么结果会返回一个Dataset/DataFrame. 你可以使用command-line or over JDBC/ODBC.来与SQL接口进行交互。

##Datasets and DataFrames

一个Dataset是一个分布式数据的集合。Dataset是在spark1.6增加的一个新的接口，提供了RDD的优势（强类型，lambda表达式）以及Spark SQL的优化执行引擎。一个Dataset可以从JVM对象进行构造，然后使用函数进行转换 (map, flatMap, filter, etc.)。Dataset API包含在 Scala and Java. Python并不支持Dataset API。但是由于Python动态语言特性，Datset API的许多语言特性已经拥有。

一个DataFrame是一个Dataset组织成为命名的列。它类型于关系型数据库中的表或者R/Python中的data Frame，但是已经大量的进行了优化。DataFrames可以从各类数据源中进行构造，比如结构化的数据文件，hive中的表，外部数据库或者已存在的RDD。DataFrame API在 Scala, Java, Python, and R是可以获得的。Scala and Java语言中，一个DataFrame表示了一个Datset的行。

##Starting Point: SparkSession

在Spark中进入所有功能的入口点是sparkSession类。

sparkSession在Spark2.0中提供了内置的hive特性支持，包含使用HiveSql进行写查询，访问Hive UDF以及从hive表中读取数据。使用这些特性，不需要安装hive;


## Global Temporary View

在SparkSql中的临时视图是session级别的，而且如果session关掉，视图会消失。如果你需要有一个临时视图，在所有的session中共享，而且会一直保持下去直到spark应用关闭掉，你可以创建一个全局的临时视图。全局视图绑定到系统内置的数据库global_temp，我们必须使用名称去指定它。

### Interoperating with RDDs

Spark SQL支持两种不同类型的方法把已存在的RDD转化为Datasets。第一种方法是通过反射来推断一个RDD的schema。反射会产生更多简洁的代码，而且当你已知schema的情况下会运行的更好；

第二种方法是通过编程接口的方式来创建dataset。你可以构造一个schema，然后应用到已存在的Rdd中。这个方法可能会比较繁琐，它允许你构造dataset时可以直到运行时才确认数据列和类型。

###Untyped User-Defined Aggregate Functions  未定义类型的自定义聚集函数


###Type-Safe User-Defined Aggregate Functions 强类型的用户自定义聚集函数

## 数据源

sparkSql支持通过DataFrame接口来操作一系列的数据源。一个DataFrame可以使用关系转换也可以创建临时数据源。登记一个DataFrame作为临时数据源允许你使用Sql查询。


## 手工指定选项

你可以手工指定数据源的额外选项。通过全称可以指定数据源，但是对于内置的数据源，你也可以使用简称（json, parquet, jdbc, orc, libsvm, csv, text）


## 保存方式

- SaveMode.ErrorIfExists (default)	
- SaveMode.Append	
- SaveMode.Overwrite	
- SaveMode.Ignore	

## 保存到持久化表格中

可以使用saveAsTable命令把DataFrame保存到Hive metastore中。注意的是，并不需要部署一套Hive。Spark会创建一个默认的本地hive metastore(使用derby)。SaveAsTable会把DataFrame的内容物理化到hive中，并创建一个指针。只要你保持连接到同一个metastore，spark程序重启后，持久化的表仍然会存在。

对于文件数据源，你可以指定table的文件参数，如df.write.option("path", "/some/path").saveAsTable("t")。当表被删除的时候，指定文件路径不会被删除而且数据还在。如果没有指定表格路径，spark会把数据写入到默认路径。当表被删除的时候，表的路径也会被删除掉。

## 性能优化

对于一些场景，可以通过以下方式提高性能：缓存数据到内存或者开启实验性选项；

### 在内存中缓存数据

spark sql可以缓存数据通过调用spark.catalog.cacheTable("tableName") or dataFrame.cache(). Spark SQL会扫描需要的列，而且自动进行优化压缩，减少内存占用和GC压力。你可以调用spark.catalog.uncacheTable("tableName")来从内存删除表。

### Broadcast Hint for SQL Queries
没看懂


### 分布式SQL引擎

Spark SQL可能通过JDBC /odbc或者命令行接口，来做为一个分布式的查询引擎。终端用户或者应用可以直接运行Sql查询而不需要写任何代码。

## PySpark Usage Guide for Pandas with Apache Arrow

### Apache Arrow in Spark

apache arrow是一个内存列式数据格式，在spark中使用来实现数据从JVM到python线程的快速传输。这对于使用pandas/Numpy的Python用户是非常有用的。它的使用不是自动的，需要一些额外的改动配置或者编码来实现兼容。

### 保证pyArrow安装

### 启用 pandas转换功能























