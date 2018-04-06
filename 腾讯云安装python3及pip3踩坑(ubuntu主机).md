# 腾讯云安装python3及pip3踩坑(ubuntu主机)
## ssl依赖库部分
使用pip时会提示ssl不可用之类的错误，这是因为系统缺少相关的openssl相关的依赖库，应该在终端使用如下两行命令：
```
sudo apt-get install openssl
sudo apt-get install libssl-dev
```
## configure部分
而且安装python3时没有加上--with-openssl参数，因此应该在.configure时补上这个参数：
```
./configre --with-openssl
```
**完整的命令如下**：
```
./configure --with-openssl
make
sudo make install
```
## tcl/tk依赖
首先，终端要输入：
```
sudo apt-get install python-tk 
```
其次，到[http://www.tcl.tk/software/tcltk/download.html](http://www.tcl.tk/software/tcltk/download.html)下载tar.gz格式的tcl/tk安装包。
命令如下：
```
wget https://prdownloads.sourceforge.net/tcl/tcl8.6.8-src.tar.gz
wget https://prdownloads.sourceforge.net/tcl/tk8.6.8-src.tar.gz
```
https下载有时可能会出现问题，可以在后面补上选项`--no-check-certificate`
然后，解压缩，安装tcl，tk，以tcl为例子：
```
tar -zxvf tcl8.6.8-src.tar.gz
cd ./tcl8.6.8/unix/
./configure
make
sudo make install
```
** 注意一个小地方，安装tk过程可能会报类似如下的错误：
```
~/tk8.6.8/unix/../generic/tk.h:96:25: 致命错误： X11/Xlib.h：没有那个文件或目录  
编译中断。  
make: *** [tk3d.o] 错误 1 
```
补上如下的依赖包：
```
sudo apt-get install libx11-dev  
```
使用到pip时要先切换到管理员身份（或者一般用户要加上sudo）
```
 pip3.6 install --upgrade pip
 pip3.6 install numpy
```
然后继续以相同的方式安装tk(参考上面的tcl)



