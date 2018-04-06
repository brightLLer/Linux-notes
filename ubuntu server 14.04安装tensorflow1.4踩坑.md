# ubuntu server 14.04安装tensorflow1.4踩坑

+ tf1.4要求cuda8.0+cudnn6，别人安装过的cuda8可以拷贝一份，再装入cudnn6，比如复制到/usr/local/cuda-8.0-ling
+ 导入环境变量，使用自己（Home下的用户）的环境变量，`vim ~/.bashrc`，然后`source ~/.bashrc`生效，不要随便重启。

```
export PATH=/usr/local/cuda-8.0-ling/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/cuda-8.0-ling/lib64:/usr/local/cuda-8.0-ling/extras/CUPTI/lib64:LD_LIBRARY_PATH
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-8.0-ling/lib64
export CUDA_HOME=/usr/local/cuda-8.0-ling:$CUDA_HOME
export CUPIT_LIB_PATH=/usr/local/cuda-8.0-ling/extras/CUPTI/lib64
```
+ 安装anaconda（Linux）
```
wget https://repo.continuum.io/archive/Anaconda3-5.1.0-Linux-x86_64.sh
bash ~/Downloads/Anaconda3-5.1.0-Linux-x86_64.sh
# 之后按要求yes/no一步一步回车就行
# 安装完成后保存到了~/anaconda3
# 用~/.bashrc再配一下conda的环境变量
export PATH=~/anaconda3/bin:$PATH
# 之后查看conda的版本
conda -V
```
如果是安装windows版本的anaconda
```
# path环境变量一般都是指定到可执行文件exe文件的的上级目录为止，例如bins
# 指定python.exe的安装目录如下
path = D:/anaconda3/
# 指定conda,pip,jupyter等的环境变量
path = D:/anaconda3/Scripts/
# 类似的还有Java，建立系统变量JAVA_HOME作为前缀，指定Java的环境变量（在path里）
JAVA_HOME = XXXXX
path = %JAVA_HOME%/YYYY
where java
```
+ 由于python3.6不支持tf1.4，所以用conda开一个python3.5的环境
```
conda create --name speech python=3.5
conda info --envs # 查看环境信息
source activate speech
```
+ 然后安装tensorflow1.4
```
(sudo -H) pip install tensorflow-gpu==1.4
# 如果是更新
pip install --upgrade tensorflow
# 卸载
pip uninstall tensorflow
```
+ 配置远程的jupyter server
```
# activate python3.5环境(speech)之后
conda/pip install jupyter
jupyter notebook --generate-config
# 写入配置文件到/home/brightLLer/.jupyter/jupyter_notebook_config.py
```
```
In [1]: from IPython.lib import passwd

In [2]: passwd()
Enter password: 
Verify password: 
Out[2]: 'sha1:0e422dfccef2:84cfbcbb3ef95872fb8e23be3999c123f862d856' 
# 其实就是获取到登录jupyter的密码，经过了hash
```
```
# 修改一下配置文件
$vim ~/.jupyter/jupyter_notebook_config.py 
```
```
# 注意去掉前面的#号
c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8866 #随便指定一个端口
```
```
# 启动jupyter
jupyter notebook --no-browser --port 8866 -ip=0.0.0.0
# 本地浏览器输入
http://address:88666/
```
```
# 这里可能出现一个问题，就是jupyter会启动默认的python3.6，而不是speech环境里的python3.5,因此可以使用如下命令添加speech环境里的python版本，如果都是python3会覆盖掉原来的
 conda install -n speech ipykernel
 python -m ipykernel install --user
 # 提示：Installed kernelspec python3 in /home/brightLLer/.local/share/jupyter/kernels/python3
```
+ 启动远程tensorboard
```
# 使用putty远程连接之后，在speech(py3.5)环境下开启tensorboard
# 然后在本地浏览器直接输入http://address:yyyy就行
tensorboard --logdir xxxx --port=yyyy
```
+ 将训练数据上传到ubuntu server
```
# 首先先在本地(windows)下载和安装xftp
# 之后ubuntu上要先安装好ftp服务
sudo apt-get install vsftpd
# 然后修改下配置文件
sudo vi /etc/vsftpd.conf
# 在normal模式下输入:set nu显示行号
# 26 local_enable=YES
# 29 write_enable=YES
# 启动ftp服务
sudo service vsftpd start/restart # (修改过配置使用restart)
# 关闭ftp服务
sudo service vsftpd stop
# 使用xftp创建于服务器的连接，端口默认21，用户名和密码就是自己登录服务器的账号和密码
```

