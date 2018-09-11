---
layout:     post
title:      "caffe中的妖娥子"
author:     "xcandy"
header-img: "img/post-bg-2015.jpg"
tags:
    - EnvConfig
---

> 本文主要汇总了在编译caffe过程中的各种妖娥子问题

## 正文

Error 1:
```
#  error "OpenCV 4.x+ requires enabled C++11 support"
```
主要引起原因是caffe切换至C++ 11作为标准。
解决方法：可以在cmake 中增加编译选项-DCMAKE_CXX_FLAGS="-std=c++11"

Error 2:
```
缺少OpenCV配置环境
```
解决方法：在编译前事先使用
export OpenCV_DIR=XXX/XXX/opencv-install
后在进行编译

Error 3:
```
提示protobuf版本过低
```
解决方法:

```	
sudo apt-get purge libprotobuf  
sudo apt-get purge protobuf-compile
mkdir protobuf
```

```	
cd protobuf
sudo apt-get install autoconf automake libtool curl make g++ unzip
git clone https://github.com/google/protobuf.git 
./autogen
./configure --prefix=/usr
make -j8
sudo make install
```
---------------------------------------------------------------------- 
http://blog.fangchengjin.cn/reid-market-1501.html


