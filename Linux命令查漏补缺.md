# Linux命令查漏补缺
> 忘了又学，学了又忘，忘了再学......
## 1、过滤和管道
```
# 过滤出hello.py中包含def的文本行，若加上-v，则与上述操作相反
grep [-v] 'def' hello.py

# 管道：上一个命令的输出作为下一个命令的输入
# 文件列表中只过滤出有.py的行
ls -al | grep '.py'
# 过滤出hello.py中包含def的文本行
cat hello.py | grep 'def'
```
## 2、重定向
```
# >和>>的区别在于一种是清空后重写，一种是追加一行
# 箭头右边都是文件而不是标准输出，建议用>>
# 第一组输出:Python
echo Java > hello.txt
echo Python > hello.txt
# 第二组输出:Java
#            Python
echo Java > hello.txt
echo Python >> hello.txt
```
## 3、删除rm
```
rm -r ./xxx/yyy # -r递归删除某目录，当然用于文件也没问题
rm -rf ./xxx/yyy # -f强制删除，请慎用
```
## 4、touch新建文件
```
touch filename # 创建一个新文件，如文件存在则只更新修改时间
```
## 5、mv移动与重命名
```
mv ~/test1/hello.py ~/test2/ # 移动
mv ~/test1/hello.py ~/test1/HelloNew.py # 重命名(一般在同一目录下)
mv ~/test1/hello.py ~/test2/HelloNew.py # 移动并重命名
```
## 6、复制
```
cp ~/test1/hello.py ~/test2/ # 仅复制
cp ~/test1/hello.py ~/test2/hellocp.py # 复制并重命名
cp -r /test /test1 # 复制目录
```
## 7、解压缩
```
tar -zxvf xxx.tar.gz # 解压到当前目录
```
## 8、创建文件夹
```
mkdir test1 test2 # 创建test1和test2两个目录
```
## 9、ping命令
```
ping -c 4 www.baidu.com # 限制发送4个ping包，否则会无限发送
```
## 10、ps命令
```
ps -ef # 显示所有进程信息
ps -ef | grep ssh # 过滤出仅含有ssh的进程信息
```
## 11、lsblk命令
```
lsblk -a # 查看块设备的信息，常用于检测usb设备连接成功与否
```
## 12、下载，安装，卸载，更新，搜索软件包
```
wget https://www.xxx.com/yyy.tar.gz
sudo apt-get install xxx
sudo apt-get remove xxx
sudo apt-get update # 更新软件源列表
sudo apt-get upgrade # 真正更新软件（升级）
sudo apt-cache search xxx
```
## 13、安装下载的软件包的步骤
```
tar -zxvf xxx.tar.gz
cd xxx
./configure
make
sudo make install
# 用户安装的软件包一般在/usr/local/
```
## 14、连接文本文件的内容
```
cat aaa.txt # 显示aaa.txt的内容
cat aaa.txt bbb.txt # 连接两个文件的内容并显示
```
## 15、vim编辑文本
```
vim hello.txt
# 按住i进入insert(编辑)模式
# 按住esc进入normal(模式)：1、dd删除当前行；2、x删除当前光标定位的字符；3、wq强制写入磁盘并退出
```
## 16、ifconfig显示网络接口
```
ifconfig [-a]
```
## 17、查看tcp连接
```
netstat -n
netstat -n | grep 80 # 过滤出80端口的tcp连接
```
## 18、chmod修改文件权限
```
# rwx:421 ugo/a:user,group,others
chmod 776 hello.py
chmod ug=rwx,o=rw hello.py
chmod ug+x hello.py
chmod o-x hello.py
chmod a+x hello.py
```
## 19、日历，日期，帮助手册，历史命令
```
cal # 查看日历
date # 查看日期
man ls # 查看关于ls的帮助说明
history # 查看最近使用过的命令
```




