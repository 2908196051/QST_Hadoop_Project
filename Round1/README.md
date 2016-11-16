# Round 1 介绍
方案
第一步：环境搭建
      1）安装VMware，安装4个linux虚拟机，将主机名分别改为master,slave1,slave2,slave3;
      2）桥接IP容易被抢，采用NAT模式并设置静态IP
      3）下载hadoop2.6.5放到master下并解压;
      4）修改四个节点hosts文件
            192.168.189.132 master
            192.168.189.133 slave1
            192.168.189.134 slave2
            192.168.189.135 slave3
      5) 建立互信关系
            在master ssh-keygen -t rsa生成公钥
            在所有slave节点先ssh-keygen 然后scp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys把公钥复制到slave的./ssh/authorized_keys中
            在master登录每个slave测试成功
      6) sudo apt-get install update
         sudo apt-get install openjdk
         选择安装jdk1.8
         java -version检测安装成功
      7）配置jdk环境变量
