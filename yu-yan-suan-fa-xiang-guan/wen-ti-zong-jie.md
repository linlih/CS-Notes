# 问题总结

## ubuntu 14.04 cmake版本更新

直接使用apt-get install cmake得到的版本是2.8的，如果要安装3.x版本，方法如下：

```bash
sudo add-apt-repository ppa:george-edison55/cmake-3.x
sudo apt update
sudo apt install cmake
```

如何上面的方式安装的版本还是不满足的话，可以选择自行下载对应版本编译安装：

```bash
wget http://www.cmake.org/files/v3.10/cmake-3.10.1.tar.gz
tar -xvzf cmake-3.10.1.tar.gz
cd cmake-3.10.1/
sudo ./configure
sudo make
sudo make install
```

## OpenCV在ubuntu平台上的安装

下面这个链接是官方提供的安装github上最新版本的：

{% embed url="https://docs.opencv.org/3.1.0/d7/d9f/tutorial\_linux\_install.html" %}

Ubuntu上安装3.4版本

{% embed url="https://www.linuxidc.com/Linux/2019-05/158462.htm" %}

## 运行service，返回unknown job

解决办法，加上sudo即可，原因如下：

{% embed url="https://www.cnblogs.com/linuxzxy/p/6542884.html" %}



