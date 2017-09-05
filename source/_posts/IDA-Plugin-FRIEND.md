---
title: 'IDA Plugin: FRIEND'
date: 2017-09-06 00:00:26
tags: [iOS, 翻译]
---

# FRIEND

一个灵活查看寄存器和指令的文档扩展工具。

**原文地址：** [FRIEND](https://github.com/alexhude/FRIEND)

**翻译：** Chensh


## 特性
FRIEND是一个IDA插件，用于改善反汇编以及在IDA视图里面展示寄存器和指令文档。

1. 使用第三方库来改进处理器模块。（例如Capstone）
   ![](https://ws4.sinaimg.cn/large/006tKfTcgy1fj7jnfyw32j310v0dvq6g.jpg)


2. 再IDA视图和反汇编视图里，对指令和寄存器进行提示。
   ![](https://ws3.sinaimg.cn/large/006tKfTcgy1fj7jphlxu4j319514ltlf.jpg)

3. 在浏览器中显示高亮选项的扩展引用。
   ![](https://ws3.sinaimg.cn/large/006tKfTcgy1fj7jrby7a9j31kw0mv128.jpg)

4. 在IDA视图和反汇编视图里显示函数功能摘要。
   ![](https://ws3.sinaimg.cn/large/006tKfTcgy1fj7jtk62wej31kw0wi15v.jpg)

5. 可以选择你感兴趣的元素进行展示。
   ![](https://ws4.sinaimg.cn/large/006tKfTcgy1fj7ju6xpykj313k0ywjx4.jpg)



## 如何编译

### 准备编译环境

想要编译IDA插件，需要满足以下的依赖：

* CMake 版本为3.3或更高。
* 再Linux或者MacOS上面使用GCC或者Clang；Windows上面使用Visual Studio 2015。
* Git。
* IDA SDK（解压为``idasdk``）
* Hex-Rays SDK（拷贝并重命名为 ``hexrays_sdk``）

将IDA SDK的内容解压到``idasdk``文件夹内。并将Hex-Rays SDK拷贝并重命名为``hexrays_sdk``。在Linux或MacOS上面，你可以使用以下命令操作：

```
$ unzip /path/to/idasdk69.zip -d idasdk
$ mv idasdk/idasdk69/* idasdk
$ rm -r idasdk/idasdk69
$ cp -r /path/to/ida/plugins/hexrays_sdk hexrays_sdk
```

### Linux

使用 ``cmake`` 命令准备编译环境，并运行 ``make`` 命令来编译插件:

```sh
$ mkdir _build
$ cd _build
$ cmake ..
$ make
```

### MacOS

使用 ``cmake`` 命令准备编译环境，并运行 ``make`` 命令来编译插件:

```sh
$ mkdir _build
$ cd _build
$ cmake ..
$ make
```

如果你更倾向于使用Xcode工程来进行编译，那你可以运行以下命令来替换上面的步骤：

```sh
$ mkdir _build
$ cd _build
$ cmake -G Xcode ..
$ open FRIEND.xcodeproj # or simply run xcodebuild
```

### Windows

使用 ``cmake`` 命令准备编译环境，并运行 ``make`` 命令来编译插件:

```sh
$ mkdir _build
$ cd _build
$ "%VS140COMNTOOLS%\..\..\VC\vcvarsall.bat" x86
$ cmake -G "Visual Studio 14 2015" ..
$ msbuild FRIEND.sln /p:Configuration=Release
```

## 安装

将编译出来的二进制文件拷贝到IDA Pro的插件目录里。以下是不同平台的默认路径：

| 系统      | 插件目录                                     |
| ------- | ---------------------------------------- |
| Linux   | `/opt/ida-6.95/plugins`                  |
| macOS   | `/Applications/IDA Pro 6.95/idabin/plugins` |
| Windows | `%ProgramFiles(x86)%\IDA 6.95\plugins`   |

## 配置文件

内容讨论请查看 [这里](https://github.com/alexhude/FRIEND/issues/1)

FRIEND 配置文件遵循以下文件结构:

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<documentation>
	<document id="pdf_id" name="ARM Architecture Reference Manual" version="A.k">
		<path>/path/to/your/pdf/or/link</path>
	</document>
	<elements>
		<group type="reg" name="Group Name">
			<hint page="1" header="Element Header" doc_id="pdf_id" token="R0">info</>
			...
		</group>
		<group type="ins" name="Group Name">
			<hint page="2" header="Element Header" doc_id="pdf_id" token="MOV">info</>
			...
		</group>
		...
	</elements>
</documentation>
```

注意：你需要将你自己的pdf文件路径填充到上面的\<path\>标签，否则无法在浏览器进行文档扩展。

## 提示编辑器

为了使得本工程更加容易使用，这里有一个简单的配置编辑器。

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fj7ktpnl37j30vx0ikjv7.jpg)

__注意:__ 这个编辑器只能在MacOS以及使用Xcode8及以上的环境才能够编译，其他系统不支持。

使用 ``cmake`` 命令来生成Xcode工程。

```sh
$ cd HintEditor/HintEditor/
$ mkdir _build
$ cd _build
$ cmake -G Xcode ..
$ xcodebuild
```

使用``open``命令来启动应用程序：

```
$ open Debug/HintEditor.app
```

## 依赖

FRIEND 依赖要求:  
- [IDA SDK](https://www.hex-rays.com/products/ida/support/download.shtml)   
- [Capstone](https://github.com/aquynh/capstone) (使用 Patches/capstone.diff 分支编译)  
- [pugixml](https://github.com/zeux/pugixml)

提醒编辑器依赖要求：
- [AEXML](https://github.com/tadija/AEXML) (使用 Patches/aexml.diff 分支编译)  

## 贡献者

@ **in7egral, mbazaliy** for bug reports and all kind of support    
@ __qwertyoruiopz, iH8sn0w, Morpheus\_\_\_\_\_\_, xerub, msolnik, marcograss, pr0x13, \_argp, oleavr, brinlyau__ and other gang for inspiration  
@ __\_kamino\___ for porting project to Windows and Linux  
@ __williballenthin__ for the idea of function summary