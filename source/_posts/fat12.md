---
title: fat12文件系统实现原理
date: 2017-06-13 20:46:25
---

​<iframe src="https://ghbtns.com/github-btn.html?user=imtypist&repo=fat12&type=star&count=true" frameborder="0" scrolling="0" width="90px" height="20px"></iframe><iframe src="https://ghbtns.com/github-btn.html?user=imtypist&repo=fat12&type=fork&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>

### 文件说明

`image/` : fat12格式测试文件

`DiskLib.[h|lib|dll]` : 动态链接库文件

`fat12/` : FAT12动态链接库目录

`fat12_test/` : 测试程序目录

### fat12引导扇区字段说明

<!-- more -->

| 名称             | 偏移(BYTE) | 长度(BYTE) | 内容                         | 软盘参考值                  |
| -------------- | -------- | -------- | -------------------------- | ---------------------- |
| BS_jmpBoot     | 0        | 3        | -                          | jmp LABEL_START && nop |
| BS_OEMName     | 3        | 8        | 厂商名                        | 'Forrest Y'            |
| BPB_BytsPerSec | 11       | 2        | 每扇区字节数                     | 0x200(512字节)           |
| BPB_SecPerClus | 13       | 1        | 每簇扇区数                      | 0x01                   |
| BPB_RsvdSecCnt | 14       | 2        | Boot记录占用多少                 | 0x01                   |
| BPB_NumFATs    | 16       | 1        | 共有多少FAT表                   | 0x02                   |
| BPB_RootEntCnt | 17       | 2        | 根目录文件数最大值                  | 0xE0(224)              |
| BPB_TotSec16   | 19       | 2        | 扇区总数                       | 0xB40(2280)            |
| BPB_Media      | 21       | 1        | 介质描述符                      | 0xF0                   |
| BPB_FATSz16    | 22       | 2        | 每FAT扇区数                    | 0x09                   |
| BPB_SecPerTrk  | 24       | 2        | 每磁道扇区数                     | 0x12                   |
| BPB_NumHeads   | 26       | 2        | 磁头数                        | 0x02                   |
| BPB_HiddSec    | 28       | 4        | 隐藏扇区数                      | 0                      |
| BPB_TotSec32   | 32       | 4        | 如果BPB_TotSec16是0，由这个值记录扇区数 | 0xB40(2280)            |
| BS_DrvNum      | 36       | 1        | 中断13的驱动器号                  | 0                      |
| BS_Reserved1   | 37       | 1        | 未使用                        | 0                      |
| BS_BootSig     | 38       | 1        | 扩展引导标记                     | 0x29                   |
| BS_VolD        | 39       | 4        | 卷序列号                       | 0                      |
| BS_VolLab      | 43       | 11       | 卷标                         | 'OrangeS0.02'          |
| BS_FileSysType | 54       | 8        | 文件系统类型                     | 'FAT12'                |
| 引导代码           | 62       | 448      | 引导代码、数据及其他填充字符等            |                        |
| 结束标志           | 510      | 2        |                            | 0xAA55                 |

### fat1

​	在偏移512字节处开始（即200h），每个fat项占用12bit，fat表开始3个字节不用于用户文件分配，也就是占用了0簇和1簇，所以用户的数据从簇2开始分配。

​	Fat表从头开始按3字节分成一组，一组中第2字节的低半字节作为最高半字节和一组中第1字节组成整数表示一个簇号，第2字节的高半字节作为最低半字节和第3字节组成整数表示一个簇号。

​	我们只需要知道一个文件的首簇号，根据该簇号内容指向的下一个簇号寻找，直到遇到簇号对应内容为0xfff则停止索引，说明已经到了文件末尾。

### 根目录表字段说明

| 名称           | 偏移   | 长度   | 描述                                |
| ------------ | ---- | ---- | --------------------------------- |
| DIR_Name     | 0    | 0xB  | 文件名8个字节，扩展名3个字节，文件名不够8字节用空格0x20填充 |
| DIR_Attr     | 0xB  | 1    | 文件属性，卷标项是0x28，文件项是0x20，目录是0x10    |
| 保留           | 0xC  | 10   |                                   |
| DIR_WrtTime  | 0x16 | 2    | 最后修改时间                            |
| DIR_WrtDate  | 0x18 | 2    | 最后修改日期                            |
| DIR_FstClus  | 0x1A | 2    | 此条目对应开始的簇号                        |
| DIR_FileSize | 0x1C | 4    | 文件大小                              |

