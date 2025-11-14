---
title: Unity知识整理(一_入门)
math: true
---
# 新建工程和工程文件夹
- 如果有其他项目，可以用Add来添加
![Pasted image 20241113204345.png](https://s2.loli.net/2024/11/13/haryWDjMFAq1mTk.png)
![Pasted image 20241113204502.png](https://s2.loli.net/2024/11/13/jOcYQ9Nhq3pPzbr.png)
- 可以选择已经下载好的版本
![Pasted image 20241113204404.png](https://s2.loli.net/2024/11/13/B8A3xaHgCeTVl9M.png)

![Pasted image 20241113205241.png](https://s2.loli.net/2024/11/13/oyXSYNBis5QKR1w.png)

- 添加别人的工程文件
![Pasted image 20241113210240.png](https://s2.loli.net/2024/11/13/suMmVWQKt4xLPcz.png)
![Pasted image 20241113210713.png](https://s2.loli.net/2024/11/13/5B2yaoDiquxGjC1.png)

- 这里可以选择打开项目使用的Unity的版本
![Pasted image 20241113210518.png](https://s2.loli.net/2024/11/13/AETyPnju9tRXVba.png)

![Pasted image 20241113210539.png](https://s2.loli.net/2024/11/13/bVGDMpZedy59rY1.png)

- 创建后包含的文件夹
1. <font color="red">Assets：工程资源文件夹（美术资源，脚本等等）</font>
2. Library：库文件夹（Unity自动生成管理）
3. Logs：日志文件夹，记录特殊信息（Unity自动生成管理）
4. obj：编译产生中间文件（Unity自动生成管理）
5. Packages：包配置信息（Unity自动生成管理）
6. ProjectSettings：工程设置信息（Unity自动生成管理）

# Scene和Hierarchy窗口
## 窗口布局

![Pasted image 20241113210954.png](https://s2.loli.net/2024/11/13/PbBmYnULuO45cqV.png)

![Pasted image 20241113211126.png](https://s2.loli.net/2024/11/13/uaXsLOgBKd9e4fG.png)

## Hierarchy层级窗口
### 快捷键
```
F2：对象改名
Ctrl+C：复制
Ctrl+V：粘贴
Ctrl+D：克隆一个，等价于复制+粘贴
Ctrl+Z：撤销上一步
Delete：删除
```
- Hierarchy鼠标右键
```
Cube: 正方体
Sphere：球体
Capsule：胶囊体
Cylinder：圆柱体
Plane：平面
Quad：面片
```
![Pasted image 20241113211525.png](https://s2.loli.net/2024/11/13/l4HagJ92cykNb35.png)

## Scene场景窗口

![Pasted image 20241113212238.png](https://s2.loli.net/2024/11/13/pRm6ABzI1yJw32C.png)

- 着色器（一般选择这个就行）
![Pasted image 20241113212955.png](https://s2.loli.net/2024/11/13/4ZKrRpSLMgF5kzN.png)

- 网格
![Pasted image 20241113213021.png](https://s2.loli.net/2024/11/13/Y3fHTio1NvtKMpG.png)

- 着色器+网格
![Pasted image 20241113213036.png](https://s2.loli.net/2024/11/13/ak9Ob68Eo7Ai3hf.png)

- 3D显示
![Pasted image 20241113213536.png](https://s2.loli.net/2024/11/13/jLmFGNJUw5S2O7k.png)
- 2D显示
![Pasted image 20241113213601.png](https://s2.loli.net/2024/11/13/ylpQqjSKA7rmeTN.png)

![Pasted image 20241113214635.png](https://s2.loli.net/2024/11/13/YECS6i3fUoIXvnd.png)
- 对应的是X，Y，Z
- 可以点击有颜色的部分进行切换
- Persp的文字是表示透视，点击后Iso是正交模式
- 透视模式是近大远小，Iso是都一样大
![Pasted image 20241113214824.png](https://s2.loli.net/2024/11/13/qdRF6aDQZzgcjPX.png)
- 长按Q键+鼠标左键
	- 移动屏幕
- 长按Alt键+鼠标左键
	- 相对于视口中心旋转屏幕
- 选中物体+F键 或者 Hierarchy层窗口双击物体
	- 居中显示物体
- Shift+F键
	- 物体一直处于窗口中心，盯着物体移动
- 鼠标右键+移动鼠标
	- 旋转视口
- 鼠标右键+WASD
	- 漫游场景
- 鼠标右键+WASD+Shift
	- 快速漫游场景
- 长按Alt键+鼠标右键+移动鼠标
	- 相对屏幕中心拉近或者拉远
- 鼠标中键滚轮
	- 相对屏幕中心拉近或者拉远
- 鼠标中键按下+移动鼠标
	- 平移观察视口
- 长按Alt键+滚动鼠标中键
	- 鼠标指哪就朝哪拉近拉远
- Global是相对于整个世界的坐标轴
- Local是相对于物体本身的坐标轴
![Pasted image 20241113215900.png](https://s2.loli.net/2024/11/13/XQn8pgkfOetDzIc.png)
- 选中后是一格一格移动物体
- 按住Ctrl，更小格移动物体，可以不用开启下面的这个按钮的
![Pasted image 20241113220426.png](https://s2.loli.net/2024/11/13/8dgyeFUOSasMQ1u.png)

![Pasted image 20241113221904.png](https://s2.loli.net/2024/11/13/mNLbG5OQnjVqozg.png)


# Game和Project
> Game游戏窗口：游戏画面窗口，玩家能看到的画面内容
> Project工程窗口：工程资源窗口，所有的工程资源都会在该窗口中显示，显示的内容为Assets文件夹中的所有内容
## Game游戏窗口
- 是摄像机拍摄的内容，如果没有摄像机，就不显示

![Pasted image 20241113223403.png](https://s2.loli.net/2024/11/13/SNJ4HPIX37BU5g2.png)
- Free Aspect
	- 自由选择分辨率
![Pasted image 20241113223318.png](https://s2.loli.net/2024/11/13/iwAI2acT1HhMCFU.png)
![Pasted image 20241113223900.png](https://s2.loli.net/2024/11/13/lWXmb8nHxJVAUFI.png)


- Warn if No Cameras Rendering
	- 场景中没有摄像机时发出警告
- Clear Every Frame in Edit Mode
	- 游戏未播放时，也更新Game窗口，避免显示问题
- Maximize
	- 窗口最大化
- Close Tab
	- 关闭当前窗口
- Add Tab
	- 添加新窗口
![Pasted image 20241113223512.png](https://s2.loli.net/2024/11/13/eFqaEvsAlO2KpGM.png)

## Project工程窗口
![Pasted image 20241113224222.png](https://s2.loli.net/2024/11/13/z91a83rSluhA2Wk.png)

![Pasted image 20241113224413.png](https://s2.loli.net/2024/11/13/SOqZEaN8e29Lp6W.png)

![Pasted image 20241113224533.png](https://s2.loli.net/2024/11/13/HZLD65sISaez2mp.png)

# Inspector和Console窗口
> Inspector检查窗口：
> 查看场景中游戏对象关联的C#脚本信息
> Console控制台窗口:
> 用于查看调试信息的窗口
> 报错、警告、测试打印都可以显示在其中

## Inspector检查窗口
- 这个可以更换颜色，就是对象外面发光的颜色
![Pasted image 20241113232237.png](https://s2.loli.net/2024/11/13/NslHxZgOaJdD6uQ.png)
- 对象是否激活，不激活就不显示对象了，和隐藏不一样
- 对象名字的修改，或者在对象上（Hierarchy上）进行修改
![Pasted image 20241113232402.png](https://s2.loli.net/2024/11/13/JjngtGY8RLekX3E.png)

- 对象的标签（小标签）
![Pasted image 20241113232535.png](https://s2.loli.net/2024/11/13/SnfqK8RWtDx9IeO.png)
- 对象的层级（大标签，同一个层级下可以有很多一样的标签）
![Pasted image 20241113232718.png](https://s2.loli.net/2024/11/13/OxcpiSRarHjCwfv.png)

- Transform
	- 位置信息
	- Position
		- 位置信息
	- Rotation
		- 角度信息
	- Scale
		- 缩放比例
![Pasted image 20241113232834.png](https://s2.loli.net/2024/11/13/R5JI8ugKEd1m9iz.png)

## Console窗口
- 打开Console窗口
- 快捷键
	- Ctrl+Shift+C
![Pasted image 20241113233026.png](https://s2.loli.net/2024/11/13/LvIxH2tVW7KiaGS.png)
- Clear
	- 清空控制台
- Clear on Play
	- 运行时清空
- Clear on Build
	- 构建时清空，也就是代码编译的时候清空控制台
- Collapse
	- 相同内容折叠显示
- Error Pause
	- 报错暂停运行
- 气泡+!
	- 是否显示错误信息
- 三角+！
	- 是否显示警告信息
- 圆+！
	- 是否显示打印信息
![Pasted image 20241113233205.png](https://s2.loli.net/2024/11/13/qxDjG7kgORwEzYs.png)


# 工具栏和父子关系
![image.png](https://s2.loli.net/2024/11/14/BcG5ZKgpHu1zV4m.png)

- File
	- 新建工程，新建场景，工程打包等等
- Edit
	- 对象编辑操作相关，工程设置，引擎设置相关
- Assets
	- 基本等同于Project窗口中右键相关功能
- GameObject
	- 基本等同于Hierarchy窗口中右键相关功能
- Commponent
	- Unity自带的脚本，可以添加各系统中的脚本
- Window
	- 可以打开Unity各系统核心系统的窗口
- Help
	- 检查更i性能，查看版本等等功能

![image.png](https://s2.loli.net/2024/11/14/Kd154oAgMTPOXqE.png)
- 换IDE的时候，找到可执行文件就行
![image.png](https://s2.loli.net/2024/11/14/VeUu4PaGt673JrK.png)

- 编辑器使用中文
- 推荐英文了，改成中文后，需要重新启动项目
![image.png](https://s2.loli.net/2024/11/14/LJmBgYid8FuU1e2.png)

![image.png](https://s2.loli.net/2024/11/14/Vn8MfJH12XwebDu.png)
- Edit (编辑)->Preferences(首选项)->languages(语言)->Editor language->简体中文
![image.png](https://s2.loli.net/2024/11/14/PkXxAINQvVgK3i8.png)

- 快捷键设置
![image.png](https://s2.loli.net/2024/11/14/pGX1C9wnyKedLrN.png)

- 设置格子的默认长度和步长
![image.png](https://s2.loli.net/2024/11/14/IWEPQFDaldckpRj.png)

![image.png](https://s2.loli.net/2024/11/14/H7fjTteOCyikZaS.png)
- Ctrl+Alt+F
	- 物体移动到看见的窗口的中心点
- Ctrl+Shift+F
	- 能够将物体移动到当前视角
	- 一般用来设置摄像机
![image.png](https://s2.loli.net/2024/11/14/hJ6tTHzdObELSXZ.png)

- 有资源商店
![image.png](https://s2.loli.net/2024/11/14/dyL1Pc7Qrnu9vsV.png)

- 官方文档
![image.png](https://s2.loli.net/2024/11/14/weXQ8v9PURtcCGj.png)
- **Unity Manual（用户手册）**：
    - **内容**：主要介绍 Unity 的基本概念、工作流程和各种工具的使用方法。
    - **用途**：适用于入门和理解 Unity 的核心概念，比如项目设置、界面介绍、物理系统、渲染管道、资源管理、2D/3D 工具、UI 系统等。
    - **示例**：用户可以找到有关如何搭建场景、如何使用 Unity 的图形和音效工具、如何优化游戏的说明。
    - **适合对象**：对 Unity 整体环境和功能模块有深入了解需求的开发者。
- **Unity Scripting API（脚本 API 文档）**：
    - **内容**：详细介绍 Unity 的 API，包括所有脚本类、函数、属性等，主要围绕 C# 脚本编写和调用。
    - **用途**：用于查找编程接口和具体的代码使用示例，特别是在编写脚本、调用 Unity 的内置功能时。
    - **示例**：包含每个 API 的详细定义、参数说明、代码示例等，帮助开发者正确使用方法、类或属性。
    - **适合对象**：需要通过代码操控 Unity 功能的开发者，尤其是编写和调试脚本时。



- 构成父子对象
	- 其中Sphere是父对象
	- Cube是子对象
- 子对象会随着父对象的变化而变化
	- 父对象移动，缩放，旋转，子对象相对应也会一起修改
- 子对象Inspector窗口中Transform信息是相对父对象的
	- 子对象的Transform中的数据是相对坐标，是相对于父对象的
- Scene上方 Pivot 和 Global的作用
	- Pivot是相对于自己对象
	- Center是父对象+子对象的综合的中心结果
![image.png](https://s2.loli.net/2024/11/14/rWCGA4BS8XYvcUl.png)

![image.png](https://s2.loli.net/2024/11/14/Cw4renEpWHkvLtm.png)

![image.png](https://s2.loli.net/2024/11/14/43ktELRFpGlSeqK.png)

# 反射机制和游戏场景
- 创建一个脚本
![image.png](https://s2.loli.net/2024/11/15/nW8KfN632etkzd1.png)
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    public int age;
    public bool sex;
    public string name;
    // 下面是初始化就有的代码
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}

```
- 如何将脚本挂在对象上
![202411151140792.png](https://s2.loli.net/2024/11/15/DOtsYPfUmwZvNA3.png)
- 这里可以看见public变量，并实时修改
![image.png](https://s2.loli.net/2024/11/15/dxCD6K9GbvBpI1Q.png)
- Scene的本质
- 就是一个配置文件
![image.png](https://s2.loli.net/2024/11/15/JLxzvXi5HkFMqTa.png)

![image.png](https://s2.loli.net/2024/11/15/MBuR72CE9frpILy.png)
![image.png](https://s2.loli.net/2024/11/15/tzLOf3M5egDuX24.png)

# 预设体和资源包的导入导出
## 预设体
> 预设体本质是记录了对象以及对象上的脚本信息
- 创建预设体
![image.png](https://s2.loli.net/2024/11/15/dYlcOw9P6H5pFn4.png)
- 使用预设体
- 将选中的预设体拖回去就可以了
![image.png](https://s2.loli.net/2024/11/15/nJxYqMP3iDdzK5Q.png)
- 修改（覆盖原有的预设体）

- 添加
![image.png](https://s2.loli.net/2024/11/15/RBi7XfZmrkKG2AO.png)

![image.png](https://s2.loli.net/2024/11/15/z6lEFOmJDaijKvX.png)

- 删除（也包含添加）
- 直接删除会弹出这个，点击open Prefab
![open Prefab](https://s2.loli.net/2024/11/15/UFCN7pSBoOAPRQn.png)


- 会进入这个Prefab设置的界面，这里可以修改，修改后所有对应的预设体都会被修改
- 左上角的`<`是用来返回Scene设置的
![image.png](https://s2.loli.net/2024/11/15/QMFPXwkEy7LBIvf.png)
- 点击预设体的Open也能够进入上面的界面
![image.png](https://s2.loli.net/2024/11/15/4r236NRs8aLknBW.png)
- 如果想要将对象和预设体的关系切断
- 点击这个Unpack
- Unpack Completely主要用在预设体里面还有预设体的情况


![image.png](https://s2.loli.net/2024/11/15/73oRFTVNjbM8Imz.png)

![image.png](https://s2.loli.net/2024/11/15/2bDCSY9hqcJRgIv.png)

## 资源包的导入和导出
- import 是导入资源
- export 是导出资源
![Pasted image 20241115123207.png](https://s2.loli.net/2024/11/18/FeMDxZXp6P8bN5H.png)

- 导出资源
- include dependencies的意思是会默认把脚本也关联上
![Pasted image 20241115123324.png](https://s2.loli.net/2024/11/18/BFcYMf7d168oLVs.png)
- 这个就是导出后的Unity包
![Pasted image 20241115123453.png](https://s2.loli.net/2024/11/18/wuAWbFVjCNhxeXa.png)
- 既然已经导出了资源包，我们把原来的删除
- 试试导入
- 一种方式是将资源包拖入Project中
![Pasted image 20241115123631.png](https://s2.loli.net/2024/11/18/7MVmTavb8jKoX6s.png)
- 另一种就是右键Import 
![Pasted image 20241115123735.png](https://s2.loli.net/2024/11/18/wPV2EbhDk4o7aOU.png)

# 脚本基本规则
> 1. 不在VStudio中创建脚本，统一在Unity中创建脚本
> 2. 脚本可以放在Assets文件夹中任何位置，建议在Assets文件夹中创建一个Scripts文件夹，统一管理
> 3. 类名和文件名必须一直，不然不能挂载（因为反射机制创建对象，会通过文件名去找Type）
> 4. 不要用中文命名（我也不会用）
> 5. 没有特殊需求，不用管命名空间
> 6. 创建的脚本默认继承MonoBehaviour

## MonoBehaviour基类

- 继承MonoBehaviour类，一个文件中只能有一个类继承MonoBehaviour类
> 1. 创建的脚本默认都继承MonoBehaviour，继承了它才能够挂载在GameObject上
> 2. 继承了MonoBehaviour的脚本不能new，只能挂！！！！！！！！可以通过AddComponent()来创建脚本
> 3. 继承了MonnBehaviour的脚本不要去写构造函数，因为我们不会去new它，写构造函数没有任何意义
> 4. 继承了MonoBehaviour的脚本可以在一个对象上挂多个（如果没有加DisallowMultipleComponent特性)
> 5. 继承MonoBehaviour的类也可以再次被继承，遵循面向对象继承多态的规则
- 不继承MonoBehaviour类
>1. 不继承MonoBehaviour的类不能挂载在GameObject上
>2. 不继承MonoBehaviour的类想怎么写怎么写，如果要使用需要自己new
>3. 不继承MonoBehaviour的类一般是单例模式的类(用于管理模块)或者数据结构类 (用于存储数据）
>4. 不继承MonoBehaviour的类不用保留默认出现的几个函数

## 挂载脚本的执行顺序

![Pasted image 20241116120518.png](https://s2.loli.net/2024/11/18/WCkzVZvOq41PQeN.png)

![Pasted image 20241116120547.png](https://s2.loli.net/2024/11/18/s9QTp3Wj6cOSyXu.png)

- 选择自己的脚本
![Pasted image 20241116120600.png](https://s2.loli.net/2024/11/18/7SC6qg3ODmfa2Ml.png)
- 按照这个优先度来执行脚本
- 比如`UnityEngine.UI.ToggleGroup`就比`NewBehaviourScript`先执行
- 如果没有设置这个，那脚本执行顺序是不确定的
![Pasted image 20241116120644.png](https://s2.loli.net/2024/11/18/sGS5hlQrXPpkbYi.png)

## 修改脚本默认内容
> 一般不改

![Pasted image 20241116120902.png](https://s2.loli.net/2024/11/18/R7XMn9vcaBDoU3z.png)

- 进入这个路径
- `看具体路径\2020.3.48f1c1\Editor\Data\Resources\ScriptTemplates`
- 修改81那个文件就可以了
![Pasted image 20241116121408.png](https://s2.loli.net/2024/11/18/M8V3Ox46lf2othu.png)

# 生命周期函数
> 所有继承MonoBehaviour的脚本，最终会挂载到GameObject游戏对象上
> 生命周期函数，就是该脚本依附的GameObject对象从出生到消亡整个生命周期中
> 会通过反射自动调用的一些特殊函数
> 生命周期函数访问修饰符一般是private和protected。不需要外部自己调用生命周期函数，是Unity自己调用
> 如果脚本依附的对象失活，这些生命周期函数都不会被调用，失活对象第一次从失活->激活，会调用Awake...等生命周期函数
> 不需要使用到的生命周期函数不要写，因为Unity会调用，有额外的开销

![Pasted image 20241117105345.png](https://s2.loli.net/2024/11/18/vZBf3zcrYDAS5U8.png)

## 1. **对象的初始化**

这些方法在脚本挂载的对象被创建时调用：

- `Awake()`
    - **触发时机**：脚本对应的类实例化后，所有其他对象的初始化之前。
    - **用途**：初始化变量或引用。即使对象未启用，也会调用。
    - 只会调用一次，反复激活脚本依附的对象不会二次调用
```csharp
void Awake()
{
    Debug.Log("Awake called");
}
```
- `OnEnable()`
    - **触发时机**：对象或脚本被启用时。
    - **用途**：每次对象激活时需要执行的逻辑。
    - 当脚本依附的对象被激活时，都会调用一次，如果脚本在Unity中被失活了，不会调用
    - 只有脚本时激活的，并且，对象被激活了（包含刚开始实例化对象的那一次）都会调用
```csharp
void OnEnable()
{
    Debug.Log("OnEnable called");
}
```
- `Start()`
    - **触发时机**：对象启用后，所有 `Awake()` 调用完成后第一次更新前。
    - **用途**：需要在对象启用时执行的初始化逻辑。
```csharp
void Start()
{
    Debug.Log("Start called");
}
```
        

---

## 2. **游戏循环更新**
- 下面的三个方法会一直不断调用，循环调用

这些方法用于处理游戏对象的行为更新：
- `Update()`
    - **触发时机**：游戏每帧调用一次。
    - **用途**：实现逐帧更新的逻辑。
```csharp
void Update()
{
    Debug.Log("Update called");
}
```
- `LateUpdate()`
    - **触发时机**：每帧调用，但在所有 `Update()` 调用后。
    - **用途**：用于更新需要依赖其他对象已完成更新的逻辑（如跟随摄像机）。
```csharp
void LateUpdate()
{
    Debug.Log("LateUpdate called");
}
```
        
- `FixedUpdate()`
    - **触发时机**：每固定时间间隔调用，与物理系统同步。
    - **用途**：处理物理相关逻辑（如力和速度的应用）。
    - Edit->Project Setting->Time->Fixed Timestep
	    - 修改这个，表示每多少秒调用一次`FixedUpdate()`
![Pasted image 20241117113557.png](https://s2.loli.net/2024/11/18/IP4bQ5ZEu9CUKLr.png)

```csharp
void FixedUpdate()
{
    Debug.Log("FixedUpdate called");
}
```
        

---

## 3. **暂停或停止时**

处理对象被禁用或销毁的阶段：
- `OnDisable()`
    - **触发时机**：对象或脚本被禁用时，对象被销毁前如果没有失活，会先失活，再销毁。失活了，再删除对象，只会调用OnDestroy()。
    - **用途**：释放资源或暂停逻辑。
```csharp
void OnDisable()
{
    Debug.Log("OnDisable called");
}
```
- `OnDestroy()`
    - **触发时机**：对象销毁时。
	    - 程序中删除对象或程序结束，都会调用OnDestroy()，不论对象是否失活
    - **用途**：释放资源或清理逻辑。
```csharp
void OnDestroy()
{
    Debug.Log("OnDestroy called");
}
```
---

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    // 当对象（自己这个类对象）被创建时，才会调用该生命周期函数
    // 类似构造函数的存在，可以在一个类对象创建时进行一些初始化操作
    private void Awake()
    {
        // 在Unity中打印信息的两种方式
        //1. 没有继承MonoBehaviour类的时候
        //Debug.Log("123");
        // Debug.LogError("报错");
        // Debug.LogWarning("警告");

        // 继承了MonoBehaviour，有一个线程的方法，可以使用
        print("Awake");
    }

    // 当脚本依附的GameObject对象被激活时调用
    // 当脚本被激活时调用
    private void OnEnable()
    {
        print("OnEnable");
    }

    // Start is called before the first frame update
    // 默认访问修饰符是private
    // 主要作用是初始化信息，相对于Awake要晚一些
    // 在对象进行第一次帧更新之前才会执行
    void Start()
    {
        print("Start");
    }

    // 主要用于物理更新
    // 它是每一帧执行的，但是和游戏帧不一样
    // 时间间隔可以在Project setting中的Time中设置
    private void FixedUpdate()
    {
        print("FixedUpdate");
    }

    // Update is called once per frame
    // 主要用于处理游戏核心逻辑更新的函数
    // 默认是以最快的游戏帧率运行
    void Update()
    {
        print("Update");
    }

    // 一般这个更新用来处理摄像机的
    // 因为Update()和LateUpdate()之间Unity进行了动画处理，进行动画的更新
    // 如果在Update中处理摄像机，会导致某些内容渲染出错
    private void LateUpdate()
    {
        print("LateUpdate");
    }

    // 当对象失活时调用这个函数
    // 当脚本失活时调用这个函数
    private void OnDisable()
    {
        print("OnDisable");
    }

    // 当对象被销毁时调用
    // 销毁前会调用OnDisable()
    // 然后销毁，在程序中删除对象或程序结束，都会调用OnDestroy()
    private void OnDestroy()
    {
        print("OnDestroy");
    }
}
```

## 4. **渲染和碰撞**
与渲染或物理相关的生命周期函数：
- `OnGUI()`
    - **触发时机**：每帧用于绘制 UI 元素。
    - **用途**：处理 GUI 的绘制逻辑。
```csharp
void OnGUI()
{
    GUILayout.Label("Hello, World!");
}

```
- `OnCollisionEnter(Collision collision)`**
    - **触发时机**：对象与其他碰撞器发生碰撞时。
    - **用途**：处理碰撞事件。
```csharp
void OnCollisionEnter(Collision collision)
{
    Debug.Log("Collision detected");
}
```
- `OnTriggerEnter(Collider other)`
    - **触发时机**：对象进入触发器时。
    - **用途**：处理触发器事件。
```csharp
void OnTriggerEnter(Collider other)
{
    Debug.Log("Trigger entered");
}
```
        

---

## 5. **其他特殊事件**

一些其他常用的生命周期函数：
- `Reset()`
    - **触发时机**：在组件被添加到对象时调用。
    - **用途**：初始化默认值。
```csharp
void Reset()
{
    Debug.Log("Reset called");
}
```
- `OnApplicationQuit()`
    - **触发时机**：游戏退出时。
    - **用途**：释放全局资源或保存数据。
```csharp
void OnApplicationQuit()
{
    Debug.Log("Application quitting");
}
```
- `OnDrawGizmos()`
    - **触发时机**：在编辑器模式下绘制场景中调试信息。
    - **用途**：绘制调试辅助图形。
```csharp
void OnDrawGizmos()
{
    Gizmos.DrawSphere(transform.position, 1f);
}
```
---
## 总结表格

| **函数名**               | **触发时机**      | **用途**      |
| --------------------- | ------------- | ----------- |
| `Awake()`             | 对象实例化后        | 初始化脚本或依赖引用  |
| `OnEnable()`          | 对象启用时         | 每次对象激活执行逻辑  |
| `Start()`             | 第一次更新前        | 延迟初始化逻辑     |
| `Update()`            | 每帧调用一次        | 逐帧更新逻辑      |
| `LateUpdate()`        | 每帧更新后         | 依赖其他对象更新的逻辑 |
| `FixedUpdate()`       | 固定时间间隔（与物理同步） | 处理物理逻辑      |
| `OnDisable()`         | 对象被禁用时        | 暂停逻辑或释放资源   |
| `OnDestroy()`         | 对象销毁时         | 清理资源        |
| `OnGUI()`             | 每帧绘制 UI 元素    | GUI 绘制      |
| `OnCollisionEnter()`  | 碰撞发生时         | 处理碰撞事件      |
| `OnTriggerEnter()`    | 进入触发器时        | 处理触发事件      |
| `Reset()`             | 组件被添加到对象时     | 初始化默认值      |
| `OnApplicationQuit()` | 游戏退出时         | 保存数据或释放全局资源 |
| `OnDrawGizmos()`      | 编辑器模式下绘制调试信息  | 调试辅助图形绘制    |

# Inspector窗口可编辑的变量
> 也就是如何让自定义的类可以在Inspector窗口上进行显示

- Iinspector显示的可编辑内容就是脚本的成员变量
- 私有和保护无法显示编辑，公共的可以显示编辑

## 如何让私有和保护的也可以被显示编辑
```csharp
// 加上强制序列化字段特性
//[SerializeField]
// 所谓序列化指将对象保存到文件或者数据库字段中
[SerializeField]
private int privateInt;
[SerializeField]
protected string protectedStr;
```

## 如何让公共的不显示编辑
```csharp
// 在变量前加上特性[HideInInspector]
// 可以让公共变量不显示编辑
[HideInInspector]
public int publicInt = 50;
```


## 哪些类型不可以被显示编辑
1. Dictionary<T,K> dic; // 字典不行
2. 自定义类型

## 让自定义类型也可以被显示编辑
```csharp
// 加上序列化特性
// [System.Serializable]
// 字典怎样都无法显示编辑

[System.Serializable]
public struct MyStruct
{
	public int age;
	public bool sex;
}

public MyStruct myStruct;
```
![Pasted image 20241117122436.png](https://s2.loli.net/2024/11/18/ZszNTrc1MVtO3mx.png)

## 其他的辅助特性
- 分组说明特性Header
- 他这个其实只是加一行，显示说明，并不是严格意义上的分组
```csharp
// 分组说明特性Header
// 作用：为成员分组
// [Header("分组说明")]
[Header("基础属性")]
public int age;
public bool sex;
[Header("战斗属性")]
public int atk;
public int def;
```

![Pasted image 20241117122736.png](https://s2.loli.net/2024/11/18/4sqDb2rKchC67YR.png)

- 悬停注释Tooltip
```csharp
// 悬停注释Tooltip
// 为变量添加说明
// [Tooltip("说明内容")]
[Tooltip("闪避")]
public int miss;
```
![Pasted image 20241117123110.png](https://s2.loli.net/2024/11/18/X8zRtWCgNTMdZJE.png)


- 间隔特性Space()
- 默认间距是 6 像素
- `[Space(n)]`：在字段上方增加 **n 像素**的垂直间隔。
- `[Space]`：如果未指定数值，默认间隔为 **6 像素**。
```csharp
// 间隔特性Space()
// 让两个字段之间出现间隔
// [Space()]
[Space]
public int crit;
```
![Pasted image 20241117123640.png](https://s2.loli.net/2024/11/18/hMylmiRz6OYQvVK.png)

- 修饰数值的滑条范围Range
```csharp
// 修饰数值的滑条范围Range
// [Rang(最小值，最大值)]
[Range(0,10)]
public float luck;
```

![Pasted image 20241117123818.png](https://s2.loli.net/2024/11/18/w76Ms8aloEuiTtB.png)

- 多行显示字符串，默认显示3行`[Multiline]`
```csharp
//多行显示字符串，默认不写参数显示3行
//写参数就是对应行
// [Multiline]
[Multiline(5)]
public string tips;
```
- 如果不加`[Multiline]`，只能是一行，如果加了，可以多行，但是最初的时候会显示n行
![Pasted image 20241117124128.png](https://s2.loli.net/2024/11/18/tVxSpCOUse7Nzi5.png)

- 滚动条显示字符串
```csharp
//滚动条显示字符串
//默认不写参数就是大于等于3行显示滚动条
//[TextArea(minLines, maxLines)]
//minLines：文本区域的最小行数。
//maxLines：文本区域的最大行数。
//最少显示3行，最多4行，大于等于4行就显示滚动条
[TextArea(3, 4)]
public string myLife; // 需要为string，否则报错type is not a supported string value
```
![Pasted image 20241117233101.png](https://s2.loli.net/2024/11/18/AvoCHyYui2d7UfJ.png)


- 为变量添加快捷方法 ContextMenuItem
```csharp
//为变量添加快捷方法 ContextMenuItem
//参数1显示按钮名1
//参数2方法名不能有参数，返回值随便，因为也不用
//[ContextMenuItem("显示按钮名","方法名")]
[ContextMenuItem("重置钱","Test")]
public int money;
private void Test() {
	money = 99;
}
```

![Pasted image 20241117233743.png](https://s2.loli.net/2024/11/18/4DFVXOTZqMC9ymN.png)

- 为方法添加特性能够在Inspector中执行
- 可以在编辑或者运行的时候点击这个哈哈哈哈的按钮，就会执行对应的方法，教程里面说运行的时候没有意义，等以后再说吧
```csharp
//为方法添加特性能够在Inspector中执行
//[ContextMenu("测试函数")]
[ContextMenu("哈哈哈哈")]
private void TestFun()
{
	print("测试方法");
	money = 11;
}
```
![Pasted image 20241117234227.png](https://s2.loli.net/2024/11/18/4BDm3OTXjAeu95C.png)

## 注意
1. Inspector窗口中的变量关联的就是对象的成员变量，运行时改变他们就是在改变成员变量
2. 拖曳到Gameobject对象后再改变脚本中变量默认值界面上不会改变
3. 运行中修改的信息不会保存
4. 如果需要记录运行中脚本的值，可以先Copy Component，再停止运行，使用Paste Component Values
5. Paste Component As New的意思是在本对象上再复制一个脚本，值和原脚本一样，但是如果是运行中用这个，结束运行后会丢失这个复制的脚本
![Pasted image 20241117234607.png](https://s2.loli.net/2024/11/18/mbY5K8Ehq6RcTUW.png)

# MonoBehaviour中的重要内容
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    private void Start()
    {
        #region 重要成员
        // 1. 获取依附的GameObject对象
        print(this.gameObject);
        // 2. 获得依附的GameObject的位置信息，三种方法，是等价的，一般直接用transform
        print(this.transform.position); // 位置
        print(this.gameObject.transform.eulerAngles); // 欧拉角
        print(transform.lossyScale); // 缩放比例
        // 3. 获取脚本是否激活
        // 其实如果脚本本就是不激活的，那么就不会调用这个Start()生命周期函数了
        print(this.enabled); // 一定是True
        this.enabled = false;// 不过可以通过反射得到脚本，控制其激活和失活
        #endregion

        #region 重要方法
        // 1. 得到同一个对象下的其他脚本
        // 根据脚本名获取类对象，如果获取失败，表示没有对应的脚本，返回NULL
        Lesson3_Test t = this.GetComponent("Lesson3_Test") as Lesson3_Test;
        print(t);
        // 根据Type获取
        t = this.GetComponent(typeof(Lesson3_Test)) as Lesson3_Test;
        print(t);
        // 用的最多
        // 根据泛型获取
        t = this.GetComponent<Lesson3_Test>();
        print(t);
        // 所以只要能够获取场景中别的对象或者对象依附的脚本
        // 就能够获取到它的所有信息，依附的对象就是gameObject，脚本用GetComponent()

        // 2. 得到自己挂载的多个脚本
        // 这里得到的只能是Lesson3_Test类的脚本，其他的得不到
        Lesson3_Test[] array = this.GetComponents<Lesson3_Test>();
        print(array.Length);
        // List<T> 是引用类型，在方法内部修改它的内容会直接反映到调用方
        List<Lesson3_Test> list = new List<Lesson3_Test>();
        this.GetComponents<Lesson3_Test>(list);
        print(list.Count);
        MonoBehaviour[] allScripts = this.GetComponents<MonoBehaviour>();
        foreach (var script in allScripts)
        {
            Debug.Log($"Script Name: {script.GetType().Name}");
        }

        // 3. 得到子对象挂载的脚本（默认也会找自己身上是否挂载此脚本）
        // 先找自己对象上是否挂载了这个脚本，如果没有，再找子对象，直到找到了一个或者全部子对象都找完了
        // 参数：默认是false，表示不寻找失活的子对象上的脚本
        // true，表示会寻找失活的子对象上的脚本
        t = this.GetComponentInChildren<Lesson3_Test>();
        print(t);

        // 得到子对象挂载的所有指定脚本（默认也会找自己身上是否挂载此脚本）
        Lesson3_Test[] lts = this.GetComponentsInChildren<Lesson3_Test>(true);
        print(lts.Length);
        List<Lesson3_Test> list2 = new List<Lesson3_Test>();
        this.GetComponentsInChildren<Lesson3_Test>(true, list2);
        print(list2.Count);

        // 4. 得到父对象挂载的脚本（默认也会找自己身上是否挂载此脚本）
        // 这里是如果父对象失活了，子对象全部都会失活
        // 所以没有参数，先找离自己最近的父对象，如果没找到，再找更远的父对象，找到一个就行了
        t = this.GetComponentInParent<Lesson3_Test>();
        print(t);
        lts = this.GetComponentsInParent<Lesson3_Test>(true);
        print(lts.Length);
        this.GetComponentsInParent<Lesson3_Test>(true, list2);
        print(list2.Count);

        // 5. 尝试获取脚本
        Lesson3_Test l3t;
        // 提供一个更加安全的，获取单个脚本的方法，如果有，返回true，否则为false
        if (this.TryGetComponent<Lesson3_Test>(out l3t))
        {
            // 逻辑处理
        }
        else 
        {
            // 就是没有这个脚本了
        }
        #endregion
    }
}

```

# GameObject的成员变量

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson4 : MonoBehaviour
{
    //准备用来克隆的对象
    //1.直接是场景上的某个对象
    //2.可以是一个预设体对象
    public GameObject myObj;

    public GameObject myObj2;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 GameObject中的成员变量
        //名字
        print(this.gameObject.name);
        this.gameObject.name = "Lesson4wing2791改名";
        print(this.gameObject.name);
        //是否激活
        print(this.gameObject.activeSelf);
        //是否是静态
        print(this.gameObject.isStatic);
        //层级，对象身上的Layer索引
        print(this.gameObject.layer);
        //层级，对象身上的Layer名字
        print(LayerMask.LayerToName(this.gameObject.layer));
        //标签，对象上的tag
        print(this.gameObject.tag);
        //this.transform，通过Mono去得到的依附对象的GameObject的位置信息
        //两者信息是一样，都是依附的GameObject的位置信息
        // 输出的信息会保留一位小数
        print(this.gameObject.transform.position);
        // 表示保留 7 位小数
        print(this.gameObject.transform.position.ToString("F7"));
        #endregion

        #region 知识点二 GameObject中的静态方法
        //创建自带几何体
        //只要得到了GameObject对象,就可以得到它身上挂载的任何脚本信息
        //通过obj.GetComponent<>()来获取脚本信息
        GameObject obj = GameObject.CreatePrimitive(PrimitiveType.Cube);
        obj.name = "wing2791创建的立方体";

        //查找对象相关的知识

        #region 查找单个对象
        //两种找单个对象的共同点:
        //1.无法找到失活的对象的
        //  只能找到激活的对象

        //2.如果场景中存在多个满足条件的对象
        //  我们无法准确确定找到的是谁

        //查找单个对象
        //通过对象名查找
        //查找效率低下,因为会在场景中的所有对象去查找
        //没有找到返回null
        GameObject obj2 = GameObject.Find("wing2791");
        if (obj2 != null)
        {
            print(obj2.name);
        }
        else
        {
            print("没有找到对应对象");
        }
        //通过tag来查找对象
        //GameObject obj3 = GameObject.FindWithTag("Player");
        //该方法和上面这个方法 效果一样 只是名字不一样而已
        // 如果Tag不存在，UnityException: Tag: UI is not defined.
        // 在 Unity 中，GameObject.FindGameObjectWithTag("Untagged") 不会返回任何 GameObject，因为 Unity 的标签系统中不存在 "Untagged" 作为一个可用的查找标签。
        GameObject obj3 = GameObject.FindGameObjectWithTag("Respawn");
        if (obj3 != null)
        {
            print("根据tag找的对象" + obj3.name);
        }
        else
        {
            print("根据tag没有找到对应对象");
        }

        //得到某一个单个对象 目前有2种方式了
        //1.是public从外部面板拖 进行关联
        //2.通过API去找

        //找到场景中挂载某一个脚本的对象
        //效率更低，上面的GameObject.Find 和FindWithTag只是遍历对象
        //此方法不仅要遍历对象，还要遍历对象上挂载的脚本
        Lesson4 o = GameObject.FindObjectOfType<Lesson4>();
        print(o.gameObject.name);
        #endregion

        #region 查找多个对象
        //查找多个对象
        //只能是通过tag去找多个对象，没有方法是通过名字查找多个对象

        //通过tag找到多个对象
        //只能找到激活对象，无法找到失活对象
        GameObject[] objs = GameObject.FindGameObjectsWithTag("Player");
        print("找到tag为Player对象的个数" + objs.Length);

        //还有几个查找对象相关是用的比较少的方法，是GameObject父类Object提供的方法
        //Unity中的Object和C#中的万物之父的区别
        //Unity里面的Object不是指的万物之父object
        //Unity里的Object命名空间在UnityEngine中的Object类是集成万物之父的一个自定义类
        //C#中的Object命名空间是在System中的
        #endregion

        #region 实例化对象
        //实例化对象（克隆对象）
        //根据一个GameObject对象，创建出一个和它一模一样的对象
        GameObject obj5 = GameObject.Instantiate(myObj);
        //如果继承了MonoBehavior，可以不用写GameObject一样能使用
        //因为该方法是Unity里面的Object基类提供的，MonoBehaviour也是继承了Unity中的Object基类
        //本来就有，可以直接用
        //Instantiate(myObj);
        #endregion

        #region 删除对象
        //删除对象
        GameObject.Destroy(myObj2);
        //第一个参数：表示删除的对象
        //第二个参数：表示延迟时间删除（秒）
        GameObject.Destroy(obj5, 5);
        //Destroy不仅可以删除对象 还可以删除脚本，不过停止运行后，脚本还会恢复
        //GameObject.Destroy(this);

        //删除对象有两种作用
        //1.删除指定的一个游戏对象
        //2.删除一个指定的脚本对象
        //注意：Destroy方法不会马上移除对象，只是给这个对象加了一个移除标识
        //     一般情况下 它会在下一帧时把这个对象移除并从内存中移除

        //如果没有特殊需求，就是一定要马上移除一个对象的话
        //建议使用上面的 Destroy方法，因为是异步的，降低卡顿的几率
        //下面这个方法 是立即把对象从内存中移除了
        //GameObject.DestroyImmediate(myObj);

        //如果是继承MonoBehavior的类 不用写GameObject
        //Destroy(myObj2);
        //DestroyImmediate(myObj);

        //过场景不移除
        //默认情况在切换场景时（Application.LoadLevel("SceneName");、SceneManager.LoadScene("SceneName");）
        //场景中对象都会被自动删除掉
        //如果你希望某个对象过场景不被移除（卸载）
        //不想谁过场景被移除就传谁
        //一般都是传脚本依附的GameObject对象
        //下自己依附的GameObject对象 过场景不被删除
        GameObject.DontDestroyOnLoad(this.gameObject);
        //如果继承MOnoBehavior也可以直接写
        //DontDestroyOnLoad(this.gameObject);
        #endregion

        #endregion

        #region 知识点三 GameObject中的成员方法
        //创建空物体
        //new一个GameObject就是在创建一个空物体
        GameObject obj6 = new GameObject();
        GameObject obj7 = new GameObject("wing2791创建的空物体");
        // 加了脚本以后,会产生一个对象，会运行对象上的脚本
        // 但是呢，如果脚本依附的对象失活了，这个脚本也不会运行，下面的代码是让对象失活
        // 所以Lesson2和Lesson1没有被运行
        // 实际意思是，先把这个Lesson4脚本运行完，如果有对象创建并且激活了，再运行上面的脚本
        GameObject obj8 = new GameObject("顺便加脚本的空物体", typeof(Lesson2), typeof(Lesson1));

        //为对象添加脚本
        //继承MonoBehaviour的脚本是不能new
        //如果想要动态的添加继承MonoBehaviour的脚本 在某一个对象上
        //直接使用GameObject提供的方法即可
        Lesson1 les1 = obj6.AddComponent(typeof(Lesson1)) as Lesson1;
        //用泛型更方便
        Lesson2 les2 = obj6.AddComponent<Lesson2>();
        //得到的返回值是脚本信息
        //得到脚本的成员方法和继承Mono的类得到脚本的方法一模一样

        //标签比较
        //下面两种比较的方法 是一样的
        if (this.gameObject.CompareTag("Player"))
        {
            print("对象的标签 是 Player");
        }
        if (this.gameObject.tag == "Player")
        {
            print("对象的标签 是 Player");
        }

        //设置激活失活
        //false 失活
        //true 激活
        obj6.SetActive(false);
        obj7.SetActive(false);
        obj8.SetActive(false);

        //次要的成员方法，了解即可，不建议使用
        //下面讲的方法 都不建议大家使用，效率比较低
        //通过广播或者发送消息的形式让自己或者别人执行某些行为方法

        //通知自己执行什么行为
        //命令自己去执行这个TestFun这个函数 会在自己身上挂在的所有脚本去找这个名字的函数
        //找到自己身上所有的脚本有这个名字的函数去执行
        //意思是只要this.gameObject中的脚本中出现了TestFun的函数，都会执行，不管是哪个类
        // 当然，只是执行脚本文件名一样的类，如果脚本中有其他的类，不去执行，反射也找不到啊
        this.gameObject.SendMessage("TestFun");
        this.gameObject.SendMessage("TestFun2", 199);

        //广播行为 让自己和自己的子对象执行
        //this.gameObject.BroadcastMessage("函数名");

        //向父对象和自己发送消息 并执行
        //this.gameObject.SendMessageUpwards("函数名");
        #endregion
    }

    void TestFun()
    {
        print("Lesson4的TestFun");
    }

    void TestFun2(int a)
    {
        print("Lesson4的TestFun2" + a);
    }
}
```

# Time知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson5 : MonoBehaviour
{
    void Update()
    {
        #region Time相关内容主要用来干啥
        //时间相关内容 主要 用于游戏中参与位移、记时、时间暂停等
        #endregion

        #region 知识点一 时间缩放比例
        //时间停止
        //Time.timeScale = 0;
        //回复正常，默认
        //Time.timeScale = 1;
        //2倍速
        //Time.timeScale = 2;
        #endregion

        #region 知识点二 帧间隔时间
        //帧间隔时间 主要是用来计算位移
        //路程 = 时间*速度
        //根据需求选择参与计算的间隔时间
        //如果希望游戏暂停时就不动的，那就使用deltaTime
        //如果希望不受暂停影响unscaledDeltaTime

        //帧间隔时间：最近的一帧用了多长时间（秒），如果是第一帧，那固定是0.02s
        // 它是在当前帧的 Update() 被调用时，和上一帧的 Update() 之间的时间差
        //受scale影响，也就是实际计算的时候会乘上这个Time.timeScale
        //print("帧间隔时间" + Time.deltaTime);
        //不受scale影响的帧间隔时间
        //print("不受scale影响的帧间隔时间" + Time.unscaledDeltaTime);
        #endregion

        #region 知识点三 游戏开始到现在的时间
        //单机游戏中计时，从游戏开始到现在的时间
        //受scale影响
        //print("游戏开始到现在的时间:" + Time.time);
        //不受scale影响
        //print("不受scale影响的游戏开始到现在的时间:" + Time.unscaledTime);
        #endregion

        #region 知识点五 帧数
        // 一帧中，是多个脚本的多个Update在运行
        //从开始到现在游戏跑了多少帧(次循环)
        print(Time.frameCount);
        #endregion
    }

    #region 知识点四 物理帧间隔时间 FixedUpdate
    private void FixedUpdate()
    {
        //受scale影响
        //print(Time.fixedDeltaTime);
        //不受scale影响
        //print(Time.fixedUnscaledDeltaTime);
    }
    #endregion
}
```

# Transform
## Vector3基础和位置相关知识点
![Pasted image 20241118160300.png](https://s2.loli.net/2024/11/18/xIedDGX8Z6mNMw7.png)

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson6 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region Transform主要用来干嘛？
        //游戏对象（GameObject）位移、旋转、缩放、父子关系、坐标转换等相关操作都由它处理
        #endregion

        #region 知识点一 必备知识点 Vector3基础
        //Vector3主要是用来表示三维坐标系中的 一个点 或者一个向量
        //申明
        Vector3 v = new Vector3();
        v.x = 10;
        v.y = 10;
        v.z = 10;
        //只传xy 默认z是0
        Vector3 v2 = new Vector3(10, 10);
        //一步到位
        Vector3 v3 = new Vector3(10, 10, 10);

        // 默认(0,0,0)
        Vector3 v4;
        v4.x = 10;
        v4.y = 10;
        v4.z = 10;

        //Vector的基本计算
        // + - * /
        Vector3 v1 = new Vector3(1, 1, 1);
        Vector3 v12 = new Vector3(2, 2, 2);
        // 对应用x+或者-x.....
        print(v1 + v12); // (3,3,3)
        print(v1 - v12); // (-1,-1,-1)

        print(v1 * 10); // (10,10,10)
        print(v12 / 2); // (1,1,1)

        //常用
        // 右边 x
        // 上边 y
        // 前边 z
        print(Vector3.zero); // (0, 0, 0)
        print(Vector3.right); // (1, 0, 0)
        print(Vector3.left); // (-1, 0, 0)
        print(Vector3.forward); // (0, 0, 1)
        print(Vector3.back); // (0, 0, -1)
        print(Vector3.up); // (0, 1, 0)
        print(Vector3.down); // (0, -1, 0)

        //常用的一个方法
        //计算两个点之间的距离的方法
        print(Vector3.Distance(v1, v12));
        #endregion

        #region 知识点二 位置
        //相对世界坐标系，也就是绝对位置，如果这个对象是子对象，Unity中显示的是相对父对象的相对位置
        //this.gameObject.transform // 这个脚本依附的对象的位置信息
        //position的位置是相对于世界坐标系的原点的位置，可能和面板上显示的是不一样的
        //因为如果对象有父子关系 并且父对象位置，不在原点，那么和面板上肯定就是不一样的
        print(this.transform.position);

        //相对父对象
        //这两个坐标 对于我们来说 很重要 如果你想以面板坐标为准来进行位置设置
        //那一定是通过localPosition来进行设置的
        print(this.transform.localPosition);

        //他们两个 可能出现是一样的情况
        //1.父对象的坐标 就是世界坐标系原点0,0,0
        //2.对象没有父对象

        //注意：位置的赋值不能直接改变x，y，z 只能整体改变
        //不能单独改 x y z某一个值
        //this.transform.position.x = 10; // 报错
        this.transform.position = new Vector3(10, 10, 10);
        this.transform.localPosition = Vector3.up * 10;

        //如果只想改一个值x y和z要保持原有坐标一致
        //1.直接赋值
        this.transform.position = new Vector3(
            19,
            this.transform.position.y,
            this.transform.position.z
        );
        //2.先取出来 再赋值
        //虽然不能直接改 transform的 xyz 但是 Vector3是可以直接改 xyz的
        //所以可以先取出来改Vector3 再重新赋值
        Vector3 vPos = this.transform.localPosition;
        vPos.x = 10;
        this.transform.localPosition = vPos;

        //如果你想得到对象当前的 一个朝向
        //那么就是通过 trnasform.出来的
        //对象当前的面朝向
        // 那世界坐标系的朝向呢？这个是固定值啊，就是Vector3.right等等
        // 这个对象当前的面朝向是相对于世界坐标系朝向的，只是把对象的转换为世界坐标系下的表示了
        print(this.transform.forward);
        //对象当前的头顶朝向
        print(this.transform.up);
        //对象当前的右手边
        print(this.transform.right);

        #endregion
    }

    // Update is called once per frame
    void Update()
    {
        #region 知识点三 位移
        //理解坐标系下的位移计算公式
        //路程 = 方向 * 速度 * 时间

        #region 自己计算
        //方式一 自己计算
        //想要变化的 就是 position

        //用当前的位置 + 我要动多长距离  得出最终所在的位置
        //this.transform.position = this.transform.position + this.transform.up * 1 * Time.deltaTime;

        //因为我用的是 this.transform.forward 所以它始终会朝向相对于自己的面朝向去动
        //this.transform.position += this.transform.forward * 1 * Time.deltaTime;

        //方向非常重要 因为 它决定了你的前进方向
        //this.transform.position += Vector3.forward * 1 * Time.deltaTime;
        #endregion

        #region API计算
        //方式二 API
        //参数一：表示位移多少  路程 = 方向 * 速度 * 时间
        //参数二：表示相对坐标系，默认是相对于对象自己坐标系，意思就是以这个坐标系为标准坐标系，在上面进行偏移移动

        //1.相对于世界坐标系的Z轴动,始终是朝世界坐标系的Z轴正方向移动
        //this.transform.Translate(Vector3.forward * 1 * Time.deltaTime, Space.World);

        //2.相对于世界坐标的自己的面朝向去动,始终朝自己的面朝向移动
        //this.transform.Translate(this.transform.forward * 1 * Time.deltaTime, Space.World);

        //3.相对于自己的坐标系下的自己的面朝向向量移动,（一定不会这样让物体移动） XXXXXXX
        //this.transform.Translate(this.transform.forward * 1 * Time.deltaTime, Space.Self);

        //4.相对于自己的坐标系下的Z轴正方向移动,始终朝自己的面朝向移动
        this.transform.Translate(Vector3.forward * 1 * Time.deltaTime, Space.Self);

        //注意：一般使用API来进行位移
        // 如果上面2，4都启用，实际的是两者叠加的运动结果
        #endregion

        #endregion
    }
}
```

## 角度和旋转相关知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson7 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 角度相关
        //相对世界坐标角度
        //print(this.transform.rotation);// 这个是四元数
        print(this.transform.eulerAngles); // 和Unity中显示的一样

        //相对父对象角度
        print(this.transform.localEulerAngles);

        //注意：设置角度和设置位置一样 不能单独设置xyz 要一起设置
        //如果我们希望改变的 角度 是面板上显示的内容 那一点是改变 相对父对象的角度
        //this.transform.localEulerAngles = new Vector3(10, 10, 10);
        //this.transform.eulerAngles = new Vector3(10, 10, 10);
        print(this.transform.localEulerAngles);
        #endregion
    }

    // Update is called once per frame
    void Update()
    {
        #region 知识点二 旋转相关
        //自己计算（省略不讲了 和位置一样 不停改变角度即可）

        //API计算
        //自转
        //每个轴具体转多少度，就是每个轴每帧改变多少
        //第一个参数：相当于是旋转的角度每一帧
        //第二个参数：默认不填，就是相对于自己坐标系进行的旋转，Space.Self
        //this.transform.Rotate(new Vector3(0, 10, 0) * Time.deltaTime);
        //this.transform.Rotate(new Vector3(0, 10, 0) * Time.deltaTime, Space.World);

        //相对于某个轴转多少度,在【第三个参数】的坐标系下，绕找轴【第一个参数】进行旋转，旋转角度是【第二个参数】
        //参数一：是相对哪个轴进行转动
        //参数二：是转动的角度是多少
        //参数三：默认不填,就是相对于自己的坐标系进行旋转
        //       如果填，可以填写相对于世界坐标系进行旋转
        //this.transform.Rotate(Vector3.right, 10 * Time.deltaTime);
        //this.transform.Rotate(Vector3.right, 10 * Time.deltaTime, Space.World);

        //相对于某一个点产生的轴进行旋转
        //参数一：相当于哪一个点转圈圈
        //参数二：相对于哪一个点的哪一个轴转圈圈
        //参数三：转的度数：旋转速度 * 时间
        // RotateAround 的轴和中心点都是明确定义的，不依赖对象的本地坐标系
        // 使用的坐标系都是Space.World
        this.transform.RotateAround(Vector3.zero, Vector3.right, 10 * Time.deltaTime);
        #endregion
    }
}
```

## 缩放和看向相关知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson8 : MonoBehaviour
{
    public Transform lookAtObj;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 缩放
        //相对世界坐标系
        // lossyScale只有get，没有set
        print(this.transform.lossyScale);
        //相对本地坐标系（父对象）
        print(this.transform.localScale);

        //注意：
        //1.同样缩放不能只改xyz,只能一起改(相对于世界坐标系的缩放大小只能得,不能改)
        //一般要修改缩放大小，都是改相对于父对象的缩放大小localScale
        //this.transform.localScale = new Vector3(3, 3, 3);

        //2.Unity没有提供关于缩放的API
        //之前的 旋转 位移 都提供了 对应的 API 但是 缩放并没有
        //如果你想要 让 缩放 发生变化 只能自己去写(自己算)
        #endregion
    }

    // Update is called once per frame
    void Update()
    {
        //2.Unity没有提供关于缩放的API
        //之前的 旋转 位移 都提供了 对应的 API 但是 缩放并没有
        //如果你想要 让 缩放 发生变化 只能自己去写(自己算)
        //this.transform.localScale += Vector3.one * Time.deltaTime;

        #region 知识点二 看向
        //让一个对象的面朝向（z轴）一直看向某一个点或者某一个对象
        //看向一个点相对于世界坐标系的
        //this.transform.LookAt(Vector3.zero);
        //看向一个对象，传入一个对象的Transform信息，也是看向对象的Position的信息
        // 第二个参数的意思是，让this.transform对象的【第二个参数的方向】尽可能的对齐lookAtObj的y轴
        // 默认不填是Vector3.up
        this.transform.LookAt(lookAtObj, Vector3.up);
        #endregion
    }
}
```

## 父子关系相关知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson9 : MonoBehaviour
{
    public Transform son;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 获取和设置父对象
        //获取父对象
        // 如果this.transform没有父对象，那么这个this.transform.parent就是null
        //print(this.transform.parent.name);
        //设置父对象，断绝父子关系
        //this.transform.parent = null;
        //设置父对象，认爸爸
        //this.transform.parent = GameObject.Find("Father2").transform;

        //通过API来进行父子关系的设置
        //this.transform.SetParent(null);//断绝父子关系
        //this.transform.SetParent(GameObject.Find("Father2").transform);//认爸爸

        //参数一：父对象
        //参数二：是否保留世界坐标的位置，角度，缩放，信息
        //       true：会保留世界坐标下的状态和父对象进行计算，得到本地坐标系的信息，其实也就是进行了父对象和子对象的绑定，其他没变
        //       false：不会保留世界坐标系下的状态，直接把世界坐标系下的位置角度缩放，直接赋值到本地坐标系下，改变了子对象的位置信息
        //this.transform.SetParent(GameObject.Find("Father3").transform, false);
        #endregion

        #region 知识点二 所有子对象关系分离
        //和自己的子对象的关系进行分离，但是子对象自己的关系不管，如果子对象还有子对象，这个关系保留
        //this.transform.DetachChildren();
        #endregion

        #region 知识点三 获取子对象
        //按名字查找儿子
        //找到儿子的transform信息
        //Find方法 是能够找到 失活的对象的 ！！！！！ GameObject相关的查找是不能找到失活对象的
        print(this.transform.Find("Cube (1)").name);
        //只能找到自己的儿子 找不到自己的孙子 ！！！！！！
        //print(this.transform.Find("GameObject").name);
        //的效率比GameObject.Find相关要高一些，但是前提是你必须知道父亲是谁才能找

        //遍历儿子
        //如何得到有多少个儿子
        //1.失活的儿子也会算数量
        //2.找不到孙子，所以孙子不会算数量
        print(this.transform.childCount);
        //通过索引号 去得到自己对应的儿子
        //如果编号超出了儿子数量的范围，会直接报错的
        //返回值是transform，可以得到对应儿子的位置相关信息
        this.transform.GetChild(0);

        for (int i = 0; i < this.transform.childCount; i++)
        {
            print("儿子的名字：" + this.transform.GetChild(i).name);
        }
        #endregion

        #region 知识点四 儿子的操作
        //判断自己的爸爸是谁
        //一个对象判断自己是不是另一个对象的儿子
        if (son.IsChildOf(this.transform))
        {
            print("是我的儿子");
        }
        //得到自己作为儿子的编号
        print(son.GetSiblingIndex());
        //把自己设置为第一个儿子
        // 这个在Hierarchy中的上下顺序，也就是实际的儿子对应的索引
        son.SetAsFirstSibling();
        //把自己设置为最后一个儿子
        son.SetAsLastSibling();
        //把自己设置为指定个儿子
        //索引超出了范围（负数或者更大的数） 不会报错，会直接设置成最后一个编号
        son.SetSiblingIndex(1);
        #endregion
    }
}
```

- 使用`this.transform.DetachChildren()`去除父子关系
- 如果子对象还有子对象，不影响子对象的关系
![Pasted image 20241118203515.png](https://s2.loli.net/2024/11/18/1HfoxrEDvkZn5PG.png)
![Pasted image 20241118203534.png](https://s2.loli.net/2024/11/18/CFgcxXYuqKVW2Dr.png)

## 坐标转换相关知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 世界坐标转本地坐标
        print(Vector3.forward);

        //世界坐标系转本地坐标系，可以帮助我们大概判断一个相对位置
        //世界坐标系的点转换为相对本地坐标系的点
        //受到缩放影响，比例相反
        // Vector3.forward是世界坐标系下的点；this.transform是自己想要作为参考系的坐标系
        // 将世界坐标系下的点Vector3.forward转换为this.transform坐标系下的点
        print("转换后的点 " + this.transform.InverseTransformPoint(Vector3.forward));

        //世界坐标系的方向转换为相对本地坐标系的方向
        // 世界坐标系的方向可以理解为，从(0,0,0)开始的一个向量（方向）
        // 从世界坐标系转换为相对对象坐标系的一个向量，模长不变
		// 也可以理解为一个向量从世界坐标系的原点移动到本地坐标系的原点，然后求在本地坐标系下的终点的坐标
        //不受缩放影响
        print("转换后的方向" + this.transform.InverseTransformDirection(Vector3.forward));
        //受缩放影响，比例相反
        print("转换后的方向(受缩放影响)" + this.transform.InverseTransformVector(Vector3.forward));
        #endregion

        #region 知识点二 本地坐标转世界坐标，更重要
        //本地坐标系的点转换为相对世界坐标系的点，受到缩放影响
        // 这里Vector3.forward是this.transform坐标系上的一个点
        // 将其转换为为世界坐标系下的坐标，缩放一致
        print("本地 转 世界 点" + this.transform.TransformPoint(Vector3.forward));

		//本地坐标系的方向 转换 为相对世界坐标系的方向
		// 也可以理解为一个向量从本地坐标系的原点移动到世界坐标系的原点，然后求在世界坐标系下的终点的坐标
		//不受缩放影响
		print("本地 转 世界 方向" + this.transform.TransformDirection(Vector3.forward));
        //受缩放影响，缩放一致
        print("本地 转 世界 方向" + this.transform.TransformVector(Vector3.forward));
        #endregion
    }
}
```

# Input触摸手柄陀螺仪相关知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson11 : MonoBehaviour
{
    // Update is called once per frame
    void Update()
    {
        #region 注意：输入相关内容肯定是写在Update中的

        #endregion

        #region 知识点一 鼠标在屏幕位置
        //屏幕坐标的原点 是在 屏幕的左下角  往右是X轴正方向 往上时Y轴正方向
        //返回值时Vector3 但是只有 x和y有值 z一直是0 是因为屏幕本来就是2D的 不存在Z轴
        //print(Input.mousePosition);
        #endregion

        #region 知识点二 检测鼠标输入
        //鼠标按下相关检测 对于我们来说
        //比如： 1.可以做 发射子弹
        //      2.可以控制摄像机 转动

        //鼠标按下一瞬间 进入
        //0左键 1右键 2中键
        //只要按下的这一瞬间 进入一次
        if (Input.GetMouseButtonDown(0))
        {
            print("鼠标左键按下了");
        }

        //鼠标抬起一瞬间 进入
        if (Input.GetMouseButtonUp(0))
        {
            print("鼠标左键抬起了");
        }

        //鼠标长按按下抬起都会进入
        //就是 当按住按键不放时 会一直进入 这个判断
        if (Input.GetMouseButton(1))
        {
            print("右键按下");
        }

        //中键滚动
        //返回值的 y -1往下滚  0没有滚  1往上滚
        //它的返回值 是Vector的值 我们鼠标中键滚动 会改变其中的Y值
        // x: 鼠标滚轮在水平方向（横向滚动）上的变化量。通常只有在支持横向滚动的触控板或鼠标设备上有效。
        // y: 鼠标滚轮在垂直方向（上下滚动）上的变化量。对于普通鼠标滚轮，y 是最常用的属性。
        //print(Input.mouseScrollDelta);

        #endregion

        #region 知识点三 检测键盘输入
        //比如说 按一个键释放一个技能或者切换武器 等等的操作

        //键盘按下，建议使用这个
        if (Input.GetKeyDown(KeyCode.W))
        {
            print("W键按下");
        }

        //传入字符串的重载
        //Input.GekeyDown()只能传入小写，不能是大写，不然会报错
        //不过按键按大写和小写都你能触发下一行代码
        // 不建议使用这个
        //if (Input.GetKeyDown("q"))
        //{
        //    print("q按下");
        //}

        //键盘抬起
        if (Input.GetKeyUp(KeyCode.W))
        {
            print("W键抬起");
        }

        //键盘长按
        if (Input.GetKey(KeyCode.W))
        {
            print("W键长按");
        }

        #endregion

        #region 知识点四 检测默认轴输入
        //我们学习鼠标 键盘输入 主要是用来
        //控制玩家 比如 旋转 位移等等
        //所以Unity提供了 更方便的方法 来帮助我们控制 对象的 位移和旋转

        //键盘AD按下时 返回 -1(A)到1(D)之间的变换
        // 没有按时，会慢慢回到0
        //相当于 得到得这个值 就是我们的 左右方向 我们可以通过它来控制 对象左右移动 或者左右旋转
        //print(Input.GetAxis("Horizontal"));

        //键盘SW按下时 返回 -1(S)到1(W)之间的变换
        // 没有按时，会慢慢回到0
        //得到得这个值 就是我们的 上下方向 我们可以通过它来控制 对象上下移动 或者上下旋转
        //print(Input.GetAxis("Vertical"));

        //鼠标横向移动时 -1(左) 到 1(右) 左 右
        // 测试了，这个的值左边是小于0，右边是大于0，并不是一直处于[-1,1]
        // 速度越大，绝对值越大
        // 没有移动时，会慢慢回到0
        print(Input.GetAxis("Mouse X"));

        //鼠标竖向移动时  -1(下) 到 1(上) 下 上
        // 测试了，这个的值下面是小于0，上面是大于0，并不是一直处于[-1,1]
        // 速度越大，绝对值越大
        // 没有移动时，会慢慢回到0
        print(Input.GetAxis("Mouse Y"));

        //我们默认的 GetAxis方法 是有渐变的 会总 -1~0~1之间 渐变 会出现小数

        //GetAxisRaw方法 和 GetAxis使用方式相同
        //只不过 它的返回值 只会是 -1 0 1 不会有中间值
        #endregion

        #region 其它
        //是否有任意键或鼠标长按
        if (Input.anyKey)
        {
            print("有一个键长按");
        }
        //是否有任意键或鼠标按下
        if (Input.anyKeyDown)
        {
            print("有一个键 按下");
            //这一帧的键盘输入
            print(Input.inputString);
        }

        //手柄输入相关
        //得到连接的手柄的所有按钮名字
        string[] strs = Input.GetJoystickNames();

        //某一个手柄键按下
        if (Input.GetButtonDown("Jump")) { }

        //某一个手柄键抬起
        if (Input.GetButtonUp("Jump")) { }

        //某一个手柄键长按
        if (Input.GetButton("Jump")) { }

        //移动设备触摸相关
        //如果手机被触碰了
        if (Input.touchCount > 0)
        {
            // 触摸的第一个点
            Touch t1 = Input.touches[0];
            //位置
            print(t1.position);
            //相对上次位置的变化
            print(t1.deltaPosition);
        }

        //是否启用多点触控
        Input.multiTouchEnabled = false;

        //陀螺仪（重力感应）
        //是否开启陀螺仪，必须开启下面的功能才能正常使用
        Input.gyro.enabled = true;
        //重力加速度向量
        print(Input.gyro.gravity);
        //旋转速度
        print(Input.gyro.rotationRate);
        //陀螺仪 当前的旋转四元数
        //比如 用这个角度信息 来控制 场景上的一个3D物体受到重力影响
        //手机怎么动 它怎么动
        print(Input.gyro.attitude);
        #endregion
    }
}
```

- Input.GetAxis()里面的参数可以参考Project Setting的Input Manager
![Pasted image 20241118220201.png](https://s2.loli.net/2024/11/18/taEAFI5USqgP2ZC.png)
![Pasted image 20241118220143.png](https://s2.loli.net/2024/11/18/r7fmc2eyBq1ECTF.png)

# 屏幕相关Screen知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson12 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 静态属性

        #region 常用
        //当前屏幕分辨率
        Resolution r = Screen.currentResolution;
        print("当前屏幕分辨率的宽" + r.width + "高" + r.height);

        //当前屏幕窗口宽高
        //是当前窗口的宽高，不是设备分辨率的宽高
        //一般写代码要用窗口宽高，不然窗口缩小后，UI会乱
        print(Screen.width);
        print(Screen.height);

        //屏幕休眠模式
        // 永不熄屏
        Screen.sleepTimeout = SleepTimeout.NeverSleep;
        // 系统设置
        //Screen.sleepTimeout = SleepTimeout.SystemSetting;
        #endregion

        #region 不常用
        //运行时是否全屏模式
        Screen.fullScreen = true;
        //窗口模式
        //独占全屏：FullScreenMode.ExclusiveFullScreen
        //全屏窗口：FullScreenMode.FullScreenWindow
        //最大化窗口：FullScreenMode.MaximizedWindow
        //窗口模式：FullScreenMode.Windowed
        Screen.fullScreenMode = FullScreenMode.Windowed;

        //移动设备屏幕转向相关
        //允许自动旋转为左横向 Home键在左
        Screen.autorotateToLandscapeLeft = true;
        //允许自动旋转为右横向 Home键在右
        Screen.autorotateToLandscapeRight = true;
        //允许自动旋转到纵向 Home键在下
        Screen.autorotateToPortrait = true;
        //允许自动旋转到纵向倒着看 Home键在上
        Screen.autorotateToPortraitUpsideDown = true;

        //指定屏幕显示方向
        //ScreenOrientation.AutoRotation：自动旋转
        //ScreenOrientation.Landscape：横屏，左横向、右横向都可以
        //ScreenOrientation.LandscapeLeft：左横向
        //ScreenOrientation.LandscapeRight：右横向
        //ScreenOrientation.Portrait：竖屏
        //ScreenOrientation.PorraitUpsideDown：倒竖屏
        //ScreenOrientation.Unknown：弃用
        Screen.orientation = ScreenOrientation.Landscape;
        #endregion

        #endregion

        #region 知识点二 静态方法
        //设置分辨率 一般移动设备不使用
        // 第三个参数是否全屏。false不是全屏，true是全屏
        //true：游戏以全屏模式运行。
        //false：游戏以窗口模式运行。
        Screen.SetResolution(1920, 1080, false);
        #endregion
    }
}
```


![Pasted image 20241118222953.png](https://s2.loli.net/2024/11/18/UiEdyJHT4zSPBQV.png)

# Camera可编辑参数知识点

- Clear Flags
	- Skybox
		- 天空盒，主要用在3D游戏
	- Solid Color
		- 纯色填充，主要用在2D游戏
	- Depth only
		- 叠加渲染，和Camera脚本的Depth一起使用
		- 一般用在Depth较高的相机上面，后渲染的相机只渲染物体，背景不渲染，是透明的，所以可以叠加到另一个相机渲染的画面上
	- Don't Clear
		- 不清除渲染
- Culling Mask
	- 表示这个相机可以渲染哪些层级
- Projection
	- Perspective 透视模式 
		- 主要用于3D游戏
	- Orthographic 正交摄像机
		- 主要用于2D游戏
- FOV Axis
	- 视场角
	- 决定视场的角度由垂直还是水平决定
	- 一般竖直就可以了
- Field of View
	- 一般不改
	- 如果值变小，东西会变近，相机可观测距离变远，但是变窄
	- 如果值变大，东西会变远，相机可观测距离变近，但是变宽
- Size
	- 正交摄像机的观察范围
	- 值越大，范围越大，但物体也变小；值越小，范围越小，但物体变大
- Clipping Planes
	- 裁剪平面距离
		- Near 最小可以看见的距离
		- Far 最大可以看见的距离
- Viewport Rect
	- 视口范围，屏幕上将绘制该摄像机视图的位置；主要用于双摄像机游戏；0~1是百分比的意思
	- X 0 Y 0 W 0.5 H 0.5
		- 意思是从左下角(0,0)开始，向上50%的高度，向右的宽度的窗口显示
- Depth
	- 渲染顺序上的深度
	- 数值越小，渲染顺序越靠前。渲染靠后的会覆盖渲染靠前的
- Rendering Path
	- 渲染路径
- Target Texture
	- 渲染纹理
	- 把摄像机看到的内容渲染到Render Texture上，这个选项上正好可以加一个Render Texture
	- 可以用于创建小地图，新建一个相机，然后俯视观察对象，将渲染的内容放在Render Texture上
- Occlusion Culling 
	- 是否启用剔除遮挡
	- 如果某个对象，完全被另一个对象遮挡，那么这个被遮挡的对象就不再渲染，可以节约性能
- Allow HDR
	- 是否允许高动态范围渲染
- Allow MSAA
	- 是否允许抗锯齿
- Allow Dynamic Resolution
	- 是否允许动态分辨率呈现
- Target Display
	- 用于哪个显示器
- Target Eye
	- VR用的
![Pasted image 20241118224450.png](https://s2.loli.net/2024/11/18/iG9Cfuawc41NXZe.png)
- 裁剪平面
- 如果物体在裁剪平面范围以外，会看不见物体
![Pasted image 20241118225935.png](https://s2.loli.net/2024/11/18/8biSk4FRKqsyvZc.png)


![[摄像机参数说明.png](https://s2.loli.net/2024/11/18/apt9ITxSeC56Xbl.png)

![摄像机参数说明2.png]![.png](https://s2.loli.net/2024/11/18/apt9ITxSeC56Xbl.png)

# Camera代码相关的知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson14 : MonoBehaviour
{
    public Transform obj;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 重要静态成员
        //1.获取摄像机
        //主摄像机的获取
        //如果想通过这种方式 快速获取摄像机 那么场景上必须有一个 tag为MainCamera的摄像机
        // 如果有两个MainCamera，不知道获取的是哪个
        print(Camera.main.name);
        //获取摄像机的数量
        print(Camera.allCamerasCount);
        //得到所有摄像机
        Camera[] allCamera = Camera.allCameras;
        print(allCamera.Length);

        //2.渲染相关委托
        //摄像机剔除对象不渲染前处理的委托函数
        // 和Camera脚本上的Occlusion Culling有关
        Camera.onPreCull += (c) => { };
        //摄像机 渲染前处理的委托
        Camera.onPreRender += (c) => { };
        //摄像机 渲染后 处理的委托
        Camera.onPostRender += (c) => { };
        #endregion

        #region 知识点二 重要成员
        //1.界面上的参数 都可以在Camera中获取到
        //比如 下面这句代码 就是得到主摄像机对象 上的深度 进行设置
        Camera.main.depth = 10;

        //2.世界坐标转屏幕坐标 this.transform.position就是自己想要转换的世界坐标
        // 屏幕坐标就是game左下角为(0,0),物体距离左下角的实际坐标
        //转换过后x和y对应的就是屏幕坐标，z对应的是这个3D物体里我们的摄像机有多远
        //我们会用这个来做的功能最多的就是头顶血条相关的功能，z轴可以控制血条的近大远小
        Vector3 v = Camera.main.WorldToScreenPoint(this.transform.position);
        print(v);
        //3.屏幕坐标转世界坐标
        //print(Camera.main.ScreenToWorldPoint(v));

        #endregion
    }

    void Update()
    {
        //3.屏幕坐标转世界坐标
        //只所以改变Z轴 是因为 如果不改 Z默认为0
        //转换过去的世界坐标系的点 永远都是一个点 可以理解为 视口 相交的焦点
        //如果改变了Z 那么转换过去的 世界坐标的点 就是相对于 摄像机前方多少的单位的横截面上的世界坐标点
        Vector3 v = Input.mousePosition;
        // z的意思就是在相机前面多少的距离
        v.z = 5;
        obj.position = Camera.main.ScreenToWorldPoint(v);
        //print(Camera.main.ScreenToWorldPoint(v));
    }
}
```

# 光源组件知识点
- 右键，能看到很多类型的光源
![Pasted image 20241120113043.png](https://s2.loli.net/2024/11/25/uyM1sbf5wr2SF7v.png)
- Directional Light （方向光）
	- Shadow Type
		- NoShadows 关闭阴影
			- 性能消耗最低
			- ![Pasted image 20241120121053.png](https://s2.loli.net/2024/11/26/PoGbQKvxY6EBhg1.png)
		- HardShadows 生硬阴影
			- 性能消耗中等
			  ![Pasted image 20241120121113.png](https://s2.loli.net/2024/11/26/BGIPvMnJfpDhN4b.png)
		- SoftShadows 柔和阴影
			- 性能消耗最高
			  ![Pasted image 20241120121128.png](https://s2.loli.net/2024/11/25/e9H2rxUkzd5Y1Mb.png)  
  - Cookie Size
	  - 遮罩大小，只有在Directional Light里面才有
	  - 方向光一般不设置遮罩大小
  - Render Mode 渲染模式
	  - Auto 运行时确定
	  - Important 以像素质量为单位进行渲染，效果逼真，消耗大
	  - Not Important 以快速模式进行渲染
![Pasted image 20241120120020.png](https://s2.loli.net/2024/11/25/q3ZcbdLYMvJXWe8.png)
- Point Light（点光源）
	- Range
		- 发光范围
	- Color
		- 光源的颜色
	- Mode
		- Realtime 实时光源
			- 每帧实时计算，效果好，消耗性能大
		- Baked 烘焙光源
			- 预先计算
		- Mixed 混合光源
			- 预先计算+实时计算
	- Intensity
		- 光照强度
		- 值越大，光越亮
	- Indirect Multiplier
		- 改变间接光的强度
			- 低于1，每次反弹会使光更暗
			- 等于1，无变化
			- 高于1，每次反弹会使光更亮（山洞）
	- Draw Halo （球形光环开关）
		- 是一个true或者false的开关，开启了效果如下图所示，一般用在方向光上
	- Flare （耀斑）
		- 一般较强的光才有
	- Culling Mask （剔除遮罩层）
		- 决定哪些层的对象受到该光源的影响，如果没有选中这个层，那么这个层的对象不会受到光源的影响
	- RealtimeShadows 
		- Strength 阴影暗度0~1之间，越大越黑，指的是影子的明暗程度
		- Resolution 阴影贴图渲染分辨率，越高越逼真，消耗越高，可以自己设置，也可以使用默认的几个设置
		- Bias 阴影推离光源的距离 计算阴影时使用的参数
		- Normal Bias 阴影投射面沿法线收缩距离 计算阴影时使用的参数
		- Near Panel 渲染阴影的近裁剪面 计算阴影时使用的参数
![Pasted image 20241120114736.png](https://s2.loli.net/2024/11/25/L4uFoxKlp2hIgUv.png)
- Draw Halo （球形光环开关）
![Pasted image 20241121114528.png](https://s2.loli.net/2024/11/25/yLZq3tmXK6iOPY9.png)
- Flare （耀斑）
![Pasted image 20241121115201.png](https://s2.loli.net/2024/11/25/6TotuY5gmb8BIRE.png)
- 耀斑要想在界面上看见，摄像机要加这个脚本（Falre Layer）
![Pasted image 20241121115229.png](https://s2.loli.net/2024/11/25/pHfijuIen4gL2kM.png)

- Spot Light（聚光灯）
	- Range
		- 发光距离，值变大了，但是原本照射的地方距离不变，就会导致原本位置的亮度变大
	- Spot Angle
		- 聚光灯发光圆圈的半径（角度）
	- Cookie （投影遮罩）
		- 更换Cookie的材质对象，能够呈现不同的显示效果
![Pasted image 20241120115019.png](https://s2.loli.net/2024/11/25/dWs8zvGEo2BMyK3.png)
- 投影遮罩
![Pasted image 20241121113657.png](https://s2.loli.net/2024/11/25/pSW7ZhfbCIPABq4.png)

- Area Light（面光源）
- 实际看起来没啥效果，是烘焙光源（Baked）
- 烘焙
	- 提前计算好，然后贴图上去，并不会实时计算光源，节约性能

![光源参数说明1.png](https://s2.loli.net/2024/11/25/IcGHgNm128nVuoT.png)

![光源参数说明2.png]![2.png](https://s2.loli.net/2024/11/25/iz4crBunRbPLUhI.png)



# 光面板相关
- Window->Rendering->Lighting
![Pasted image 20241121194630.png](https://s2.loli.net/2024/11/25/46wOZGRiLnevMEx.png)
- 重要的是Environment
![Pasted image 20241121194731.png](https://s2.loli.net/2024/11/26/YxeDfPJjuHcULBT.png)

- Skybox Material
- 天空盒材质的创建
![Pasted image 20241121195016.png](https://s2.loli.net/2024/11/26/Irmp4QCoV68xqyS.png)
![Pasted image 20241121195106.png](https://s2.loli.net/2024/11/26/DKkMlrsuah5pzE1.png)
![Pasted image 20241121195125.png](https://s2.loli.net/2024/11/26/9ECRoVwtG8WsZn6.png)
![Pasted image 20241121195145.png](https://s2.loli.net/2024/11/26/kchsTf14xQr26PI.png)

- Sun Source 太阳来源
> 当场景上有很多个方向光，需要指定某个作为太阳光，才设置这个

- Environment Lighting
	- Source 
		- 环境光的叠加会和来源叠加
		- Skybox 天空盒材质作为环境光颜色
		- Gradient 为天空、地平线、地面单独选择颜色和他们之间混合 
	- Intensity Multiplier 
		- 光强度，需要在山洞里面用的多
	- 

![光面板说明1.png](https://s2.loli.net/2024/11/26/oxRckLHsYKGWX1E.png)


-  Fog （性能消耗比较大）
	- Color 雾的颜色
	- Mode 雾影响和距离的关系
		- Linear 线性
			- Start 离相机多远开始有雾
			- End 离相机多远开始完全看不见
		- Exponential 指数型
		- Exponential Qquare 比指数更快
- Halo Texture
	- 光源周围光环的纹理，可以是圆的，方的。。。
- Halo Strength
	- 光环可见性，改的是光环的半径大小
- Flare Fade Speed
	- 耀斑最初出现之后，淡出的时间
- Flare Strength
	- 耀斑可见性，值越小，能够看见的半径越小
- Spot Cookie
	- 探照灯的形状，这个是默认设置，如果创建一个Spot Light，默认就是这个
![光面板说明2.png](https://s2.loli.net/2024/11/26/1tRWGiwnPcp8KYy.png)

# 碰撞检测之刚体
> 碰撞产生的必要条件
> 两个物体都要有碰撞器，只要一个物体有刚体

- 这个绿色的颜色就是碰撞器实际范围
![Pasted image 20241121203407.png](https://s2.loli.net/2024/11/26/LRpdbgyPw4cUkZ9.png)
- Rigidbody 刚体脚本（刚体）
- Mass 质量（默认为千克）
	- 质量越大，惯性越大
- Drag 空气阻力
	- 根据力移动对象时影响对象的空气阻力大小
	- 0表示没有空气阻力
- Angular Drag 扭矩空气阻力
	- 根据扭矩旋转对象时影响对象的空气阻力大小。0表示没有空气阻力
	- 0的话则此时如果物体被撞旋转了，会一直旋转，不是0，会慢慢停止旋转
- use Gravity
	- 是否受重力影响
- Is Kinematic 物理运动学
	- 如果启动此选项，则对象将不会被物理引擎驱动，只能通过Transform对其操作
	- 对于移动平台，或者如果要动画化附加了HingeJoint的刚体，此属性非常有用
	- 理解为可以受到碰撞，但是状态不会改变了
- Interpolate 插值运算，让刚体物体移动更加平滑，如果物理帧很长的话，可以通过设置这个让他更平滑
	- None 不应用插值运算
	- Interpolate 根据前一帧的变换来平滑变换
	- Extrapolate 根据下一帧的估计变换来平滑变换
- Collision Detection （碰撞检测模式）
	- 用于防止快速移动的对象穿过其他对象而不检测碰撞
	- Discrete （离散检测）
		- 对场景中的所有其他碰撞体使用离散碰撞检测。其他碰撞体在测试碰撞时会使用离散碰撞检测。用于正常碰撞（这是默认值）
	- Continuous （连续检测）
		- 对动态碰撞体（具有刚体）使用离散碰撞检测
		- 并对静态碰撞体（没有刚体）使用连续碰撞检测。
		- 设置为连续动态（Continuous Dynamic）的刚体将在测试与该刚体的碰撞时使用连续碰撞检测。（此属性对物理性能有很大影响，如果没有快速对象的碰撞问题，请将其保留为Discrete设置）
		- 其他刚体将使用离散碰撞检测。
	- Continuous Dynamic （连续动态检测） 性能消耗高
		- 对设置为连续（Continuous）和连续动态（Continuous Dynamic）碰撞的游戏对象使用连续碰撞检测。还将对静态碰撞体（没有刚体）使用连续碰撞检测。
		- 对于所有其他碰撞体，使用离散碰撞检测。用于快速移动的对象。
	- Continuous Speculative （连续推测检测）
		- 对刚体和碰撞体使用推测性连续碰撞检测。该方法通常比连续碰撞检测的成本更低。
	- ![Pasted image 20241121210445.png](https://s2.loli.net/2024/11/26/DMQr1U5mLFqpzGH.png)
- Constraints 约束，对刚体运动的限制
	- Freeze Position
		- 有选择地停止刚体沿世界X、Y和Z轴的移动
		- 如果选择了X和Z轴，那么X和Z的位置在碰撞时就不会变
	- Freeze Rotation
		- 有选择地停止刚体围绕局部X、Y和Z轴的移动
		- 如果选择了Y轴，那么在碰撞时就不会绕着Y轴旋转
![Pasted image 20241121203602.png](https://s2.loli.net/2024/11/26/3nWsJv78aPXxbcQ.png)


![Pasted image 20241121204429.png](https://s2.loli.net/2024/11/26/KFB7G5DCS21RpjO.png)

- 这个时间和实际的物理更新有关，这个值大的话，表示过一秒后才更新物理世界，可能会发生类似卡顿的情况
![Pasted image 20241121205301.png](https://s2.loli.net/2024/11/26/EPomSaKAH56XgqU.png)



![刚体参数1.png](https://s2.loli.net/2024/11/26/gySBfKFl7HM1u86.png)

![刚体参数2.png](https://s2.loli.net/2024/11/26/NGRPC3TVoqye2F7.png)

![刚体参数3.png](https://s2.loli.net/2024/11/26/fqWckHyV3w54sUT.png)

# 碰撞检测之碰撞器
- 盒状、球状、胶囊状的3D碰撞器比较常用，准确性稍低，但是性能要求也低
- 网格、轮胎，地形的3D碰撞器准确性稍高，但是性能要求也高

- IsTrigger
	- 是否是触发器，如果启用此属性
	- 则该碰撞体将用于触发事件，并被物理引擎忽略主要用于进行没有物理效果的碰撞检测
	- 比如，箭，会穿过怪物，但是，箭不会和怪物实际产生相互碰撞，一起后退的效果
- Material 
	- 物理材质，可以确定碰撞体和其它对象碰撞时的交互（表现）方式。
- Center
	- 碰撞体在对象局部空间中的中心点位置,下图就是改了Center的值产生的结果
	- ![Pasted image 20241121214805.png](https://s2.loli.net/2024/11/26/sORGpalnEBFq4Dc.png)
- Edit Collider
	- 改变碰撞器的形状
- BoxCollider 盒状碰撞器
	- Size 碰撞体在X、Y、Z方向上的大小
- Sphere Collider 球状碰撞器
	- Radius 球形碰撞体的半径大小
- Capsule Collider 胶囊碰撞器
	- Radius  胶囊体的半径
	- Height 胶囊体的高度
	- Direction 胶囊体在对象局部空间中的轴向

- 异形物体使用多种碰撞器组合
	- 刚体对象的子对象碰撞器信息，参与碰撞检测
	- 比如这种的金字塔，下面的子对象是加了碰撞器Collider，但是金字塔没有碰撞器，只有刚体，这样组合后就会和实际的金字塔的碰撞情况一致了，是一种处理异形物体的方法
	- ![Pasted image 20241121220308.png](https://s2.loli.net/2024/11/26/E5D4jyoQLCFnSgA.png)
- Mesh Collider
	- Convex
		- 加了Rigidbody的圆柱体必须勾选Mesh Collider的Convex的选项，否则没有碰撞体力的效果
		- 勾选此复选框可启用Convex。如果启用此属性，该MeshCollider将与其他 Mesh Collider 发生碰撞。ConvexMesh Collider最多 255 个三角形
	- Cooking Options
		- 启用或禁用影响物理引擎对网格处理方式的网格烹制选项。
	- Mesh
		- 引用需要用于碰撞的网格

- Wheel Collider
	- Car是一个整体的父对象，表示是一个整体的汽车，需要加上Rigidbody脚本，并且Mass设置大一些，否则会弹飞，如果没有加Rigidbody脚本，wheel Collider无法显示
		- Cube是车的身体
		- Root是汽车的四个轮胎，需要加上wheel Collider
		- ![Pasted image 20241121221334.png](https://s2.loli.net/2024/11/26/sEC6waxd2OYmt9R.png)
![碰撞器参数说明1.png](https://s2.loli.net/2024/11/26/slwtaSRUXc5Yiuy.png)

![碰撞器参数说明2.png](https://s2.loli.net/2024/11/26/xj6CytXopkLriA3.png)

![碰撞器参数说明3.png](https://s2.loli.net/2024/11/26/jKBJtp9OMdgRETw.png)

![碰撞器参数说明4.png](https://s2.loli.net/2024/11/26/VjGW9Z3ebuzyYiK.png)

![碰撞器参数说明5.png](https://s2.loli.net/2024/11/26/Ah9QyueoENsHvxk.png)
# 碰撞检测之物理材质
- 创建物理材质
![Pasted image 20241121221922.png](https://s2.loli.net/2024/11/26/OTgoWQ9AmKFkVS6.png)
- 物理材质在碰撞器上的位置
![Pasted image 20241121222023.png](https://s2.loli.net/2024/11/26/NY5tfOJ4CWj6bkn.png)

- Dynamic Friction
	- 已在移动时使用的摩擦力。通常为0到1之间的值。值为零就像冰一样，值为1将使对象迅速静止（除非用很大的力或重力推动对象）
- Static Friction
	- 当对象静止在表面上时使用的摩擦力。通常为0到1之间的值。值为零就像冰一样，值为1将导致很难让对象移动。
- Bounciness
	- 表面的弹性如何？值为0将不会反弹。值为1将在反弹时不产生任何能量损失，预计会有一些近似值，但可能只会给模拟增加少量能量。
- Friction Combine
	- 两个碰撞对象的摩擦力的组合方式
	- Average 对两个摩擦值求平均值。
	- Minimum 使用两个值中的最小值
	- Maximum 使用两个值中的最大值
	- Multiply 两个摩擦值相乘。
- Bounce Combine
	- 两个碰撞对象的弹性的组合方式。其模式与Friction Combine模式相同

![物理材质参数说明.png](https://s2.loli.net/2024/11/26/bKnkrcMwSUFqVso.png)


# 碰撞检测之碰撞检测函数
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson16 : MonoBehaviour
{
    #region 知识点回顾
    //1.如何让两个游戏物体之间产生碰撞（至少1个刚体 和 两个碰撞器）
    //2.如何让两个物体之间碰撞时表现出不同效果（物理材质）
    //3.触发器的作用是什么(让两个物体碰撞没有物理效果，只进行碰撞处理)
    #endregion

    #region 注意：碰撞和触发响应函数 属于 特殊的生命周期函数 也是通过反射调用

    #endregion

    #region 知识点一 物理碰撞检测响应函数
    //碰撞触发接触时会 自动执行这个函数
    // Collision 碰撞相关
    private void OnCollisionEnter(Collision collision)
    {
        //Collision类型的 参数 包含了 碰到自己的对象的相关信息

        //关键参数
        //1.碰撞到的对象碰撞器的信息
        //collision.collider

        //2.碰撞对象的依附对象（GameObject）
        //collision.gameObject

        //3.碰撞对象的依附对象的位置信息
        //collision.transform

        //4.触碰点数相关
        //collision.contactCount
        //接触点 具体的坐标
        //ContactPoint[] pos = collision.contacts;

        //只要得到了 碰撞到的对象的 任意一个信息 就可以得到它的所有信息

        print(this.name + "被" + collision.gameObject.name + "撞到了");
    }

    //碰撞结束分离时  会自动执行的函数
    private void OnCollisionExit(Collision collision)
    {
        print(this.name + "被" + collision.gameObject.name + "结束碰撞了");
    }

    //两个物体相互接触摩擦时 会不停的调用该函数
    private void OnCollisionStay(Collision collision)
    {
        print(this.name + "一直在和" + collision.gameObject.name + "接触");
    }
    #endregion

    #region 知识点二 触发器检测响应函数
    // 需要将碰撞器上的IsTrigger勾上
    // Collider 碰撞相关，可以理解为是GameObject上的那个碰撞器的脚本

    //触发开始的函数 当第一次接触时 会自动调用
    protected virtual void OnTriggerEnter(Collider other)
    {
        print(this.name + "被" + other.gameObject.name + "触发了");
    }

    //触发结束的函数 当水乳相融的状态结束时 会调用一次
    private void OnTriggerExit(Collider other)
    {
        print(this.name + "被" + other.gameObject.name + "结束水乳相融的状态了");
    }

    //当两个对象 水乳相融的时候 会不停调用
    private void OnTriggerStay(Collider other)
    {
        print(this.name + "和" + other.gameObject.name + "正在水乳相融");
    }

    #endregion

    #region 知识点三 要明确什么时候会响应函数
    //1.只要挂载的对象能和别的物体产生碰撞或者触发，那么对应的这6个函数就能够被响应，有没有刚体脚本不重要，有碰撞器脚本就可以了（碰撞的条件）
    //2.6个函数不是说 我都得写 我们一般是根据需求来进行选择书写
    //3.如果是一个异形物体，刚体在父对象上，如果你想通过子对象上挂脚本检测碰撞是不行的，必须挂载到这个刚体父对象上才行
    //4.要明确物理碰撞和触发器响应的区别，触发器是有IsTrigger的设置的，碰撞就是碰撞的条件
    #endregion

    #region 知识点四 碰撞和触发器函数都可以写成虚函数 在子类去重写逻辑
    //一般会把想要重写的碰撞和触发函数写成保护类型的，没有必要写成public，因为不会自己手动调用，都是Unity
    //通过反射帮助我们自动调用的
    #endregion
}
```


![生命周期函数图.bmp](https://s2.loli.net/2024/11/26/91tdX5jECVMmWlb.png)

# 碰撞检测之刚体加力知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson17 : MonoBehaviour
{
    Rigidbody rigidBody;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 刚体自带添加力的方法
        //给刚体加力的目标就是
        //让其有一个速度 朝向某一个方向移动

        //1.首先应该获取刚体组件
        rigidBody = this.GetComponent<Rigidbody>();

        //2.添加力
        //相对世界坐标
        //世界坐标系 Z轴正方向加了一次瞬时的力
        //加力过后对象是否停止移动，是由阻力决定的
        //如果阻力为0，那给了一次力过后不会停止运动
        //rigidBody.AddForce(Vector3.forward * 10);
        //如果想要在 世界坐标系方法中 让对象 相对于自己的面朝向动
        //rigidBody.AddForce(this.transform.forward * 10);

        //相对本地坐标
        //rigidBody.AddRelativeForce(Vector3.forward * 10);


        //3.添加扭矩力，让其旋转
        //相对世界坐标
        //rigidBody.AddTorque(Vector3.up * 10);
        //相对本地坐标
        //rigidBody.AddRelativeTorque(Vector3.up * 10);

        //4.直接改变速度
        //速度方向是相对于世界坐标系的
        //rigidBody.velocity = Vector3.forward * 5;

        //5.模拟爆炸效果
        //模拟爆炸的力一定是所有希望产生爆炸效果影响的对象，否则只有加了爆炸脚本的才受到影响
        //都需要得到他们的刚体 来执行这个方法 才能都有效果
        // 第一个参数是力的大小
        // 第二个参数是爆炸中心
        // 第三个参数是爆炸半径
        //rigidBody.AddExplosionForce(100, Vector3.zero, 10);
        #endregion

        #region 知识点二 力的几种模式

        //第二个参数 力的模式 主要的作用 就是 计算方式不同而已
        //由于4中计算方式的不同 最终的移动速度就会不同
        //rigidBody.AddForce(Vector3.forward * 10, ForceMode.Acceleration);

        //动量定理
        //Ft = mv
        // v = Ft/m;
        //F:力
        //t：时间
        //m:质量
        //v:速度

        //1.Acceleration
        //给物体增加一个持续的加速度，忽略其质量
        //v = Ft/m
        //F:(0,0,10)
        //t:0.02s Fixed time
        //m:默认为1，就是忽略其质量的意思
        //v = 10*0.02/ 1 = 0.2m/s
        //每物理帧移动0.2m/s*0.02 = 0.004m

        //2.Force，最真实的情况
        //给物体添加一个持续的力，与物体的质量有关
        //v = Ft/m
        //F:(0,0,10)
        //t:0.02s
        //m:2kg
        //v = 10*0.02/ 2 = 0.1m/s
        //每物理帧移动0.1m/s*0.02 = 0.002m

        //3.Impulse
        //给物体添加一个瞬间的力，与物体的质量有关,忽略时间，默认为1
        //v = Ft/m
        //F:(0,0,10)
        //t:默认为1
        //m:2kg
        //v = 10*1/ 2 = 5m/s
        //每物理帧移动5m/s*0.02 = 0.1m

        //4.VelocityChange
        //给物体添加一个瞬时速度，忽略质量，忽略时间
        //v = Ft/m
        //F:(0,0,10)
        //t:默认为1
        //m:默认为1
        //v = 10*1/ 1 = 10m/s
        //每物理帧移动10m/s*0.02 = 0.2m
        #endregion

        #region 知识点三 力场脚本
        // Constant Force脚本
        // 脚本中可以设置，一直给予的持续力的大小，使用这个提供的脚本能够快速测试/实现其他功能
        #endregion
    }

    // Update is called once per frame
    void Update()
    {
        //如果你希望即使有阻力，也希望对象一直动，那你就一直“推”就行了
        //rigidBody.AddForce(Vector3.forward * 10);

        #region 补充 刚体的休眠
        //获取刚体是否处于休眠状态 如果是
        if (rigidBody.IsSleeping())
        {
            //就唤醒它
            rigidBody.WakeUp();
        }
        #endregion
    }
}
```


- 刚体的休眠，为了节约性能，刚体不进行计算了
![Pasted image 20241121234518.png](https://s2.loli.net/2024/11/26/XFodzIvPLRhOBt1.png)


# 音频文件导入知识点
- 这个是Unity中对mp3的一些设置
![Pasted image 20241122160356.png](https://s2.loli.net/2024/11/26/OlRqb9vkdpYj7mP.png)
- Force To Mono
	- 默认是多声道，勾选了这个选项就变成了单声道
- <font color="red">Load in Background</font>
	- 在后台加载，会新开一个线程加载音乐，如果是大音频用这个比较好，不会卡顿
- Ambisonic
	- 立体混响声，一般是VR才会选择这个选项，不然不会选择
- <font color="red">Load Type</font>
	- Decompress On Load
		- 不压缩形式存在内存，加载块，但是内存占用高
		- 适用于小音效
	- Compress in memory
		- 压缩形式存在内存，加载慢，内存小
		- 仅适用于较大音效文件
	- Streaming
		- 以流形式存在，使用时解码。内存占用最小，cpu消耗高
		- 性能换内存，一般内存特别紧张的时候才选这个
- <font color="red">Preload Audio Data</font>
	- 预加载音频，勾选后进入场景就加载，不勾选，第一次使用时才加载
	- 一般默认加载
- Compression Format
	- PCM 音频以最高质量存储
	- Vorbis 相对PCM压缩的更小，根据质量决定
	- ADPCM 包含噪音，会被多次播放的声音，如碰撞声
- Quality
	- 音频质量
	- 确定要应用于压缩剪辑的压缩量
	- 不适用于PCM/ADPCM/HEVAG格式
- Sample Rate Setting
	- PCM和ADPCM压缩格式允许自动优化或手动降低采样率
	- Preserve Sample Rate 此设置可保持采样率不变 (默认值)
	- Optimize Sample Rate  PCM和ADPCM压缩格式允许自动优化或手动降低采样率此设置根据分析的最高频率内容自动优化采样率
	- Override Sample Rate 此设置允许手动覆盖采样率，因此可有效地将其用于丢弃频率内容。



![音频导入参数1.png](https://s2.loli.net/2024/11/26/h4aLdzDNMCujQJr.png)

![音频导入参数2.png](https://s2.loli.net/2024/11/26/aTSijsxnQldHY8g.png)

# 音频源和音频监听脚本知识点

![Pasted image 20241122162845.png](https://s2.loli.net/2024/11/26/SJZbcMkeiP6tqLT.png)


- AudioClip
	- 想要关联的音频文件
- Output
	- 默认会输出到Main Camera的Audio Listener脚本上
- Mute
	- 静音开关，如果勾选，后台的音乐会被静音
- Play On Awake
	- 运行时就播放，一般除了背景音乐，其他都不勾选
- Loop
	- 循环播放
- Priority
	- 优先级越高，越不容易被别的取代
- Volume
	- 音量大小
- Stereo Pan
	- <0是左声道
	- >0是右声道
Spatial Blend 
- 默认是2D音效，远近声音一样大
- 3D音效是近大远小
![音效源参数1.png](https://s2.loli.net/2024/11/26/bXwzgS27YfP1l8R.png)
![音效源参数2.png](https://s2.loli.net/2024/11/26/kRAvE5gMCV8zQ4U.png)

# Audio Listener
> 场景中一定要存在Audio Listener才能够听到声音
> 有且只有一个Audio Listener，否则会报错

# 代码控制音频源
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson20 : MonoBehaviour
{
    AudioSource audioSource;

    public GameObject obj;

    public AudioClip clip;

    // Start is called before the first frame update
    void Start()
    {
        audioSource = this.GetComponent<AudioSource>();

        #region 知识点三 如何动态控制音效播放
        //1.直接在要播放音效的对象上挂载脚本 控制播放

        //2.实例化挂载了音效源脚本的对象，设置了Play on Awake
        //这种方法 其实用的比较少
        //Instantiate(obj);

        //3.用一个AudioSource来控制播放不同的音效
        //AudioSource aus = this.gameObject.AddComponent<AudioSource>();
        //aus.clip = clip;
        //aus.Play();

        //潜在知识点
        //一个GameObject可以挂载多个 音效源脚本AudioSource
        //使用时要注意 如果要挂载多个 那一定要自己管理他们 控制他们的播放 停止 不然 我们没有办法准确的获取
        //谁是谁

        #endregion
    }

    // Update is called once per frame
    void Update()
    {
        #region 知识点一 代码控制播放停止
        if (Input.GetKeyDown(KeyCode.P))
        {
            //播放音效
            audioSource.Play();
            //延迟播放 填写的是秒数
            //audioSource.PlayDelayed(5);
        }
        if (Input.GetKeyDown(KeyCode.S))
        {
            //停止音效
            audioSource.Stop();
        }
        if (Input.GetKeyDown(KeyCode.Space))
        {
            //暂停，一旦暂停了，只能用audioSource.Play()或者audioSource.UnPause()来播放
            audioSource.Pause();
        }

        if (Input.GetKeyDown(KeyCode.X))
        {
            //停止暂停 和暂停后 Play效果是一样的 都会继续播放现在的音效
            audioSource.UnPause();
        }
        #endregion

        #region 知识点二 如何检测音效播放完毕
        //如果你希望某一个音效播放完毕后 想要做什么事情
        //那就可以在Update生命周期函数中 不停的去检测 它的 该属性
        //如果是false就代表播放完毕了
        // AudioSource没有提供音效播放完毕的委托
        if (audioSource.isPlaying)
        {
            print("播放中");
        }
        else
        {
            print("播放结束");
        }
        #endregion
    }
}
```

# 麦克风输入相关的知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson21 : MonoBehaviour
{
    private AudioClip clip;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 获取设备麦克风信息
        string[] strs = Microphone.devices;
        for (int i = 0; i < strs.Length; i++)
        {
            print(strs[i]);
        }
        #endregion
    }

    // Update is called once per frame
    void Update()
    {
        #region 知识点二 开始录制
        //参数一：设备名 传空使用默认设备
        //参数二：超过录制长度后是否重头录制，填true会把之前的覆盖了
        //参数三：录制时长，单位是秒
        //参数四：采样率
        if (Input.GetKeyDown(KeyCode.Space))
        {
            // 44100 Hz 是一个标准的采样率，常用于音频录制和回放。
            clip = Microphone.Start(null, false, 10, 44100);
        }
        #endregion

        #region 知识点三 结束录制
        if (Input.GetKeyUp(KeyCode.Space))
        {
            // 结束录制
            Microphone.End(null);
            //第一次去获取没有才添加AudioSource脚本
            AudioSource s = this.GetComponent<AudioSource>();
            if (s == null)
                s = this.gameObject.AddComponent<AudioSource>();
            s.clip = clip;
            s.Play();

            #region 知识点四 获取音频数据用于存储或者传输
            //规则 用于存储数组数据的长度 是用 声道数 * 剪辑长度
            float[] f = new float[clip.channels * clip.samples];
            // 第一个参数是将录制的音频数据存在float数组中
            // 第二个参数是偏移位置
            clip.GetData(f, 0);
            print(f.Length); //44100 44100 Hz 是一个标准的采样率，常用于音频录制和回放。
            #endregion
        }
        #endregion
    }
}
```



# 参考文献
[游习堂](https://www.yxtown.com/)
