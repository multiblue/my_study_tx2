# Tx2安装tensorflow & pytorch

##### *参考<https://www.ncnynl.com/search/tx2/>，可以找到tensorflow(1.0.1)-python版本，pythrch-python版本以及tensorflow(1.2)-python3.5版本。但里面只提供基本步骤，尚有缺陷。*



#### 1.关于tensorflow>1.2版本安装，详细问题在<https://blog.csdn.net/qq_35239859/article/details/79871152>里有很好的描述。这里只提及解决bazel无法安装5.4等低版本问题，需要将jdk版本降到8u121。去甲骨文官网download这个版本压缩包，解压后，在~/.bashrc里，添加jdk路径。<export JAVA_HOME=/home/nvidia/jdk1.8.0_121||export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin||export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib>。此外，tensorflow有专门在tx2上用的whl文件，去GitHub搜就行了。



#### 2.pythorch 安装最好参考<https://github.com/andrewadare/jetson-tx2-pytorch>



Ps:最好提醒一句，狡兔三窟，多找找方法总比吊死在一种方法上好。另外google这么大，为啥不去逛逛呢！！！emm



