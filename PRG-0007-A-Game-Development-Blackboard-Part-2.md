# Game Development Blackboard - Part 2

## 2019-11-25 星期一

### 2D 背景图片循环滚动

* 方案一：控制 `Renderer.material.mainTextureOffset`，实现纹理的滚动效果
* 方案二：将无缝 Sprite 首尾相接，通过控制 GameObject 位置，实现滚动效果

* [Unity 2D 背景滚动 - CSDN](https://blog.csdn.net/wuhaishengxxx/article/details/53033095)
* [背景循环滚动 - 知乎专栏](https://zhuanlan.zhihu.com/p/53913978)

## 2019-11-21 星期四

### 2D 动画

* [Unity 2D 动画系统教程 - bilibli](https://www.bilibili.com/video/av38214700?p=1)
* [Introduction to 2D Animation - Unity Docs](https://docs.unity3d.com/Packages/com.unity.2d.animation@2.2/manual/index.html)
* [PSD Importer - Unity Docs](https://docs.unity3d.com/Packages/com.unity.2d.psdimporter@1.2/manual/index.html)
* [2D Inverse Kinematics（IK）- Unity Docs](https://docs.unity3d.com/Packages/com.unity.2d.ik@1.2/manual/index.html)

## 2019-11-14 星期四

### Object Management

* [Object Management - Catlike Coding](https://catlikecoding.com/unity/tutorials/object-management/)
* [Multiple Scenes - Catlike Coding](https://catlikecoding.com/unity/tutorials/object-management/multiple-scenes/)

### Unity City Builder Asset

* [Mobile Touch Camera - Unity Asset Store](https://assetstore.unity.com/packages/tools/camera/mobile-touch-camera-43960)
* [City Adventure](https://www.beffio.com/city-adventure)
* [RPG Medieval Kingdom Kit](https://www.beffio.com/medieval-kingdom)

## 2019-10-21 星期一

### C# 预编译符号

* 在 Visual Studio `项目 -> 属性 -> 生成 -> 常规`，`条件编译符号`中填入预编译符号，即可让该预编译符号在整个项目中生效。

### 在 Visual Studio 修改项目的程序集名称

* 在 Visual Studio 中，修改了项目名称之后，其相应的程序集名称并没有自动改变，需手动在`项目 -> 属性 -> 应用程序`，修改`程序集名称`和`默认命名空间`。

### .NET 程序集之间相互调用

同一个解决方案下的不同项目之间，或者说不同程序集之间，虽然可以通过引用，让处于依赖层次上层的程序集可以调用到下层的程序集，但由于禁止循环依赖，处于依赖层次下层的程序集，是无法直接调用上层程序集的。

以下两种方法，可以一定程度上实现程序集之间的相互调用。
以上层程序集 A 依赖于下层程序集 B，但下层程序集 B 需要调用上层程序集 A 中的 Model.ClassA 为例：

* 接口与反射

    - 在下层程序集 B 中定义接口 InterfaceB，在上层程序集 A 中，让 Model.ClassA 实现该接口
    - 在下层程序集 B 中，使用反射机制，通过检索程序域中程序集 A，获取 Model.ClassA 对象类型，然后创建该对象实例
    - 在下层程序集 B 中，就可以使用 InterfaceB 引用来持有上层程序集 A 中 Model.ClassA 的对象实例，并通过接口类中定义的接口方法来主动调用对象中的具体实现方法

```csharp
System.Reflection.Assembly[] assemblies = AppDomain.CurrentDomain.GetAssemblies();
foreach (System.Reflection.Assembly assembly in assemblies)
```

* 委托（事件系统）

    - 让上层程序集 A 和下层程序集 B 均引用/依赖于另一个程序集 C，在程序集 C 中实现通用的事件系统并定义事件类型
    - 在上层程序集 A 中订阅/监听事件，该事件是由下层程序集 B 分发/发送的
    - 以此方式即可实现下层程序集 B 调用上层程序集 A 中的具体方法实现

### 动态加载程序集

* [How do I get all assemblies in the solution?](https://www.codeproject.com/Questions/1089179/How-do-I-get-all-assemblies-in-the-solution)
* [Dynamically pre-load assemblies in a ASP.NET Core(or any C#) project](https://dotnetstories.com/blog/Dynamically-pre-load-assemblies-in-a-ASPNET-Core-or-any-C-project-en-7155735300)

```csharp
// Put the right path to the assembly you are trying to load here
string path = @"..\..\ConsoleApplication11\bin\Debug\ConsoleApplication11.exe"; 
```

### 《提问的智慧》

* [How to ask questions the smart way? - GitHub](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)

## 2019-10-14 星期一

### C# 命名规范

* [Naming Guidelines - Capitalization Styles - Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/dotnet/netframework-1.1/x2dbyw72%28v%3dvs.71%29)

**Pascal case (帕斯卡命名法/大驼峰命名法) :**

The first letter in the identifier and the first letter of each subsequent concatenated word are capitalized. You can use Pascal case for identifiers of three or more characters.
For example: `BackColor`.

**Camel case (驼峰命名法/小驼峰命名法) :**

The first letter of an identifier is lowercase and the first letter of each subsequent concatenated word is capitalized.
For example: `backColor`.
 
**Uppercase**

All letters in the identifier are capitalized. Use this convention only for identifiers that consist of two or fewer letters.
For example: `System.IO` and `System.Web.UI`.

## 2019-10-10 星期四

### C# 委托与匿名函数

* [我所理解的委托和匿名函数 - UWA](https://blog.uwa4d.com/archives/2072.html)

> 因为刚才讲到，委托是一种类型，而函数并不是类型，所以通常情况下，如果没有为两种类型提供转型操作，编译器会报出类型不匹配的错误。所以，既然我们这种写法能够通过编译器，那么说明编译器为我们实现了转型操作，比如隐式用于转型的构造函数。（知识点：隐式显示转型构造函数）。
> 如果委托的函数非常简单，或者基本上没有重复利用的可能，那么可以直接把函数体实现写在委托的生成处，这样你就不用费劲心思去思考该怎样给函数取一个容易的名字了，名字压根就不重要了，函数体那几句代码更易于理解代码在干什么，这就是匿名函数，也就是叫做 lambda 表达式的东西。
> 所以要正确使用匿名函数，必须清楚以下几点：

```csharp
// 用ILSpy反编译出来的代码：
```

```csharp
// 函数功能：打印从0~9这几个数字
        // 假如满足几率很小

// ====== 反编译后的代码：

void Error1() {

// ====== 修正后的正确代码：

void Error1() {
```

* [GCFreeClosure - GitHub](https://github.com/lujian101/GCFreeClosure)

### Unity 编辑器工具制作

* [Unity 手游开发札记（从零开始搭建手游开发的工具集）- 知乎](https://zhuanlan.zhihu.com/p/24557713)
* [Unity 工具类系列教程（配置化和规范教程）- 知乎](https://zhuanlan.zhihu.com/p/30042447)
* [Unity 工具类系列教程（代码自动化生成）- 知乎](https://zhuanlan.zhihu.com/p/30716595)
* [Unity 工具类系列教程（对象池）- 知乎](https://zhuanlan.zhihu.com/p/30575559)

### UGUI 使用

* [UGUI 系列教程（UGUI 基础与界面拼接）- 知乎专栏](https://zhuanlan.zhihu.com/p/28905447)
* [UGUI 系列教程（监听事件，完成解谜）- 知乎专栏](https://zhuanlan.zhihu.com/p/28906086)
* [UGUI 系列教程（OSU 动态界面制作）- 知乎专栏](https://zhuanlan.zhihu.com/p/28906293)
* [UGUI 系列教程（OSU Battle）- 知乎专栏](https://zhuanlan.zhihu.com/p/28906798)

## 2019-10-08 星期二

### Unity I18N

* [Unity I18N 小探 - 知乎](https://zhuanlan.zhihu.com/p/81159633)
* [I2 Localization - Unity Asset Store](https://assetstore.unity.com/packages/tools/localization/i2-localization-14884)

* [游戏国际化的一些建议 - 硬盘在歌唱](http://disksing.com/game-i18n)

> * 不要为每个语言版本建立单独分支
> * 尽量不要针对不同地区写特殊代码
> * 源代码中不能出现汉字或直接用于显示的字符串
> * 服务器代码或配置中不能出现汉字或直接用于显示的字符串
> * 尽量不要使用带文字的图片
> * 设计结构化的字典配置，减少冗余
> * 不用使用 %d、%s 等标识文本中的变量
> * 注意多义词
> * 留意 UI 中文字的长度问题

### Unity 项目资源目录与命名规范

* [Mastering Unity Project Folder Structure. Level 0 - Folders required for version control systems - DEV](http://developers.nravo.com/mastering-unity-project-folder-structure-level-1-reserved-folders/#.XZ3XOI4zYUF)
* [Mastering Unity Project Folder Structure. Level 1 - Reserved Folders - DEV](http://developers.nravo.com/mastering-unity-project-folder-structure-level-1-reserved-folders/#.XZ3XOI4zYUF)
* [Mastering Unity Project Folder Structure. Level 2 - Assets Organization](http://developers.nravo.com/mastering-unity-project-folder-structure-level-2-assets-organization/#.XZ3XQY4zYUE)
* [Best Practices - Folder Structure - Unity Forum](https://forum.unity.com/threads/best-practices-folder-structure.65381/)
* [Unity 工程目录规范 - Moses's Note](https://mrsoong.com/posts/2018-04-01-3743db2c.html)
* [Unity 项目目录结构与命名规则 - 腾讯云](https://cloud.tencent.com/developer/article/1141251)
* [自动化规范 Unity 资源的实践 - UWA](https://edu.uwa4d.com/course-intro/0/121)

### Unity 项目资源制作规范

* [Unity 美术资源制作规范 - Joyimp](http://www.joyimp.com/?post=161)
* [3D 模型与动画，美术资源的规范 - 技术人生](http://www.luzexi.com/2018/08/03/Unity3D%E9%AB%98%E7%BA%A7%E7%BC%96%E7%A8%8B%E4%B9%8B%E8%BF%9B%E9%98%B6%E4%B8%BB%E7%A8%8B-3D%E6%A8%A1%E5%9E%8B%E4%B8%8E%E5%8A%A8%E7%94%BB1.html)
* [Unity 美术资源规范整理 - 程序园](http://www.voidcn.com/article/p-qjjvpian-bcw.html)
* [Unity3D 美术资源规范 - 腾讯游戏学院](https://gameinstitute.qq.com/community/detail/128388)
* [美术资源标准（纹理篇） - Unity Connect](https://connect.unity.com/p/mei-zhu-zi-yuan-biao-zhun-wen-li-pian)

### Unity 项目编码规范

* [Unity 之命名规范（一）- 阿里云](https://yq.aliyun.com/articles/666181/)
* [Unity 之命名规范（二）- 阿里云](https://yq.aliyun.com/articles/666180?spm=a2c4e.11153940.0.0.609c684f0u4nXw)

-------

