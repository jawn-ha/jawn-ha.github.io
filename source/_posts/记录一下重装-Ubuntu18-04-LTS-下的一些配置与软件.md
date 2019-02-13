---
title: 记录一下重装 Ubuntu18.04 LTS 下的一些配置与软件
date: 2018-07-03 22:27:20
tags: Ubuntu
---

以前一直用着 windows 和 ubuntu 的双系统，但ubuntu可能装的软件太多且一直缺乏管理导致系统流畅性差，不稳定，于是花了一下午装了最新版的 ubuntu 的最新版本 18.04 LTS。并记录一下一些个人配置以及软件以备不时只需。希望下次能换 MAC！

<!--more-->

### 1. SS 科学上网

####  1.1 Shadowsocks-qt5 客户端（推荐）

* 安装，终端内依次输入下面指令：

```code
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

* 目前因为18.04海狸(bionic)库中还没有项目，所以报错，如下：

```code
忽略: http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic InRelease
错误: http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic Release
404  Not Found [IP:91.189.95.83 8
```

* 把里面的　ppa源中的bionic改成xenial就可以了，执行:

```code
sudo vim /etc/apt/sources.list.d/hzwhuang-ubuntu-ss-qt5-bionic.list
```

把里面的bionic改成xenial保存就好了，接着安装就可以了。

* 最后会在Dash里有启动器，图形界面非常容易配置的。

####  1.2 普通版 的 shadowsocks

- 用pip安装，在Ubuntu下打开终端依次输入以下指令：

  ```
  sudo apt-get update
  sudo apt-get install python-pip
  sudo apt-get install python-setuptools m2crypto
  pip install shadowsocks
  ```

- apt安装，Python3.4之后好像不默认装pip了，可以直接apt安装：

  ```
  sudo apt install shadowsocks
  ```

  安装时候可能有提示需要安装一些依赖比如python-setuptools m2crypto ，依照提示安装然后再安装就好。

- 启动shadowsocks

  1. 用sslocal，在终端输入sslocal –help 可以查看帮助，通过帮助提示我们知道各个参数怎么配置；比如 sslocal -c 后面加上我们的json配置文件，或者像下面这样直接命令参数写上运行。比如：

     ```
     sslocal -s 11.22.33.44 -p 8388 -k "123456" -l 1080 -t 600 -m aes-256-cfb`
     ```

     -s表示服务IP, -p指的是服务端的端口，-k 是密码（要加” ”）, -l是本地端口默认是1080，-t超时默认300，-m是加密方法默认aes-256-cfb。

  2. 为了方便我推荐直接用sslcoal -c 配置文件路径。 我们可以在 /home/azj/ 下新建个文件 shadowsocks.json (azj是我在我电脑上的用户名，这里路径看你自己的)。内容是这样：

     ```
     {
         "server":"X.X.X.X",
         "server_port":8388,
         "local_port":1080,
         "password":"123456",
         "timeout":300,
         "method":"aes-256-cfb"
     }
     ```

     server 你服务端的IP，
     servier_port 你服务端的端口,
     local_port 本地端口，一般默认1080，
     password ss服务端设置的密码，
     timeout 超时设置和服务端一样，
     method 加密方法和服务端一样。
     确定上面的配置文件没有问题，然后我们就可以在终端输入：
     `sslocal -c /home/azj/shadowsocks.json -d -start`
     回车运行。

####  1.3配置PAC模式

在系统设置里网络设置中的网络代理选项选择自动，在URL框中填入本地PAC文件路径“file:///home/jawnha/PAC/AutoProxy.pac”。 [Pac文件](https://doub.io/ss-jc59/)可以从这里获取。



###  2.Chrome 浏览器

####  2.1 安装

### 2.1 安装

1. 私有源安装，终端内依次输入以下指令：
   1. 将chrome下载源添加到系统的源列表： `sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/`
   2. 导入谷歌软件的公钥,可能等待时间比较长： `wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -`
   3. 更新系统： `sudo apt-get update`
   4. 安装chrome： `sudo apt-get install google-chrome-stable`
      然后安装完成后就可以使用了,两个命令对应着不同的版本,一个是正常,一个是稳定版。
      正常版: `/usr/bin/google-chrome`
      稳定版: `/usr/bin/google-chrome-stable`
2. 科学上网的情况下直接去Chrome下载deb双击安装或者`dpkg -i \<deb包名>`。 

####  2.3Flash 插件

1.Chrome安装完后发现flash无法播放，显示“没有安装flash插件”，因为AdobeFlash不再支持linux，So, Google便开发了PepperFlashPlayer来替代原来的AdobeFlash。
下面介绍 PepperFlashPlayer的安装方法：

1. 安装，在终端里输入下面的命令： `sudo apt-get install pepperflashplugin-nonfree`

2. 更新 ，在终端里输入下面的命令代码： `sudo update-pepperflashplugin-nonfree --install`

3. 查看安装的PepperFlashPlayer版本，在终端里输入下面的命令： `sudo update-pepperflashplugin-nonfree --status`

4. 在终端里输入下面的命令代码：

   ```code
   sudo mkdir /opt/google/chrome/PepperFlash
   sudo cp /usr/lib/pepperflashplugin-nonfree/libpepflashplayer.so  /opt/google/chrome/PepperFlash
   ```

   重启浏览器。

  2.Chrome安装完后发现某些网页flash无法播放，显示“该插件不受支持”，一般是因为flash插件版 本低与该网站要求(grd腾讯视频最典型),一般此时更新一下flash插件就好了：

      1. 可以用\1.中的方法，倘若不行则将libpepflashplayer.so文件复制到”~/.config/google-chrome/PepperFlash/30.0.0.113/“里，没有该目录就创建一个,“30.0.0.113”是当前flash插件的版本可以从\1.中指令查到或者随便写一个。重启浏览器。
      2. 倘若还不行就去AdobeFlash官网下载对应环境的tar包“flash_player_ppapi_linux.x86_64”，解压后把所有目录内文件移到”~/.config/google-chrome/PepperFlash/30.0.0.113/“里。同样重启浏览器。
      3. 或者在以上科学上网的情况下终端如下开启Chrome全局代理：
      `google-chrome --proxy-server="socks5://127.0.0.1:8119"`
      在打开的浏览器内输入：`chrome://components` 进入组件页点击更新。
      完了重启浏览器输入：`chrome://flash` 查看flash版本。

### 3. 常用软件

####  3.1 文本编辑

##### 3.1.1搜狗输入法

##### 3.1.1 搜狗Pinyin

[配置地址](https://www.cnblogs.com/zhangfengfly/p/6867844.html)

##### 3.1.2 剪贴板Ditto

搜狗自带剪切板插件可以自己配置一下。
Linux下的剪贴板软件感觉始终不如windows下（ditto）好用，推荐一个ClipIt：
保存上一个拷贝项的历史记录；
针对最常用功能的全局热键；
声明静态项；将特定项从历史记录中排除；
可搜索的历史记录及更多功能 ；
用的时候，点击任务栏上图标，选取所需片段。
终端安装：

```
sudo add-apt-repository ppa:shantzu/clipit
sudo apt-get update
sudo apt-get install clipit
```

 


#####  3.1.3 WPS 安装

[deb包官网下载地址](http://community.wps.cn/download/)
[依赖冲突解决](https://blog.csdn.net/zhuiqiuk/article/details/53644545)

##### 3.1.4 Markdown编辑

1. 有颜值又有实力的markdown编辑器 typora，安装过程如下：

   ```
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
   sudo add-apt-repository 'deb https://typora.io ./linux/'
   sudo apt-get update
   sudo apt-get install typora
   ```

2. Moeditor，简洁好用又好看，支持全平台，Menci大佬开发。支持deb包安装和自己构建安装。

   - github上已经开源：“[https://github.com/Moeditor/Moeditor”](https://github.com/Moeditor/Moeditor%E2%80%9D)
   - 主页以及下载：“[https://moeditor.org/”](https://moeditor.org/%E2%80%9D)

3. 安装VIM （大佬们的最爱，插件自己配）
   `apt-get install vim`

#### 3.2 图像处理

##### 3.2.1 截屏软件

Shutter是值得推荐的一款截图软件，功能丰富，堪称神器。
终端安装：`sudo apt-get install shutter`

##### 3.2.2 GIMP

[安装教程](https://www.linuxidc.com/Linux/2015-12/125835.htm)

#### 3.3 视频音频播放器

##### 3.3.1 VLCMedia Player(格式广)

终端安装 `sudo apt-get install vlc`
或者自带软件商店内下载。

##### 3.3.2 gnome播放器

自带软件商店内下载。

#### 3.3.3 SMPlayer

终端安装：

```
sudo apt-add-repository ppa:rvm/smplayer
sudo apt-get update
sudo apt-get install smplayer smplayer-skins smplayer-themes
```

##### 3.3.4 网易云音乐

[deb包官网下载地址](https://link.zhihu.com/?target=http%3A//music.163.com/%23/download)

##### 3.3.5 虾米音乐

另外，有大神用Electron构建了Linux端虾米音乐Electron Xiami，需要的朋友自己摸索。
<https://github.com/eNkru/electron-xiami>

##### 3.3.6 FFmpeg

很好的音频视频处理软件。终端安装：

```
sudo add-apt-repository ppa:djcj/hybrid
sudo apt-get update
sudo apt-get install ffmpeg
```

##### 3.3.7 录屏软件

Simple Screen Recorder是一款简单的屏幕录像工具，能够在屏幕上录制视频、教程，界面简单，功能够用。终端安装：

```
sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
sudo apt-get update
sudo apt-get install simplescreenrecorder
```

### 4 系统软件

#### 4.1 系统优化

##### 4.1.1 点击图标最小化

终端输入： `gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'`。

#### 4.1.2 sysmonitor

资源监视监视器：
[教程地址](https://www.cnblogs.com/EasonJim/p/7130171.html)。

#### 4.1.3 支持exfat文件

终端安装：`sudo apt-get install exfat-fuse exfat-utils`。

#### 4.1.x 装逼

[十个装逼有意思的玩意儿](https://www.linuxidc.com/Linux/2013-12/94300.htm)

### 4.2 系统美化

使用Tweaks对gnome进行美化，终端安装：

```
sudo apt-get install gnome-tweak-tool  #安装tweak
sudo apt-get install gnome-shell-extensions -y  #安装shell扩展
sudo apt install chrome-gnome-shell  #为了能在火狐和谷歌浏览器内安装gnome插件
sudo apt-get install gtk2-engines-pixbuf  #防止GTK2错误
sudo apt install libxml2-utils
```

接下来安装主题和图标，主要从[gnome-look](https://www.gnome-look.org/browse/ord/top/)这里下载，下面举例一个。
我从网站找到Gnome-OSC主题，这是一款仿MAC OS的主题：

```
mkdir ~/Themes
cd ~/Downloads
```

我下载的两个包是：

```
Gnome-OSC-HS-light-menu– 2-themes.tar.xz 
Gnome-OSC-HS–2-themes.tar.xz
```

接下来解压到指定文件夹，并安装他们。

```
xz -d Gnome-OSC-HS-light-menu*.tar.xz
tar -xvf Gnome-OSC-HS-light-menu*.tar -C ~/Themes
xz -d Gnome-OSC-HS--2*.tar.xz
tar -xvf Gnome-OSC-HS--2*.tar -C ~/Themes
cd ~/Themes
sudo cp -R ~/Themes/Gnome-OSC* /usr/share/themes/
```

还有一款扁平化主题也不错。

```
sudo add-apt-repository ppa:daniruiz/flat-remix
sudo apt-get update
sudo apt-get install flat-remix-gnome
```

图标papirus还不错

```
sudo add-apt-repository ppa:papirus/papirus
sudo apt update 
sudo apt-get install papirus-icon-theme
```

重启！然后就可以在Tweak-tools里看见这些主题了。

#### 4.3 安装压缩软件

终端安装：`sudo apt-get install p7zip-full p7zip-rar rar unzip`

#### 4.4 Htop

终端安装：`sudo apt-get install htop`

#### 4.5 Guake Terminal（F12…）

终端安装：`sudo apt-get install guake`

#### 4.6 albert神器

这个可以考虑开机启动albert：
[项目地址](https://github.com/albertlauncher/albert)

### 5. 开发工具安装

#### 5.1 sublime编辑器

对于那些不喜欢snap包的人，你可以从它的官方apt库安装Sublime Text 3。

```code
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

卸载命令：
`sudo apt-get remove sublime-text && sudo apt-get autoremove`

#### 5.2 webstorm安装

官网下载直接安装就好，网上破解方法也挺多。



感谢阅读，方法大都来源于网络，多查多问便能全部解决了！



