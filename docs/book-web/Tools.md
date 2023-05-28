```java
//.deb结尾的文件直接 sudo dpkg -i Xxxxxamd64.deb就可以用
//我这个惠普只能用For X64的 下WPS的时候
//.tar.gz 的得解压然后放到/usr/local里面，然后设置快捷方式
```

### 阿里云盘操作

```shell
工具地址：https://github.com/tickstep/aliyunpan/releases 
启动：  ./aliyunpan    输入网页版的refresh_token值
登录：login
config set -savedir /download       注意修改文件权限才能下载
下载得切换到那个目录下去下载那个目录下的文件，用下面的命令
下载文件：download <网盘文件或目录的路径1><文件或目录2><文件或目录2>...                
                     d   <网盘文件或目录的路径1><文件或目录2><文件或目录2>...
上传文件：upload  <本机文件目录/文件名./>
查看分享的文件：share l
保存分享链接文件到网盘：rapidupload 
```

### 搜狗安装好不能使用拼音

```shell
sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2

sudo apt install libgsettings-qt1
```

### 更新下载卸载软件包

```shell
sudo aapt-get update
sudo apt-get upgrade
sudo apt-get install default-jre
sudo apt-get install default-jdk   

# 找到软件包名
sudo apt list --installed
# 如果软件没展示出来，它可能被安装成一个snap包
snap list
sudo snap remove package_name
# 移除软件包
sudo apt remove package_name
# 卸载软件包文件
sudo apt purge package_name
```

### 安装7z解压缩

```shell
# 安装方法：
sudo apt-get install p7zip
# 解压文件：
7z x manager.7z -r -o /home/xx
# 解释如下：
x 代表解压缩文件，并且是按原始目录解压（还有个参数 e 也是解压缩文件，但其会将所有文件都解压到根下，而不是自己原有的文件夹下）manager.7z 是压缩文件，这里大家要换成自己的。如果不在当前目录下要带上完整的目录
-r 表示递归所有的子文件夹
-o 是指定解压到的目录，这里大家要注意-o后是没有空格的直接接目录

# 压缩文件：
    7z a -t7z -r manager.7z /home/manager/*
# 解释如下：
a 代表添加文件／文件夹到压缩包
-t 是指定压缩类型 一般我们定为7z
-r 表示递归所有的子文件夹，manager.7z 是压缩好后的压缩包名，/home/manager/* 是要压缩的目录，＊是表示该目录下所有的文件。
```

### 安装搜狗输入法

```
安装搜狗输入法：https://shurufa.sogou.com/linux/guide 
我的笔记本只能下载 sogoupinyin_4.0.1.2800_x86_64.deb来用

外接显示器：直接在软件与更新里面应用英伟达的专用驱动  重启
```

### 安装迅雷

```java
### 配置源
```java
```

### 安装typora

```python
#安装typora

# or run:
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

wget -qO - https://typoraio.cn/linux/public-key.asc | sudo tee /etc/apt/trusted.gpg.d/typora.asc

# add Typora's repository

sudo add-apt-repository 'deb https://typoraio.cn/linux ./'

sudo apt-get update

# install typora

sudo apt-get install typora
```

### ubuntu系统问题

```python
# 查看版本命令
lihang@keary-by-hp:~$ uname -a
Linux keary-by-hp 5.19.0-38-generic #39~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Mar 
17 21:16:15 UTC 2 x86_64 x86_64 x86_64 GNU/Linux


# su认证失败
linux su认证失败的解决方法：
首先输入“sudo passwd root”；
然后输入3次密码，第一次是当前用户密码，
第二次、第三次是root用户密码；
最后使用su切换到root用户即可
    
# ubuntu系统字体错误
先在的错字：复包。。。  系统优先显示日文汉字
su
进入：cd /etc/fonts/conf.avail/
vim 64-language-selector-prefer.conf

JP －日文
KR － 韩文
SC － 简体中文
TC － 繁体中文
# 修改里面的文件，将顺序改成SC TC JP KR（HK不管）
# 重启就行

# 配置源
NO_PUBKEY BA300B7755AFCFAE
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
W: https://typoraio.cn/linux/./InRelease: 密钥存储在过时的 trusted.gpg 密钥环中（/etc/apt/trusted.gpg），请参见 apt-key(8) 的 DEPRECATION 一节以了解详情。

# ubuntu卸载软件两种方式----卸载apt-get install 的软件
# 取系统上所有已经安装的软件包列表
# 可以使用 grep 来过滤结果
sudo apt list --installed

