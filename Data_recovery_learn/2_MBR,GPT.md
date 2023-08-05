# MBR硬盘

[应知必懂的两种磁盘分区类型：MBR 和 GPT](https://zhuanlan.zhihu.com/p/541733200)

MBR 是 Master Boot Record（主分区引导记录），这种规范将硬盘引导启动程序和磁盘分区信息保存在大小512字节的0磁道主引导扇区中，所以如果0号扇区损坏，就会造成 MBR 信息丢失，从而硬盘所有分区无法正常识别，对硬盘的损坏是非常严重的。

## MBR主引导扇区的结构

![](resources/2023-08-05-15-21-54.png)

MBR由三部分构成：
- 主引导程序代码，占446字节
- 硬盘分区表DPT，占64字节
- 主引导扇区结束标志"AA 55"

### 硬盘分区表DPT的结构

在DPT共64个字节中，以16个字节为分区表项单位描述一个分区的属性

![](resources/2023-08-05-16-46-19.png)

![](resources/2023-08-05-16-54-20.png)

#### 硬盘分区数量的限制

MBR的分区表只能保存4个分区信息，因此MBR的硬盘只能划分为4个分区，有人可能会提出疑问：我的电脑是MBR硬盘，为什么分区的数量超过四个呢？

这是因为MBR分区表中虽然只能支持4个主分区，但是可以将其中应该为主分区的位置设置为扩展分区，每个扩展分区可以划分为多个逻辑分区，这些逻辑分区的信息保存在MBR以外的扇区中，而Windows的文件管理中是不区分主分区还是逻辑分区的，因此看起来所有分区数量多于4个，实际上主分区和扩展分区的总数仍然不超过4个。

## MBR硬盘的结构

![](resources/2023-08-05-16-50-59.png)



# GPT硬盘

为了破除只有512个字节的 MBR 分区表的许多限制，技术人员对硬盘分区表的标准进行了升级，提出了新的 GUID Partition Table（全局唯一标识磁盘分区表），简称 GPT 分区表，其中 GUID 是 Globally Unique Identifier（全局唯一标识符）的缩写，这是一种由算法生成的长度为128位的数字标识符。

但是由于 GPT 改变了硬盘的分区表格式，影响了电脑的初始启动过程，传统BIOS启动过程无法识别这种GPT硬盘的分区因此无法启动，采用最早由 Intel 提出的新的UEFI启动方式才能从 GPT 启动。

![](resources/2023-08-05-15-28-54.png)


# 扇区和簇（块）的区别

扇区(sector)，块(block)，簇(cluster)

[操作系统 | “扇区”、“簇”、“块”、“页”等概念](https://blog.csdn.net/weixin_47187147/article/details/126908793)

![](resources/2023-08-05-18-10-45.png)

- 扇区是对硬盘而言，是物理层的
- 簇（块）是对文件系统而言，是逻辑层的
- 磁盘控制器是用来映射两层的
- 一个块大小 = 一个扇区大小*2^n

扇区是磁盘最小的物理存储单元，是磁头从磁盘中读取数据的最小单位（一般512B），即磁头每次从磁盘中读取数据，都是一个扇区一个扇区读的。但由于操作系统无法对数目众多的扇区进行寻址，所以操作系统就将相邻的扇区组合在一起形成一个簇，然后再对簇进行管理（每个簇可以包括2、4、8、16、 32或64个扇区）

 簇（块）是操作系统与磁盘（硬盘）交互的最小数据单元（在linux系统中称为块，在windows系统中称为簇）。操作系统从硬盘中拿一块数据，即完成一次磁盘IO

 簇（块）的大小在硬盘格式化时被指定，一般有1K，2K，4K（最常用）。如果块的大小设置为4K，那么磁盘要读取8个扇区之后，才将数据块传给操作系统。另外， 簇（块）也是DOS下数据存储的最小单元。例如，如果一个文件的大小为1K，而块的大小为4K，那么该文件还是会占用一个块，块中剩下的3K被空闲出来，不能用于存储其他数据。因此，设置块的大小时，需要考虑要存储文件的大小
