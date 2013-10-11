---
layout: post
title: "ubuntu12.04安装amd显卡驱动"
date: 2013-10-12 02:02
author: Kevin
comments: true
categories: 
- OS
- Linux
---
## ubuntu12.04安装amd显卡驱动
参考[文章](http://forum.ubuntu.org.cn/viewtopic.php?f=42&t=439130), 其中对于一些小问题进行了改善

我的机器的配置是：

* CPU:	i5-4430
* GPU:	HD7850
* OS:	ubuntu12.04_64


>根据显卡型号来下载特定的显卡驱动，其实可以用开源的显卡驱动，但是对于AMD来说，闭源的显卡驱动比开源的要好，所以我们就直接在AMD[官网](http://support.amd.com/cn/Pages/AMDSupportHub.aspx)上下载就行

####卸载已有的驱动(如果没有，可以无视这个步骤)

<pre><code>
sudo sh /usr/share/ati/fglrx-uninstall.sh
sudo apt-get remove --purge fglrx fglrx_* fglrx-amdcccle* fglrx-dev*
sudo apt-get remove --purge xorg-driver-fglrx xserver-xorg-video-ati xserver-xorg-video-radeon
</code></pre>

###更新源
<pre><code>
sudo apt-get update
</pre></code>


###安装打包支持环境
<pre><code>
sudo apt-get install dpkg build-essential cdbs dh-make dkms execstack dh-modaliases fakeroot libqtgui4 dpkg-dev

sudo apt-get install build-essential cdbs fakeroot dh-make debhelper debconf libstdc++6 dkms libqtgui4 wget execstack libelfg0 dh-modaliases
</pre></code>

###安装ia32位库
<pre><code>
sudo apt-get install ia32-libs
sudo apt-get install ia32-libs-multiarch:i386 lib32gcc1 libc6-i386
</pre></code>

###解压下载下来的驱动，然后进入安装目录，执行：

<pre><code>
sudo chmod +x amd-driver-installer-catalyst-13-4-x86.x86_64.run

sudo sh ./amd-driver-installer-catalyst-13-4-x86.x86_64.run --buildpkg Ubuntu/raring
</pre></code>
这会生成三个文件：fglrx_12.104-0ubuntu1_amd64.deb， fglrx-amdcccle_12.104-0ubuntu1_amd64.deb，fglrx-dev_12.104-0ubuntu1_amd64.deb 

###安装打包好的deb文件
<pre><code>
sudo dpkg -i fglrx*.deb
</code></pre>

###重启
在上述命令完成之后，重启电脑，发现电脑竟然不能全屏，这让我很是纠结，上网找了N多资料，最终搞定了。
若是遇到非全屏的问题，执行：
<pre><code>
sudo amdconfig --set-pcs-val=MCIL,DigitalHDTVDefaultUnderscan,0
sudo reboot
</code></pre>
这中间如果出现关于config文件找不到的问题，需要init一下config文件，不过错误提示中会指导你怎么做，所以不必担心。


重启之后发现完全OK啦，好吧，这个问题也就搞定了。