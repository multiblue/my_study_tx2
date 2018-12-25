# Jetson Tx2刷机的三种方法介绍

***

###  *参考来源*

##### <https://www.ncnynl.com/search/tx2/>

##### <http://www.realtimes.cn/jishuzhichi/jishuwendang/2018/0305/873.html>

***

### 方法一（JETPACK自动刷机）

##### 适用情况

###### \* 外网未被墙

##### 1. 在Ubuntu主机上，下载好JetPack*.run文件，更改执行权限：

###### `chmod +x JetPack-L4T-3.0-linux-x64.run`

##### 执行安装

###### `./JetPack-L4T-3.0-linux-x64.run`

#####  2.连上Tx2，进入recovery模式(通电，按住recovery键3秒，再按一下reset键 )

#####  检查是否出现xxxx:xxxx Nvidia Corp (检测命令`lsusb`)

##### 3.其余步骤详细，可以百度“Jetson TX2刷机”，你可以找到非常详细的安装步骤作参考。

***

### 方法二（Proxychains + JetPack刷机包）

##### 适用情况

###### \* 外网被墙

##### 详细步骤参考本人另一篇笔记*Jetson Tx2(JetPack 3.1)刷机遇到的问题*

##### ps:方法二存在只能刷ubuntu系统，无法安装cuda、cudnn等安装包的问题。

***

### 方法三（TX2备份和恢复）

##### 适用情况

###### \* 外网被墙，以及方法二不再适用。

##### 1. 准备好备份环境，之前刷机留下的安装环境（TX2所在目录）

##### ps:刷机环境仅仅是刷完系统即可，无需完整安装所有可选安装包。

##### 2. *备份* ubuntu 主机连上TX2，进入recovery模式，检测TX2是否连上主机。

##### \+ 从安装完整可选安装包的TX2下载镜像:

`sudo ./flashNew.sh -r -k APP -G my_backup.img jetson-tx2 mmcblk0p1`

##### ps:原来的flash.sh缺少 -G 参数支持，改为新增加脚本flashNew.sh 。flashNew.sh脚本文件，参考<https://www.ncnynl.com/archives/201706/1740.html>

##### \+ 分配权限，压缩保存（可选）

##### `cd ~/TX2/64_TX2/Linux_for_Tegra_64_tx2/ `

#####  `sudo chmod 744 my_backup.img`

##### `tar -zcvf my_backup.img.zip my_backup.img`

##### \+ 备份后挂载，可以查看是否备份成功。

##### `mkdir testimg`

##### `sudo mount -o loop my_backup.img  testimg`

##### ps:挂载很有可能不成功，参考<http://www.realtimes.cn/jishuzhichi/jishuwendang/2018/0305/873.html>提供的方法即可。

#####  3. *恢复* 复制`~/TX2/64_TX2/Linux_for_Tegra_64_tx2`目录下的my_backup.img到`~/TX2/64_TX2/Linux_for_Tegra_64_tx2/bootloader`目录下的system.img，等同于my_backup.img替换了system.img。

##### \+ 主机连接上待安装的TX2上，进入recovery模式，检测Tx2与主机的连接情况。

##### \+ 进入`~/TX2/64_TX2/Linux_for_Tegra_64_tx2/`目录，使用flashNew.sh烧录。

##### `sudo ./flashNew.sh -r  jetson-tx2 mmcblk0p1`

***

### 方法三（plus）

##### 适用情况

###### \* 方法三需要已经刷好的TX2来进行镜像拷贝恢复到另一个待刷的TX2。本方法提供一种简单版的TX2镜像拷贝。

##### 1. 下载刷机包

##### `mkdir -p ~/tools/jetpack/tx2 `

##### `cd ~/tools/jetpack/tx2 `

##### `wget -O tx2.tbz2  https://developer.nvidia.com/embedded/dlc/l4t-jetson-tx2-driver-package-28-1 `

##### 2.解压包

##### `bzip2 -d l4t-tx2-28-1.tbz2`

##### `tar xf l4t-tx2-28-1.tar`

##### 3.备份和恢复，如方法三步骤。

##### ps:参考<https://www.ncnynl.com/archives/201706/1742.html>

### 方法补充

##### ps:方法三备份安装好cuda等软件包的TX2时，特别消耗磁盘空间。为此，要么你的ubuntu系统存储空间够大（本人备份时，需要至少80G的剩余空间），要么想办法压缩镜像文件。以下是压缩镜像方法

##### 1.把 备份回来的my_backup.img 用下面命令sparse一下，这样可以大大减少image大小，加快后续的烧写速度

##### `./mksparse -v --fillpattern=0 `

##### `my_backup_image_APP.img`

##### `my_backup_image_APP.img.sparse`

##### 2. 然后用`my_backup_image_APP.img.sparse`替换 `bootloader/system.img`

##### 3. 然后用flash.sh烧写时，切记加  "-r" 选项 (reuse system.img)

### 方法后续

##### 方法补充的方法太消耗时间，而且对主机存储空间也有要求。所以在这里附上一种方法。在刷机成功（只安装了ubuntu系统，没有安装cuda、cudnn等相关工具）后，在tx2的/home/nvidia目录下有一个report_ip_to_host.sh文件，运行 sudo  ./report_ip_to_host.sh 。然后再主机那边选择1.retry,主机会通过局域网传输相应的.deb文件并执行安装。
