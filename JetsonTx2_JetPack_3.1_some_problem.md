# Jetson Tx2(JetPack 3.1)刷机遇到的问题

___

### 安装过程

##### 大致参考  <https://www.jianshu.com/p/bb4587014349>

___

## 遇到的问题如下

___

#### 1. loading update lock

###### 我安装JetPack 3.1出现这个问题的原因是外网被墙。

###### __解决方法__：首先要有vpn。我自己买了vpn，所以没有vpn的小伙伴可以自己找找免费的vpn。其次要安装proxychains，可以参考<https://juejin.im/post/5ad5a17bf265da237314f21a>，也可以自己在github上搜proxychains。（PS:安装proxychains后，用`proxychains ./JetPack-L4T-3.1-linux-x64.run`命令实现翻墙）

___

##### 2. JetPack is unable to determine the IP address of the Jetson Developer Kit 

###### 我安装JetPack 3.1出现这个问题的原因是校园网需要认证登陆而且需要重启tx2后才能联网。

##### 注意事项：

###### 1）出现这个问题后，系统提示有两种选择。 `1.Retry`,`2.Manually enter IP address`选择1会重新尝试连接TX2，选择2会直接退出。

###### 2）在选择1之前，务必确认TX2和主机能够ssh访问。

######   TX2能联网。（这里以山大校园网为例讲联网的过程和问题）

###### 首先，进入Edit Connections在802.1安全连接处，设置账户和密码。如果还存在问题，还需要配置IPv4加入附加DNS。之后，`sudo gedit /etc/NetworkManager/system-connections/XX`(最后的XX代表自己编辑网络连接的名称)。再执行`sudo reboot`。

###### 其次，确定主机能够ssh访问TX2。主机需要安装ssh-server，由于我们使用proxychains命令运行.run文件，所以需要`proxychains ssh nvidia@10.20.4.22`能访问才行（后面的IP根据实际TX2的IP改变）。（PS:创新大厦实验室网不行，导致ssh 无法访问TX2，回寝室之后可以访问。）

###### 再则，确定TX2能够联网，能够更新。第一步做完重启后，ifconfig会显示TX2的IP等详细信息。但运行`sudo apt-get update`依然无法更新。这时需要做两步：第一步，`sudo gedit /etc/resolv.conf`，打开文件后，添加`nameserver 8.8.8.8`（谷歌DNS服务器）；第二步，更改源，TX2改源无法通过图形界面。所以需要参考<https://blog.csdn.net/qlulibin/article/details/80271096>，通过命令行修改源。最后执行更新。

___

#### 3. Error:CUDA cannot be installed on device. This may be caused by other apt-get command running on device when installing CUDA. Please use apt-get command  in a terminal to make sure to following packages are installed correctly on device before conuing: cuda-toolkit-8-0     After these packages are installed on device,press Enter key to continue

###### 这个问题大可不必在意，这些提示要安装的库都已经在TX2上安装好了，直接按Enter键就行。(为了不出差错，你可以尝试都在TX2上安装一遍。)

___

#### 4.验证TX2安装成功与否的命令行

###### 错误的命令行：`./backend 1 ../../data/Video/sample_outdoor_car_1080p_10fps.h264 H264 --trt-deployfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.prototxt --trt-modelfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.caffemodel --trt-forcefp32 0 --trt-proc-interval 1 -fps 10`

###### 正确的命令行：`./backend 1 ../../data/Video/sample_outdoor_car_1080p_10fps.h264 H264 \``--trt-deployfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.prototxt \``--trt-modelfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.caffemodel \``--trt-forcefp32 0 --trt-proc-interval 1 -fps 10`