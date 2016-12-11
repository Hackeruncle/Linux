配置以下5台机器root用户的相互信任关系:
sht-sgmhadoopnn-01
sht-sgmhadoopnn-02
sht-sgmhadoopdn-01
sht-sgmhadoopdn-02
sht-sgmhadoopdn-03

1.删除~/.ssh
[root@sht-sgmhadoopnn-01 ~]# rm -rf ~/.ssh
[root@sht-sgmhadoopnn-02 ~]# rm -rf ~/.ssh
[root@sht-sgmhadoopdn-01 ~]# rm -rf ~/.ssh
[root@sht-sgmhadoopdn-02 ~]# rm -rf ~/.ssh
[root@sht-sgmhadoopdn-03 ~]# rm -rf ~/.ssh

2.生成.ssh目录
[root@sht-sgmhadoopnn-01 ~]# ssh-keygen
[root@sht-sgmhadoopnn-02 ~]# ssh-keygen
[root@sht-sgmhadoopdn-01 ~]# ssh-keygen
[root@sht-sgmhadoopdn-02 ~]# ssh-keygen
[root@sht-sgmhadoopdn-03 ~]# ssh-keygen

3.选取sht-sgmhadoopnn-01,生成authorized_keys文件
[root@sht-sgmhadoopnn-01 ~]# cd .ssh
[root@sht-sgmhadoopnn-01 .ssh]# cat /root/.ssh/id_rsa.pub>> /root/.ssh/authorized_keys 

4.将其他四台的id_rsa.pub文件,ssh到sht-sgmhadoopnn-01的.ssh目录中
[root@sht-sgmhadoopnn-02 ~]# ssh ~/.ssh/id_rsa.pub root@sht-sgmhadoopnn-01:/root/.ssh/pub.1
[root@sht-sgmhadoopdn-01 ~]# ssh ~/.ssh/id_rsa.pub root@sht-sgmhadoopnn-01:/root/.ssh/pub.2
[root@sht-sgmhadoopdn-02 ~]# ssh ~/.ssh/id_rsa.pub root@sht-sgmhadoopnn-01:/root/.ssh/pub.3
[root@sht-sgmhadoopdn-03 ~]# ssh ~/.ssh/id_rsa.pub root@sht-sgmhadoopnn-01:/root/.ssh/pub.4

5.将pub.* 文件内容输出到 authorized_keys
[root@sht-sgmhadoopnn-01 .ssh]# cat pub.* >> /root/.ssh/authorized_keys

6.将sht-sgmhadoopnn-01的authorized_keys scp 给其他四台(第一次传输,需要输入密码)
[root@sht-sgmhadoopnn-01 .ssh]#  scp authorized_keys root@sht-sgmhadoopnn-02:/root/.ssh
[root@sht-sgmhadoopnn-01 .ssh]#  scp authorized_keys root@sht-sgmhadoopdn-01:/root/.ssh
[root@sht-sgmhadoopnn-01 .ssh]#  scp authorized_keys root@sht-sgmhadoopdn-02:/root/.ssh
[root@sht-sgmhadoopnn-01 .ssh]#  scp authorized_keys root@sht-sgmhadoopdn-03:/root/.ssh

7.验证(每台机器上执行下面5条命令,只输入yes,不输入密码,则这5台互相通信了)
ssh root@sht-sgmhadoopnn-01 date
ssh root@sht-sgmhadoopnn-02 date
ssh root@sht-sgmhadoopdn-01 date
ssh root@sht-sgmhadoopdn-02 date
ssh root@sht-sgmhadoopdn-03 date 

     


8.备注:
记录一次帮网友调试ssh信任关系的过程: http://blog.itpub.net/30089851/viewspace-2127102/
