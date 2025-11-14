---
title: Cpp知识整理(一)
math: true
---
# 1. 初始化列表
[初始化列表博客](https://www.cnblogs.com/ArsenalfanInECNU/p/18080526)
具体参考上面的博客就好，值得注意的是()圆括号的方法是C++11之前就可以使用了，{}的才是C++11新增的

# 2. 安装boost

## 下载可执行文件
[# win10下boost安装的三种方式，及cmake编译boost代码例子](https://blog.csdn.net/luozhichengaichenlei/article/details/122490831)
- [下载地址](https://sourceforge.net/projects/boost/files/boost-binaries/)
- 以boost_1_72_0-msvc-14.2-64.exe为例，下载后双击即可安装
- 配置环境变量
	- 配置 BOOST_INCLUDEDIR 和 BOOST_LIBRARYDIR和 PATH 中添加 D:\soft\boost\172\lib64-msvc-14.2
		![Pasted image 20250211220436.png](https://s2.loli.net/2025/03/28/g3oY2fpSIGmasxK.png)
		![Pasted image 20250211220529.png](https://s2.loli.net/2025/03/28/DOYa4HZvWe2FR3k.png)
## 配置 Include 目录
- **打开 Visual Studio**
- **进入** `项目 -> 属性 (Properties)`
- **导航到** `C/C++ -> 常规 (General)`
- **在 "附加包含目录 (Additional Include Directories)" 中添加：**
```
C:\local\boost_1_72_0
```


![Pasted image 20250211215321.png](https://s2.loli.net/2025/03/28/RgQtxYO6X1PW2ql.png)
![Pasted image 20250211215142.png](https://s2.loli.net/2025/03/28/6N2FoR9upaPBAmJ.png)

---


## 配置 Library 目录

- **在项目属性中，导航到** `链接器 (Linker) -> 常规 (General)`
- **在 "附加库目录 (Additional Library Directories)" 中添加：**
```
C:\local\boost_1_72_0\lib
```
![Pasted image 20250211215631.png](https://s2.loli.net/2025/03/28/EzLlVCrPnMdWY71.png)


---

## 链接 Boost 库

如果你使用的是 **头文件库（如 `Boost.SmartPtr`、`Boost.Algorithm`）**，那么 **不需要额外链接库**，直接包含头文件即可。

但是，如果你使用的是 **需要链接的 Boost 库**（如 `Boost.Filesystem`、`Boost.Thread`），你还需要在 **附加依赖项 (Additional Dependencies)** 中添加对应的 `.lib` 文件，例如：

#### **示例：使用 Boost.Filesystem**

- 在项目属性中，进入
```
链接器 -> 输入 -> 附加依赖项 (Linker -> Input -> Additional Dependencies)
```
- 添加对应的库文件（以 `Boost.Filesystem` 为例）：
```
boost_filesystem-vc142-mt-x64-1_72.lib boost_system-vc142-mt-x64-1_72.lib
```

![Pasted image 20250211220116.png](https://s2.loli.net/2025/03/28/2ZGVXTY7OQCzws4.png)


## 库引入
```Cpp
#include <boost/lexical_cast.hpp>
//不用库不一样
```




# 后记
> 和C++没有任何关系啦
> 本科遇见ww，虽然联系甚少了，但感谢ww最后一路的支持，不会忘记，所以遇见yf后，更加珍惜了
> 很开心遇见了yf，或许有一天总会分道扬镳，但是从我逐渐学习Cpp或者CSharp的时候，就认识了yf，是好朋友，有困难会两肋插刀
> 如果哪天你们看到了，希望你两个能知道，不仅仅是，做兄弟在心中，也会付出行动