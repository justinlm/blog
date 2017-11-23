---
title: Unity在windows平台中C++代码使用
date: 2017/11/21
tag: Unity
---
[Unity](https://unity3d.com/)中使用c++代码是以插件的形式来使用的。其他如Android的java代码也是类似，做成插件的形式。

## 那么就开始我们的表演

### 1.创建一个C++的vs空dll工程
如下图所示，然后点击完成，创建空的dll库工程
![createApp](/images/unity-use-c++-plugin-in-windows/createApp.png)

### 2.创建我们需要导出的.cpp文件FileUtil.cpp
```c++
#include <stdio.h>
extern "C" __declspec(dllexport) int SaveFile(char *lpcszSavePath, char *lpBuffer, int srcSize)
{
	printf("string is:%s\n",lpcszSavePath);

	FILE *pFile = fopen(lpcszSavePath, "wb+");
	int w = fwrite((const void *)lpBuffer, srcSize, 1, pFile);
	fflush(pFile);
	fclose(pFile);
	pFile = NULL;
	return w;
}
```
这里注意：
* 1 `extern "C"`关键字： Unity中如果使用c++（.cpp）或者objective-c(.mm)来实现插件，必须使用此关键字来表明函数是按照c语言方式编译和链接，避免[name mangling issues](https://en.wikipedia.org/wiki/Name_mangling)。
* 2 `__declspec(dllexport)`关键字：表明`SaveFile`这是一个导出给Unity调用的c++函数。

### 3.生成dll文件
* 1.一般我们最好生成两个dll库，一个32位一个64位，如果用的unity编辑器是64位的，那么在编辑器下就只会调用64位的库。
* 2.要生成64位或者32位的库，可以右键vs工程属性->配置管理器->平台->新建32位或者64位（ps：不同版本的vs，可能具体名字不一样）。
* 3.连接器-高级-目标计算机里面配置一下dll支持的目标机器类型。windows下32位dll一般配置MachineX86,64位dll配置MachineX64。


### 4.将dll运用到Unity工程中
* 1.在Unity工程中Plugins目录下创建x86_64目录，x86_64下面分别创建x86和x64目录，然后分别放入对应的dll文件
*  2.在需要调用到C++接口的C#类里面声明需要调用的函数声明
```c#
 [DllImport("libFileUtil")]
   public static extern int SaveFile(
   [MarshalAs(UnmanagedType.LPStr)]string lpcszSavePath,
   [MarshalAs(UnmanagedType.LPArray, ArraySubType = UnmanagedType.I1)]byte[] lpBuffer,int srcSize);
```
`[DllImport("libFileUtil")]`:表示导入的dll名字叫libFileUtil。
`MarshalAs`:指示如何在托管代码(c#)和非托管代码(C++)之间封送数据.

* 3.在需要调用使用调用的C++代码的地方调用
```c#
	SaveFile(SavePath, WWW.bytes, WWW.bytes.Length);//完成保存
```
到此就完成了C#调用C++代码保存文件的功能。