# Ubuntu 深度环境搭建
## 安装显卡
删除旧驱动

```
apt-get remove nvidia-*
sudo apt-get autoremove   #不执行卸载不干净
```
切换gcc版本
```
gcc -v  #查看当前系统的gcc版本
ls -l /usr/bin/gcc*   #查看系统内安装的gcc
sudo update-alternatives --config gcc #切换gcc版本
```
1. 显卡查看显卡型号

```
lspci | grep -i vga
ubuntu-drivers devices   #推荐安装驱动
sudo ubuntu-drivers autoinstall # 如果同意安装推荐版本，就可以自动安装了
```
NVIDIA-SMI 440.100      Driver Version: 440.100      CUDA Version: 10.2  

2. 查看显卡驱动
去https://www.nvidia.com/Download/index.aspx查看支持 GTX1080 显卡的驱动的最新版本的版本号

3.下载最新或次新驱动 查看次新驱动网址为https://www.nvidia.cn/geforce/drivers/
4.屏蔽开源驱动
```
sudo chmod 666 /etc/modprobe.d/blacklist.conf
sudo vim /etc/modprobe.d/blacklist.conf
#在文件最后添加如下内容
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist rivatv
blacklist nvidiafb
```
5.执行如下命令，更新系统，来禁用nouveau
```
sudo update-initramfs -u 
sudo reboot
```
6.按ctrl+alt+f1进入命令行界面或者命令行输入
```
sudo service lightdm stop
如果提示unit lightdm.service not loaded
则先安装LightDm： sudo apt install lightdm
安装完毕后跳出一个界面，选择lightdm，再sudo service lightdm stop
```
7.卸载掉原有驱动
```
sudo apt-get remove nvidia-*  
```
8.给驱动run文件赋予执行权限
```
sudo chmod  a+x NVIDIA-Linux-x86_64-396.18.run
#安装
sudo ./NVIDIA-Linux-x86_64-396.18.run -no-opengl-files
```
9.安装过程中的选择
安装过程中的选项：（这是copy别人的，自己的没记住，我也是尝试选择了好多遍才安装好）

The distribution-provided pre-install script failed! Are you sure you want to continue? 选择 yes 继续。

Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later?  选择 No 继续。

问题没记住，选项是：install without signing

问题大概是：Nvidia's 32-bit compatibility libraries? 选择 No 继续。

Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up.  选择 Yes  继续

这些选项如果选择错误可能会导致安装失败，没关系，只要前面不出错，多尝试几次就好。
10.挂载Nvidia驱动
```
modprobe nvidia
```
11.按ctrl+alt+f7恢复图像界面或者命令行输入
```
sudo service lightdm start
```
12. 最后测试一下是否安装成功
```
nvidia-smi
nvidia-settings
```
## conda 安装与操作
### 安装
``` shell
bash Anaconda3-2019.07-Linux-x86_64.sh
```
1、 验证是否安装成功

```shell
anaconda
```

2、 如果显示无法找到命令

```shell
vim ~/.bashrc	
```

​	添加

```shell
export PATH=/home/XXX/anaconda3/bin:$PATH
```

 	最后输入如下命令，更新配置文件即可 

```shell
source ~/.bashrc
```

3、 换为清华镜像的命令

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

 4、创建虚拟环境

```shell
conda create -n test_ltis(我的虚拟环境名) python==3.6

常用：
conda activate test_ltis（激活虚拟环境）
conda deactivate your_env_name(虚拟环境名称)
conda remove -n your_env_name(虚拟环境名称) --all， 即可删除。
```

5、安装opencv

```shell
pip install Opencv-python
sudo yum install PyQt4
sudo yum install qt qt-demos qt-designer qt4 qt4-designer

opencv 查看版本
import cv2
cv2.__version__
Out[4]: '3.2.0'
```

请注意换源后，一旦更新库更新库会把所有的库在更新一遍(一般不需要)。 

```shell
更新所有库
conda update –all
更新 conda 自身
conda update conda
```
6、 **Anaconda3常用命令** 

```shell
anaconda用法：
查看已经安装的包：
pip list 或者 conda list

安装和更新：
pip install requests
pip install requests –upgrade
或者
conda install requests
conda update requests
```

7、conda --version  查看版本

8、conda  移动conda安装路径

```shell
mv anaconda3 /newpath/
vi ~/.bashrc                    #修改文件中绝对路径
vi /data1/anaconda3/bin/conda
vim /data1/anaconda3/etc/profile.d/conda.sh   #更改文件中绝对路径
source ~/.bashrc                #更新环境变量
sh /data1/anaconda3/etc/profile.d/conda.sh
```
9、conda常规深度环境安装

```
#创建虚拟环境  虚拟环境名为test_env
conda create -n test_env python==3.6
#激活虚拟环境
conda activate test_env
#安装tensorflow-gpu 1.14.0版本
conda install tensorflow-gpu==1.14.0 #（cuda、cudnn对应版本附带一起安装）
#安装keras
conda install keras==2.2.4
#安装pytorch
conda install pytorch==1.2.0 torchvision==0.4.0 -c pytorch

```

