# Round 1 介绍
方案
第一步：环境搭建
      1) 安装VMware，安装4个linux虚拟机，将主机名分别改为master,slave1,slave2,slave3;
      2) 桥接IP容易被抢，采用NAT模式并设置静态IP
      3) 下载hadoop2.6.5放到master下并解压;
      4) 修改四个节点hosts文件
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
      7) 配置jdk环境变量
      8) 配置 hadoop-env.sh等。涉及的配置文件如下： 
         hadoop-2.6.0/etc/hadoop/hadoop-env.sh (JAVA_HOME)
         hadoop-2.6.0/etc/hadoop/yarn-env.sh   (JAVA_HOME)
         hadoop-2.6.0/etc/hadoop/core-site.xml 
         hadoop-2.6.0/etc/hadoop/hdfs-site.xml 
         hadoop-2.6.0/etc/hadoop/mapred-site.xml 
         hadoop-2.6.0/etc/hadoop/yarn-site.xml
      9) hadoop启动测试
            bin目录下 namenode -format
            进入sbin目录 start-all.sh
            输入jps查看进程；hadoop启动成功
第二步：
      1) 读入数据，用split以空格切分，切分后 提取出IP和URL 以每天为rowkey CF:IP   CF:URL两列  存入hbase表
      2) UV统计：从表中读出某一天的数据 对 CF:IP的内容进行count，统计去重后的IP数量
      3) PV统计：从表中读出某天数据，正则匹配出有show的数据，对这些数据处理，以show的URL为key，对访问过它的IP进行去重count，取出结果最大的10个
      4) 次日留存统计：从hbase开始循环读入每天数据，以IP为key，value=1；进行count，如果第二天有同一个IP访问 value+1。记录下IP
