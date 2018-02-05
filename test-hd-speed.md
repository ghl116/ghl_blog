
下面介绍在linux上测试磁盘IO速度的工具： 

1、hdparm

CentOS中，安装的两种方法：

1) yum安装。

# yum install hdparm
2)源码包编译安装

# wget http://ncu.dl.sourceforge.net/project/hdparm/hdparm/hdparm-9.48.tar.gz
# tar zxvf hdparm-9.48.tar.gz
# cd hdparm-9.48
# make && make install 
 

hdparm仅用于Linux系统。现在主要用来测试SSD固态硬盘读取速度。

# hdparm -Tt /dev/sdb

/dev/sdb:
Timing cached reads: 17682 MB in 2.00 seconds = 8855.82 MB/sec
Timing buffered disk reads: 604 MB in 3.01 seconds = 200.98 MB/sec

解释：

2秒钟读取了17682 MB的缓存,读取速度约合8855.82 MB/sec;
在3.01秒中读取了604 MB磁盘数据(物理读),读取速度约合200.98 MB/sec

 

/dev/sdb为SSD固态硬盘。

在我的服务器上，是这样安装的：拆掉光驱位，通过[SATA 22P母 转 SLIMLINE SATA 13P公]转接头连接光驱位的线路，直接连接主板，而不走阵列卡。因为通过阵列卡连接SSD固态硬盘，会影响SSD的性能。

而受限于SATA2接口的读取速度，如果SSD的读取速度在 200MB/sec 以上，则是正常的，说明SSD已经正常工作了。

固态硬盘，在SATA 2.0接口上平均读取速度在225MB/S，平均写入速度在71MB/S。在SATA 3.0接口上，平均读取速度骤然提升至311MB/S。
在随机文件存取测试中，采用SATA 3.0接口的成绩依然要好于采用SATA 2.0接口的成绩。尤其在写入4KB文件方面，SATA 2.0接口平均速度在50MB/S，而采用SATA 3.0后提升至70MB/S。
 

2、dd 

dd 命令并不是一个专业的测试磁盘工具，它没考虑到缓存和物理读的区分，测试的结果仅作参考，不算权威。但是它通用于所有的Linux系统中。

两个特殊设备：（不产生IO，就能单独测试写速度和读速度）
/dev/null 伪设备,回收站.写该文件不会产生IO
/dev/zero 伪设备,会产生空字符流,对它不会产生IO

 

（1）测试磁盘纯写速度

# time dd if=/dev/zero of=/data/test.dbf bs=8k count=300000 oflag=direct
# 加上 oflag=direct，测到的才是真实的磁盘IO速度
解释：从/dev/zero设备中读入数据，写出到/data/test.dbf文件中。bs=8k，每次写的大小，即一个块的大小。count=300000，一共写30000块。

（2）测试磁盘纯读速度

# time dd if=/data/test.dbf of=/dev/null bs=8k count=300000
解释：从/data/test.dbf文件中读入数据，写出到/dev/null 设备中。bs=8k，每次读的大小，即一个块的大小。count=300000，一共读30000块。

（3）测试磁盘读写速度

# time dd if=/data/test_r.dbf of=/data/test_w.dbf bs=8k count=300000
 

备注:要想测试准确，测试的数据量最好大于系统内存（避免内存干扰），最好测试5次以上取平均值。

3、sysbench

 https://www.ustack.com/blog/how-benchmark-ebs/#Linux

4、fio

 http://blog.chinaunix.net/uid-8116903-id-3914246.html

参考：http://www.cnblogs.com/hjqjk/p/5773099.html
