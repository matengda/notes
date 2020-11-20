# le logical extent

**logical extent    逻辑区域**

逻辑区域是逻辑卷中可用于分配的最小存储单元

逻辑区域的大小取决于逻辑卷所在卷组中的物理区域的大小, 也就是 le = pe




# pe physical extent

**physical extent    物理区域**

物理区域是物理卷中可用于分配的最小存储单元

物理区域的大小是在建立 卷组 时指定的, 一旦确定不能更改

同一卷组所有物理卷的物理区域大小需一致

新的pv加入到vg后, pe的大小自动更改为vg中定义的pe大小





# lv logical volume

**logical volume    逻辑卷**

逻辑卷建立在卷组基础上

逻辑卷是 卷组 中划分的1个逻辑区域

逻辑卷建立后可以动态扩展和缩小空间

卷组中未分配空间可用于建立新的逻辑卷





# vg volume group

**volume group    卷组**

卷组建立在物理卷上, 它是1个或者多个物理卷的组合

卷组将多个物理卷组合在一起, 形成1个可管理的单元

1个卷组中至少要包括1个物理卷

卷组建立后可动态的添加 卷 到卷组中，

1个逻辑卷中可有多个卷组





# pv physical volume

**physical volume    物理卷**

物理卷在逻辑卷管理系统最底层，可以是整个 物理硬盘 或 物理硬盘上的分区

例如: /dev/sdb1





# name

Logical Volume Manager

逻辑卷管理