sudo apt remove package_name
#
sudo apt-get remove package_name
# 卸载多个软件包。软件包名之前应该被空格进行分隔：
sudo apt remove package1 package2
# remove命令卸载指定软件包，但是留下一些包文件。如果你想卸载软件包，包括它的文件，使用purge替换remove
sudo apt purge package_name

# 运行sudo apt list --installed时，如果你想要卸载的应用没有被列出来，那么它可能被安装成一个 snap 包
# 列出所有的 snap 软件包
snap list
sudo snap remove package_name  #知道准确的软件包名

# ubuntu卸载软件两种方式----卸载sudo dpkg -i Xxxxxx 的软件
dpkg --list
sudo apt-get --purge remove <programname>   #移除软件相关文件
sudo apt-get remove <programname>   #移除程序但保留配置文件

virtualbox-dkms/jammy-updates,now 6.1.38-dfsg-3~ubuntu1.22.04.1 amd64 [installed,automatic]
virtualbox-qt/jammy-updates,now 6.1.38-dfsg-3~ubuntu1.22.04.1 amd64 [installed,automatic]
virtualbox/jammy-updates,now 6.1.38-dfsg-3~ubuntu1.22.04.1 amd64 [installed]

sudo apt remove virtualbox-dkms virtualbox-qt virtualbox

```

### 温度控制

在Ubuntu系统中，可以使用lm-sensors和fancontrol工具来控制笔记本风扇的转速。这两个工具需要安装并配置才能使用。以下是具体步骤：

1. 安装lm-sensors和fancontrol。在终端中执行以下命令：

   ```
   sudo apt-get update
   sudo apt-get install lm-sensors fancontrol
   ```

2. 配置lm-sensors。在终端中执行以下命令：

   ```
   sudo sensors-detect
   ```

   按照提示一步一步操作，探测到传感器后，将会要求你是否将探测到的传感器信息添加到/etc/modules文件中，输入“yes”后再按回车即可。

3. 测试lm-sensors。在终端中执行以下命令：

   ```
   sensors
   ```

   将会显示您笔记本电脑的温度传感器信息。

4. 配置fancontrol。在终端中执行以下命令：

   ```
   sudo pwmconfig
   ```

   按照提示一步一步操作，根据探测到的传感器信息配置fancontrol，设置适当的风扇转速。

5. 测试fancontrol。在终端中执行以下命令：

   ```
   sudo fancontrol
   ```

   将会显示您笔记本电脑的风扇转速信息。

在安装了lm-sensors和fancontrol之后，您可以使用fancontrol工具来控制风扇的转速。fancontrol工具根据传感器数据来自动调整风扇转速，以保持电脑的温度在安全范围内。您也可以手动配置fancontrol，以便在需要时手动调整风扇转速。

以下是手动配置fancontrol的步骤：

1. 打开终端，并以root用户身份运行fancontrol：

   ```
   sudo fancontrol
   ```

2. fancontrol将显示一些有关探测到的传感器和风扇的信息。按照提示一步一步操作，设置风扇的最小转速、最大转速、变化速率等参数。

3. 当您完成配置后，按Ctrl+C退出fancontrol。

现在，fancontrol将自动根据传感器数据调整风扇的转速。如果需要手动调整风扇转速，您可以再次运行fancontrol，然后使用键盘上的上下箭头键来调整风扇转速。调整完毕后，按Ctrl+C退出fancontrol。

请注意，在手动调整风扇转速时，需要谨慎操作，以免损坏电脑硬件或导致系统稳定性问题。建议在了解相关知识和操作方法后再进行调整

### 解压与快捷图标

```shell
# 解压 .tar.gz压缩包
tar -zxvf 压缩文件名.tar.gz
# 移动文件命令
sudo mv android-studio /usr/local

sudo gedit /usr/local/share/applications/IDEA.desktop

[Desktop Entry]
Name=Android Studio
Comment=android studio
Exec=/usr/local/android-studio/bin/studio.sh
Icon=/usr/local/android-studio/bin/studio.png
Terminal=false
Type=Application
Categories=Developer

[Desktop Entry]
Name=Clash Fow Windows
Exec=/home/user/local/bin/cfw
Icon=/home/user/local/bin/cfw
Type=Application
StartupNotify=true
    
