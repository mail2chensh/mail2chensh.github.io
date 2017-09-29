---
title: IDA插件FRIEND编译操作实践
date: 2017-09-29 10:08:48
tags: [逆向]
---

# FRIEND

以下安装步骤来源于Git源：[FRIEND](https://github.com/alexhude/FRIEND)

翻译文章请[查看这里](http://chensh.top/2017/09/06/IDA-Plugin-FRIEND/)

## 依赖环境
要编译IDA的插件，需要具备以下一些依赖条件：
* CMake 3.3版本以上
* 在MacOS或Linux系统上需要GCC或者Clang编译器
* IDA SDK（本文基于6.8版本的，可以在[这里下载](https://pan.baidu.com/s/1sljt7GT)对应的sdk，密码: yekv）
* Hex-Rays SDK

### 如何获取 Hex-Rays SDK
在自己的ida应用包里面，可以将该文件夹拷贝出来。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fjxc4vezi7j314k0vgtkk.jpg)

### 如何安装CMake

可以到CMake的[官网上面](https://cmake.org/download/)下载对应平台的安装包。

安装后，可能在命令行里面依旧会找不到cmake这个命令，这个时候需要添加以下环境变量：

```
PATH="/Applications/CMake.app/Contents/bin":"$PATH"
```

这个时候cmake命令就可以在终端下使用了：

```
$ cmake 
Usage

  cmake [options] <path-to-source>
  cmake [options] <path-to-existing-build>

Specify a source directory to (re-)generate a build system for it in the
current working directory.  Specify an existing build directory to
re-generate its build system.

Run 'cmake --help' for more information.
```

## 安装步骤

### 克隆工程

将工程克隆到本地文件夹中。

```
$ git clone https://github.com/alexhude/FRIEND.git
```

### 创建编译文件夹
进入刚刚克隆下来的远程仓库，创建一个**build**文件夹。

```
$ mkdir _build
$ cd _build
```

这里我们用的是IDA6.8版本，所以需要增加一个编译选项 **DUSE_IDA6_SDK**，指定是否为6.X的版本。

```
$ cmake -DUSE_IDA6_SDK=ON ..
```

得到以下结果：

![](https://ws2.sinaimg.cn/large/006tNc79gy1fjxce5vbhmj317m0oojy9.jpg)

cmake会帮我们创建好相应的makefile。

### 拷贝SDK文件夹

将我们上面下载的**IDA SDK6.8**文件夹和**Hex-Rays SDK**文件夹拷贝到**FRIEND**文件夹目录下，替换原有的文件夹，效果如图：

![](https://ws2.sinaimg.cn/large/006tNc79gy1fjxcl0xvthj30bu0fk0tr.jpg)

### 开始编译插件

之后我们就可以运行命令开始编译插件了。

```
$ make
```

编译结果如下： 

```
Scanning dependencies of target capstone
[  3%] Creating directories for 'capstone'
[  6%] Performing download step (git clone) for 'capstone'
Cloning into 'capstone'...
Already on 'master'
Your branch is up-to-date with 'origin/master'.
[ 10%] Performing patch step for 'capstone'
HEAD is now at a279481d Fix pp field in readPrefix for VEX3 and EVEX (#1015) (#1016)
[ 13%] Performing update step for 'capstone'
Current branch master is up to date.
[ 16%] Performing configure step for 'capstone'
-- The C compiler identification is AppleClang 8.1.0.8020042
-- The CXX compiler identification is AppleClang 8.1.0.8020042
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_CXX_FLAGS_DEBUG
    CMAKE_CXX_FLAGS_MINSIZEREL
    CMAKE_CXX_FLAGS_RELEASE
    CMAKE_CXX_FLAGS_RELWITHDEBINFO
    CMAKE_C_FLAGS_DEBUG
    CMAKE_C_FLAGS_MINSIZEREL
    CMAKE_C_FLAGS_RELEASE
    CMAKE_C_FLAGS_RELWITHDEBINFO


-- Build files have been written to: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/src/capstone-build
[ 20%] Performing build step for 'capstone'
Scanning dependencies of target capstone-static
[  6%] Building C object CMakeFiles/capstone-static.dir/cs.c.o
[ 12%] Building C object CMakeFiles/capstone-static.dir/MCInst.c.o
[ 18%] Building C object CMakeFiles/capstone-static.dir/MCInstrDesc.c.o
[ 25%] Building C object CMakeFiles/capstone-static.dir/MCRegisterInfo.c.o
[ 31%] Building C object CMakeFiles/capstone-static.dir/SStream.c.o
[ 37%] Building C object CMakeFiles/capstone-static.dir/utils.c.o
[ 43%] Building C object CMakeFiles/capstone-static.dir/arch/ARM/ARMDisassembler.c.o
[ 50%] Building C object CMakeFiles/capstone-static.dir/arch/ARM/ARMInstPrinter.c.o
[ 56%] Building C object CMakeFiles/capstone-static.dir/arch/ARM/ARMMapping.c.o
[ 62%] Building C object CMakeFiles/capstone-static.dir/arch/ARM/ARMModule.c.o
[ 68%] Building C object CMakeFiles/capstone-static.dir/arch/AArch64/AArch64BaseInfo.c.o
[ 75%] Building C object CMakeFiles/capstone-static.dir/arch/AArch64/AArch64Disassembler.c.o
[ 81%] Building C object CMakeFiles/capstone-static.dir/arch/AArch64/AArch64InstPrinter.c.o
[ 87%] Building C object CMakeFiles/capstone-static.dir/arch/AArch64/AArch64Mapping.c.o
[ 93%] Building C object CMakeFiles/capstone-static.dir/arch/AArch64/AArch64Module.c.o
[100%] Linking C static library libcapstone.a
[100%] Built target capstone-static
[ 23%] Performing install step for 'capstone'
[100%] Built target capstone-static
Install the project...
-- Install configuration: ""
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/arm64.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/arm.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/capstone.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/mips.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/ppc.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/x86.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/sparc.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/systemz.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/xcore.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/include/capstone/platform.h
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/capstone/lib/libcapstone.a
[ 26%] Completed 'capstone'
[ 26%] Built target capstone
Scanning dependencies of target pugixml
[ 30%] Creating directories for 'pugixml'
[ 33%] Performing download step (git clone) for 'pugixml'
Cloning into 'pugixml'...
error: RPC failed; curl 56 SSLRead() return error -9806
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed
Cloning into 'pugixml'...
-- Had to git clone more than once:
          2 times.
Already on 'master'
Your branch is up-to-date with 'origin/master'.
[ 36%] No patch step for 'pugixml'
[ 40%] Performing update step for 'pugixml'
Current branch master is up to date.
[ 43%] Performing configure step for 'pugixml'
-- The C compiler identification is AppleClang 8.1.0.8020042
-- The CXX compiler identification is AppleClang 8.1.0.8020042
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_CXX_FLAGS_DEBUG
    CMAKE_CXX_FLAGS_MINSIZEREL
    CMAKE_CXX_FLAGS_RELEASE
    CMAKE_CXX_FLAGS_RELWITHDEBINFO
    CMAKE_C_FLAGS_DEBUG
    CMAKE_C_FLAGS_MINSIZEREL
    CMAKE_C_FLAGS_RELEASE
    CMAKE_C_FLAGS_RELWITHDEBINFO


-- Build files have been written to: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/pugixml/src/pugixml-build
[ 46%] Performing build step for 'pugixml'
Scanning dependencies of target pugixml
[ 50%] Building CXX object CMakeFiles/pugixml.dir/src/pugixml.cpp.o
[100%] Linking CXX static library libpugixml.a
[100%] Built target pugixml
[ 50%] Performing install step for 'pugixml'
[100%] Built target pugixml
Install the project...
-- Install configuration: ""
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/pugixml/lib/libpugixml.a
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/pugixml/include/pugixml.hpp
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/pugixml/include/pugiconfig.hpp
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/pugixml/lib/cmake/pugixml/pugixml-config.cmake
-- Installing: /Users/chensh/myDoc/workSpace/FRIEND/_build/third_party/pugixml/lib/cmake/pugixml/pugixml-config-noconfig.cmake
[ 53%] Completed 'pugixml'
[ 53%] Built target pugixml
Scanning dependencies of target FRIEND.pmc64
[ 56%] Building CXX object CMakeFiles/FRIEND.pmc64.dir/FRIEND/AArch64Extender.cpp.o
[ 60%] Building CXX object CMakeFiles/FRIEND.pmc64.dir/FRIEND/AArch32Extender.cpp.o
[ 63%] Building CXX object CMakeFiles/FRIEND.pmc64.dir/FRIEND/Documentation.cpp.o
[ 66%] Building CXX object CMakeFiles/FRIEND.pmc64.dir/FRIEND/FRIEND.cpp.o
[ 70%] Building CXX object CMakeFiles/FRIEND.pmc64.dir/FRIEND/FunctionSummary.cpp.o
[ 73%] Building CXX object CMakeFiles/FRIEND.pmc64.dir/FRIEND/Settings.cpp.o
[ 76%] Linking CXX shared module FRIEND.pmc64
[ 76%] Built target FRIEND.pmc64
Scanning dependencies of target FRIEND.pmc
[ 80%] Building CXX object CMakeFiles/FRIEND.pmc.dir/FRIEND/AArch64Extender.cpp.o
[ 83%] Building CXX object CMakeFiles/FRIEND.pmc.dir/FRIEND/AArch32Extender.cpp.o
[ 86%] Building CXX object CMakeFiles/FRIEND.pmc.dir/FRIEND/Documentation.cpp.o
[ 90%] Building CXX object CMakeFiles/FRIEND.pmc.dir/FRIEND/FRIEND.cpp.o
[ 93%] Building CXX object CMakeFiles/FRIEND.pmc.dir/FRIEND/FunctionSummary.cpp.o
[ 96%] Building CXX object CMakeFiles/FRIEND.pmc.dir/FRIEND/Settings.cpp.o
[100%] Linking CXX shared module FRIEND.pmc
[100%] Built target FRIEND.pmc
```

上面的编译过程，首先会下载两个第三方的库，一个是**capstone**
,一个是 **pugixml**。
然后开始编译目标二进制，最重得到我们需要的插件文件:

**FRIEND.pmc**和**FRIEND.pmc64**

![](https://ws2.sinaimg.cn/large/006tNc79gy1fjxcpaxyc8j30mw0dm0ui.jpg)

### 拷贝插件

将生成的两个文件，拷贝到应用的插件目录下，然后重启idaq应用程序。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fjxcqsn8azj31240ouk0y.jpg)

重启进入应用，我们随便打开一个二进制文件，然后就可以在Editor菜单项下的plugin看到我们新安装的插件：

![](https://ws3.sinaimg.cn/large/006tNc79gy1fjxcryinh4j30vo0x018w.jpg)

### 加载XML

在我们克隆的项目工程，里面有个文件夹**Configurations**，存放了一些xml文件。


![](https://ws1.sinaimg.cn/large/006tNc79gy1fjxcujtjobj30n60esq4h.jpg)


我们首次使用FRIEND的时候，需要加载一些xml配置，来显示对应的高亮关键词。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fjxcsg7vn6j30x40sm40u.jpg)


### 体验

配置好后，当我们鼠标定位到一些关键词后，我们就可以看到效果了。

![](https://ws1.sinaimg.cn/large/006tNc79gy1fjxcsjzn8qj31120oqqbo.jpg)



