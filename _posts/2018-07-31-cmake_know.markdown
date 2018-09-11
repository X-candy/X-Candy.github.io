---
layout:     post
title:      "cmake常用小技巧"
author:     "xcandy"
header-img: "img/post-bg-2015.jpg"
tags:
    - EnvConfig
---

> 本文主要汇总了在编写CMakeList中所使用的一些常见的技巧

## 正文

### 制定输出目录
```
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/../../../Publish/Lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/../../../Publish/Lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/../../../Publish/Bin)
message(STATUS "Output Path:${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}")
```

### 读取git或者svn等版本号作为文件版本
CMakeList 添加如下语句:

```
SET(ROOT_DIR ${PROJECT_SOURCE_DIR})
message(STATUS "Git Address:${ROOT_DIR}/.git/")
IF(EXISTS "${ROOT_DIR}/.git/")
    message(STATUS "Found Git Repository")
    FIND_PACKAGE(Git)
    message(STATUS "${CMAKE_CURRENT_SOURCE_DIR}")
    exec_program(
      "git"
      ARGS "log --format='\"[sha1]:%h [author]:%cn [time]:%ci [commit]:%s [branch]:%d\"' -1"
      ${CMAKE_CURRENT_SOURCE_DIR}
      OUTPUT_VARIABLE VERSION_SHA1 )
    add_definitions( -DGIT_SHA1="${VERSION_SHA1}" )
    set(VERSION_INFO "${VERSION_SHA1}")
ENDIF(EXISTS "${ROOT_DIR}/.git/")
configure_file (
        "Version.h.in"
        "Version.h"
)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
```

在src中增加Version.h.in文件,内容如下:

```
// the configured options and settings for Tutorial
#define VERSION_INFO @VERSION_INFO@
```

### 制定输出名字
```
add_library(${Project_Name} SHARED ISRWatcher.h SRWatcher.cpp SRWatcher.h)
#debug 模式输出名字
SET_TARGET_PROPERTIES(${Project_Name} PROPERTIES OUTPUT_NAME_DEBUG SRWatcher)
#不区分模式
SET_TARGET_PROPERTIES(${Project_Name} PROPERTIES OUTPUT_NAME SRWatcher)
```

### git 设置文本编辑器
```
#设置文本编辑器
git config --global core.editor vi
#设置提交模板
git config --global commit.template .gitee/PULL_REQUEST_TEMPLATE.zh-CN.md
```

### 快速添加文件
```
file(GLOB cudaSources cudaUtils/*.cu)
cudaSources  变量名称
cudaUtils/*.cu  类型文件
```