#去usr/share/applications中 touch Xxx.desktop  然后添加下面 注意修改图标路径
[Desktop Entry]
Name=IntelliJ IDEA
Comment=IntelliJ IDEA
Exec=/usr/local/idea-IU-223.8617.56/bin/idea.sh
Icon=/usr/local/idea-IU-223.8617.56/bin/idea.png
Terminal=false
Type=Application
Categories=Developer
```

### jdk

```shell
sudo mkdir /usr/lib/jvm

//解压缩jdk包到/usr/lib/jvm
sudo tar -zxvf jdk-18tar.gz -C /usr/lib/jvm
//打开环境文件
gedit ~/.bashrc

//添加下面
#set oracle jdk environment
export JAVA_HOME=/usr/lib/jvm/jdk-18.0.1.1
  ## 这里要注意目录要换成自己解压的jdk 目录
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  

//环境生效
source ~/.bashrc
```

### 安装kvm并创建虚拟机

```java
//https://blog.csdn.net/twotwo22222/article/details/126767604?
//开始Android Studio里面设备没创建好，就一直显示没有avd进程
//验证CPU是否支持硬件虚拟化
grep -Eoc '(vmx|svm)' /proc/cpuinfo //数字大于0，则代表CPU支持硬件虚拟化，反之则不支持
//检查 VT 是否在 BIOS 中启用
apt install cpu-checker //检查 VT 是否在 BIOS 中启用
kvm-ok //如果处理器虚拟化能力没有在 BIOS 中被禁用，命令将会打印出,否则，这个命令将会打印一个失败信息，和打印的消息
		INFO: /dev/kvm exists
		KVM acceleration can be used
//安装KVM
apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
systemctl is-active libvirtd
//是否安装成功
lsmod | grep kvm
//出现下面这个就对了
kvm_intel             434176  2
kvm                  1130496  1 kvm_intel

//启动虚拟化和设置开机自启
systemctl start libvirtd 
systemctl enable libvirtd
systemctl list-unit-files |grep libvirtd.service //打印启动虚拟化和设置开机自启情况
```

### 安装配置vscode

```shell
# 网站下载.deb文件
https://code.visualstudio.com/
# 安装
sudo dpkg -i Xxxxxamd64.deb

# 配置C/C++
sudo apt install g++

# 时序图
# https://www.graphviz.org/download/
sudo apt install graphviz
```

### Hexo 建站

```python
1
mkdir HexoBlog
//HexoBlog初始化博客
hexo init
```

```python
lihang@lihang-by-HP:~/Documents/HexoBlog$ hexo init
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
INFO  Install dependencies
(##################) ⠼ reify:moment: http fetch GET 200 https://registry.npmjs.org/moment/-/moment-2.29.4.tgz 63456ms (cache miss)
INFO  Start blogging with Hexo!
```

### markdown

```python
# 自动生成Github目录
# 在vscode中
# 快捷键CTRL(CMD)+SHIFT+P
# 输入Markdown All in One: Create Table of Contents回车即可
```

### IDER

```python
# 插件商城不能联网
右上角设置 -- check connecting -- 输入官网地址https://plugins.jetbrains.com/就行了

# 插件
CodeGlance 显示代码缩略图插件
Lombok 简化臃肿代码插件
Statistic 代码统计插件
Translation 翻译插件，使用有道或百度的API需要神奇ID和KEY配置好才能在国内用
SequenceDiagram for IntelliJ IDEA  类调用时序图 
```

### Python安装

```python
# 先安装pip
lihang@keary-by-hp:~$ sudo apt install python3-pip
# 再修改pip源
mkdir ~/.pip
vim ~/.pip/pip.conf
# 最后再安装pygame
lihang@keary-by-hp:~$ python3 -m pip install -U pygame
# 测试pygame是否安装成功，正常输入后回车会出现坦克大战飞机的游戏界面
lihang@keary-by-hp:~$ python3 -m pygame.examples.aliens
```

### Android Studio配置

```python
#插件下载太慢或者卡  添加阿里云的源url
buildscript {
    // Top-level variables used for versioning
    ext.kotlin_version = '1.5.21'
    ext.java_version = JavaVersion.VERSION_1_8

    repositories {
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'https://maven.aliyun.com/repository/google'}
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin'}
        maven { url 'https://maven.aliyun.com/repository/public'}
        google()
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.2.2'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.3.4"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'https://maven.aliyun.com/repository/google'}
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin'}
        maven { url 'https://maven.aliyun.com/repository/public'}
        google()
        mavenCentral()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```