### 如何寻找首簇号

​	`根目录起始地址：1(boot区大小)+2(fat数)*9(fat的扇区数)*512(扇区大小)=0x2600h` 

​	从boot记录获取根区记录数，缺省224条。读出所有记录，每条记录32字节，其开始字段为文件名，比较文件名匹配则找到记录，从首簇字段（偏1ah处）获得首簇号。

### 如何定位多级目录

​	e.g. `a:\tem\tem.txt`

​	目录tem作为一个目录项存储在根目录区中。这样可找到tem目录，且**目录项属性应该不是文件**（0x20），而是其它。于是可以继续查找。

​	目录tem本身存储为一个文件，和根目录区一样格式，由32字节的目录项组成。这样tem下的不论是文件还是子目录均由一个目录项登记。而tem目录文件的大小可以随意（不同于根目录），因为文件存储空间可以用fat表的簇链来表示。这样找tem目录项后，从首簇字段获取到tem的目录文件（即类似根目录表），遍历其中的32字节目录项，**查找是否存在某个文件或目录即可**。  如此，即可形成多层次的目录嵌套。

​	先来看看fat12格式文件系统的格式：

| 数据区（长度非固定）  |
| :---------: |
| 根目录区（长度非固定） |
| FAT12（9扇区）  |
| FAT12（9扇区）  |
|  引导扇区（1扇区）  |

​	可以计算得出，根目录区的起始地址为`（1+9+9）*512=0x2600h`，假定根目录区大小为0x1c00（软盘缺省值224*32Bytes），则数据区起始偏移为`0x2600+0x1c00=0x4200`。

​	又例如：Mytxt.txt的簇号为5，减去开始两个号分配给系统对用户区而言，实际是第4个簇（簇号为3）。前面已知用户数据区从0x4200开始，那么，文件实际起始偏移是`0x4200+3*512 = 0x4800`。

### “.”和".."

​	空目录下会有两个目录项，一个是“.”，即2E，一个是“..”，即2E2E，代表了当前目录和上级目录。“.”是当前目录的别名，首簇就应该指向自己，而“..”首簇就改指向上级目录文件的首簇。如果上级目录是根目录，根目录区并没有在用户数据区分配内容。如何填写其簇号呢？其实只要让系统明白这是根目录即可。既然用户簇从2开始（其实这正是从2开始的原因，将特殊簇号留给特殊条目），用0或1作为根目录代表都可以，从实际看，用0代表根目录。

### 总结文件查找算法

1、先查看根目录区，是否有匹配的目录，如果有，通过对应目录项的首簇段获取其目录文件的首簇号。

2、通过fat表获得该目录文件的全部内容，遍历该文件，一次偏移32字节继续查找目录项，匹配查询路径中对应的项。如果查到则类似1，2查找对应的目录文件及目录项，否则说明找不到，结束。

3、如果在倒数第一层目录文件中找到了被查文件的目录项，从中获取首簇号，即可通过fat表访问该文件整个相关簇。

### 删除一个文件

​	目录项第一个字节修改为E5代表删除。

### 回收已用的簇

​	在fat表中，从被删除文件的首簇开始，将每个fat项置为00，实际文件扇区内容不用修改，这应该就是所谓的标记删除了吧，所以我们可以找到e5打头的目录项尝试恢复文件嘿嘿~

### 如何创建一个文件

1、创建文件时，需要首先定位到文件所在的目录文件，然后查找目录项（以32字节为偏移递增），如果第一个字节为0或e5表示可用。

2、根据文件大小计算出需要的簇数目，然后从fat首部开始（第4个字节），查看值为0的项，如果是则可用。找到第一个为0的项，将其索引值（以0开始）写入步骤1找到的目录项的首簇段。接着搜寻为0的项，将其索引值写入前一步找到的项。这样形成一个链，如果是最后一簇，其对应的簇项的值为fff。

3、填写文件目录项的创建时间，属性，大小等字段。

### 调试

用好压2345打开文件系统，就可以直接看到里面的目录、文件，用于直观查看运行效果！但是细节的内容建议使用ultraEdit打开查看文件镜像二/十六进制码，确认格式是否正确。