---
title: Unity知识整理(三_UGUI)
math: true
---

> NGUI全称下一代用户界面(Next-Gen UI)
> 它是第三方提供的Unity付费插件
> 专门用于制作Unity中游戏UI的第三方工具
> 相对于GUI它更适用于制作游戏UI功能
> 更方便使用，性能和效率更高
> Unity插件：是一种基于Unity规范编写出来的程序，主要用于拓展功能，简单理解就是别人基于Unity写好的某种功能代码，我们可以直接用来处理特定的游戏逻辑


# UGUI导入

- 打开资源商店
![Pasted image 20241126164355.png](https://s2.loli.net/2024/12/01/VjU51TOEKnqoGNH.png)

- 搜索NGUI
![Pasted image 20241126164726.png](https://s2.loli.net/2024/12/01/QLpyRGWAPH9FKjz.png)
- 找到这个NGUI
![Pasted image 20241126164740.png](https://s2.loli.net/2024/12/01/qiJn3wsXbrON6AS.png)
- UI创建
![Pasted image 20241126164950.png](https://s2.loli.net/2024/12/01/mvx8n3hNIUFWfrB.png)

- 另一种方式创建UI
![Pasted image 20241126165232.png](https://s2.loli.net/2024/12/01/ZFAp6CjQ8rxbygt.png)



# ROOT组件
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson1_Root : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 必备知识——分辨率概念
        //1.分辨率
        //屏幕宽高两个方向的像素点
        //比如1920 * 1080
        //宽1920个像素
        //高1080个像素

        //2.像素
        //像素
        //像素即px
        //是画面中最小的点(单位色块)

        //3.屏幕尺寸
        //屏幕对角线长度

        //4.屏幕比例
        //PC显示器
        //1920:1080 = 16:9

        //苹果手机
        //iPhone7,8：1334 * 750 = 16:9
        //iPhone 7,8 Plus：1920 * 1080 = 16:9
        //iPhoneX：2436 * 1125 = 19.5:9
        //iPhone12: 2532 * 1170 = 19.5:9

        //目前市面上设备分辨率比例传统的有：
        //4:3(ipad)
        //16:10
        //16:9(老手机 、电脑显示器)
        //18:9（去掉留海屏幕）
        //19.5:9（ 新款手机）
        //19.9:9

        //5.dpi
        //像素密度
        //单位面积上有多少个像素点
        //一般指一英寸有多少个像素点
        #endregion

        #region 知识点二 Root是用来干啥的
        //Root是用于分辨率自适应的根对象
        //可以设置基本分辨率,相当于设置UI显示区域
        //并且管理所有UI控件的分辨率自适应

        //可以简单理解 它管理一个 UI画布 所有的UI都是显示在这个画布上的
        //它会管理 UI画布 和 不同屏幕分辨率的 适应关系
        #endregion

        #region 知识点三 Root相关参数

        #endregion

        #region 总结
        //1.Flexible 适用于可以手动拖窗口改变分辨率的设备 比如pc端

        //2.Constrained 适用于移动设备
        //  因为移动设备都是全屏应用 不会频繁改变分辨率 只用适配不同分辨率的设备
        //  横屏勾选 高 fit  竖屏 勾选 宽 fit 一般就可以比较好的进行分辨率适应了
        //  需要注意的是 背景图 一定要考虑 极限 宽高比来出 最大宽高比  19.9:9

        //3.Constrained On Mobiles 是上面两者的综合体 适用于多平台发布的游戏和应用
        #endregion
    }
}
```


- 创建的2D UI
- UI ROOT上有Root脚本和Panel脚本
- Camera上有Event Systeam(UI Camera)脚本
![Pasted image 20241126170036.png](https://s2.loli.net/2024/12/01/AI8OdcPezFvTVMN.png)
![Pasted image 20241126170217.png](https://s2.loli.net/2024/12/01/6V9MuAfjZCNo5cL.png)

- Flexible 适用于PC端
	- 灵活模式
		- 在该模式下，U都是以像素为基础，100像素的物体无论在多少分辨率上都是100像素
		- 这就意味着，100像素在分辨率低的屏幕上可能显示正常，但是在高分辨率上就会显得很小
		- Minimum Height 屏幕高小于该值时开始按比例缩放
		- Maximum Height 屏幕高大于改值时开始按比例缩放
		- Shrink Portrait UI  勾选时，竖屏时，按宽度来适配，就是上面的Min Height和Max Height改成了按照宽度适配
		- Adjust by DPI 使用dpi做适配计算建议勾选
- Constrained 适用于手机端 
	- 约束模式
	- 该模式下，屏幕按尺寸比例来适配，不管实际屏幕有多大，NGUI都会通过合适的缩放来适配屏幕。
	- 这样在高分辨率上显示的UI就会被放大保持原有大小，但有可能会模糊，好处是各设备看到的UI和屏幕比例是一样的
	- Content Width
		- 按照该宽度值适配屏幕
		- 制作资源时的默认分辨率宽
	- Content Heigh
		- 按照该高度值适配屏幕
		- 制作资源时的默认分辨率高
	- Fit表示以那个值做适配
		- 勾选Width 屏幕比例变化时，按照宽度来适配 (宽度始终不变)
			- 竖屏游戏
		- 勾选Height 屏幕比例变化时，按照高度来适配 (高度始终不变)
			- 横屏游戏
		- 两个都勾选 
			- 不会被裁剪，但是有黑边
			- 意思就是会把实际制作的图完全显示
			- 当适配宽高比大于实际宽高比时，就会按照宽度适配，反之按照高度适配
		- 两个都不勾选
			- 始终保证屏幕被UI填充满
			- 不会有黑边
			- 可能会被裁剪
- Constrained On Mobiles
	- 上面两种模式的综合体
	- 在PC和Mac等桌面设备上用Flexible模式
	- 在移动设备上用Constrained模式

- 选中UI Root，创建Sprite
![Pasted image 20241126170927.png](https://s2.loli.net/2024/12/01/J8eKsuX3qgM5nBP.png)

![Pasted image 20241126171119.png](https://s2.loli.net/2024/12/01/ui4xV1S9X23AtNd.png)

- 红色的就是屏幕大小
- 蓝色的是实际图片大小
![Pasted image 20241126173125.png](https://s2.loli.net/2024/12/01/97KiC1UoGWgda3z.png)

![缩放模式设置.png](https://s2.loli.net/2024/12/01/Lhse8CGqUz5j7Iv.png)

# Panel

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson1_Panel : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Panel用来干啥
        //1.管理一个UI面板的渲染顺序
        //2.管理一个UI面板上的所有子控件
        // 如果某个Sprite对象的父对象上没有Panel脚本，则Sprite精灵图则无法被渲染
        // 所以必须要有一个Panel脚本，默认UI Root上有一个Panel脚本
        #endregion

        #region 知识点二 Panel参数相关

        #endregion

        #region 总结
        //1.没有Panel父对象 UI控件看不到
        //2.Panel一般用于管理面板 控制层级
        //3.Panel可以有多个 一般一个Panel管理一个面板
        #endregion
    }
}
```

- Alpha
	- 控制所有子UI元素的透明度
- Depth
	- 控制该Panel的层级
	- 层级高的后渲染会把层级低的先渲染的遮挡住
- Clipping 裁剪
	- None 不处理
		- 正常
	- Texture Mask 根据图片信息进行遮罩
		- 比如人物头像
	- Soft Clip 自己定范围裁剪 
		- 比如拖动框
	- Constrain But Dont Clip 约束但不剪裁
		- 不裁剪画面
		- 只限制响应范围
	- Sorting Layer 排序层

![重要参数.png](https://s2.loli.net/2024/12/01/OchYZAvETXzbiNr.png)

- Advanced Options 渲染相关高级选项
	- Render Q 渲染队列
	- Sort Order 排序
	- Normals 是否需要灯光着色器
	- UV2 是否用于自定义着色器效果
	- Shadow Mode 阴影模式
		- 3DUI得时候用，2DUI不用
	- Cull 元素组件拖动时提出
	- Visible 检查元素组件是否离开屏幕
	- Padding 边界内容
	- Offset 抵消偏移位置
	- Static 检查子元素是否会移动
	- Panel Tool 是否显示面板工具


- Anchors  锚点设置
	- 用于分辨率自适应设置大小



![次要参数.png](https://s2.loli.net/2024/12/01/MQI9gUeFqYXTrLk.png)


# EventSystem组件（UIGamera）

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson1_EventSystem : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 EventSystem是用来干啥的
        //主要作用是让摄像机渲染出来的物体
        //能够接收到NGUI的输入事件
        //大部分设置不需要我们去修改

        //有了它我们通过鼠标 触碰 键盘 控制器 操作UI 响应玩家的输入
        #endregion

        #region 知识点二 相关参数

        #endregion

        #region 总结
        //1.EventSystem很重要，如果没有它，我们没有办法监听玩家输入
        //2.创建UI时的 2DUI 和3DUI 主要就是摄像机的模式不一样
        //  EventSystem的2D和3D主要是 采用2D碰撞器 还是3D碰撞器 不能直接改变摄像机模式
        #endregion
    }
}
```

- Event Type 事件类型
	- 决定了脚本如何对鼠标和触屏事件进行响应
	- UI模式，那么他们处理事件的方式是根据组件的深度,UI组件里面有脚本Sprite-Widget-Depth，这个Depth越大，表示深度越高，点击得时候处理Depth大的UI的事件
	- world模式，那么则会根据距离离主摄像机的远近来进行响应排序，处理离摄像机近的的UI
	- 2D和3D的区别是，碰撞器是用3D碰撞器还是2D碰撞器2DUI
- Events go to
	- 事件通过刚体还是碰撞盒传递
- Process Events In
	- 事件更新进度在Update中还是LateUpdate中
	- 一般不改，默认在Update中
- Event Mask
	- 决定哪个游戏对象层级将会接受事件
- Debug
	- 是否开启调试模式
	- 如果开启，可以帮助你在点击时
	- 判断当前和鼠标事件交互的是什么对象
	- 能在Scene窗口看到信息
- Command Click
	- 苹果电脑上是否用Command按键模拟右键操作
- Allow Multi Touch
	- 是否支持多点触碰
- Auto Hide Cursor
	- 当游戏有控制器或者其他输入设备时
	- 是否自动隐藏光标
- Sticky Tooltip
	- 是否使用tooltip
- Long Press Tooltip
	- 是否长按出提示
- Tooltip Delay
	- 停留多久出现tip
- Raycast Range
	- 射线长度，一般不修改
- EventSources 接收的事件来源
	- Mouse 鼠标
	- Touch 触摸
	- Keyboard 键盘
	- Controller 控制器
- Thresholds 调整鼠标事件的点击、拖、轻拍等行为
	- Mouse Drag
	- Mouse Click
	- Touch Drag
	- Touch Tap
- Axes and keys
	- 热键关系
	- 一般不修改

![Pasted image 20241127105755.png](https://s2.loli.net/2024/12/01/dkX2Twsb7pKyWEJ.png)

![Pasted image 20241127110751.png](https://s2.loli.net/2024/12/01/u6xYagU1Avc5psD.png)
![参数1.png](https://s2.loli.net/2024/12/01/MRFHAI1JPoBUuDE.png)

![参数2.png](https://s2.loli.net/2024/12/01/zO1489ZoIsXrmVN.png)
# 图集制作

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson2 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 图集用来干啥
        //NGUI中的最小图片控件Sprite要使用图集中的图片进行显示
        //图集 就是把很多单独的小图 合并为 一张大图 合并后的大图就是图集
        //目的：提高渲染性能
        #endregion

        #region 知识点二 打开图集制作工具
        //方法一：Project右键打开
        //方法二：上方工具栏NGUI——Open——Atlas Maker
        #endregion

        #region 知识点三 新建图集
        //在图集工具中创建

        //图集关键文件有3
        //1.图集文件
        //2.图集材质
        //3.图集图片
        #endregion

        #region 知识点四 修改删除图集元素
        //在图集工具中操作
        //增删改
        #endregion
    }
}
```


![Pasted image 20241127114002.png](https://s2.loli.net/2024/12/01/BgzUijdC2kH5cSA.png)
![Pasted image 20241127114054.png](https://s2.loli.net/2024/12/01/IjToOCir9YQ31VN.png)
![Pasted image 20241127114020.png](https://s2.loli.net/2024/12/01/3KRMLVtnUJFdefl.png)
- 创建图集
![Pasted image 20241127114306.png](https://s2.loli.net/2024/12/01/VeJp4mKQjSOIzwZ.png)

![Pasted image 20241127114549.png](https://s2.loli.net/2024/12/01/35vFa1Q4YcewAUL.png)

- 可以指定当前编辑的是哪个图集
![Pasted image 20241127153648.png](https://s2.loli.net/2024/12/01/8pxksJZHGwiVWOC.png)
- Material 可以快速选中当前图集对应的材质球
- Texture 可以快速选中当前图集对应的图片
![Pasted image 20241127153810.png](https://s2.loli.net/2024/12/01/EDepYzNaHr92k85.png)

- Padding 图片间像素间隔
	- 也就是图集中图片之间的间隔
- Trim Alpha 移除图片多余空白空间
	- 如果一个图片是png，有透明背景，会将透明背景剔除，不管透明背景
- PMA Shader 预乘透明通道
- Unity Packer 自定义打包器
- Truecolor 强制ARGB32纹理
	- A 透明度
	- R 红色
	- G 绿色
	- B 蓝色
- Auto-upgrade 自动更新，用精灵替换纹理
- Force Square 如果启用，将强制方形图集纹理长宽都为2的n次方
	- 也就是图集的大小，为方形
	- 并且像素值为2的n次方
- Pre-processor 预处理器

- View Sprites
	- 更详细的查看图集中一个个的图片
![Pasted image 20241127155932.png](https://s2.loli.net/2024/12/01/demlM3NwcTiGYqQ.png)
- 选中图片的时候会出现下面的情况
- Update 表示图片名称一样，进行更新
- Add 新图片，新添加
- 上方的Add /Update就可以重新更新选中的图集了
- 添加完之后会有x，如上一张图片，就可以删除图集中添加的图片了
![Pasted image 20241127160116.png](https://s2.loli.net/2024/12/01/UiDxuLdM7beIJSK.png)

![图集工具参数.png](https://s2.loli.net/2024/12/01/aiFM7G26XIWsbK3.png)

# Sprite精灵图片

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson3 : MonoBehaviour
{

	// NGUI需要用UISprite
    public UISprite sprite;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Sprite用来做啥
        //NGUI中所有中小尺寸图片显示都用Sprite显示
        //使用它来显示图集中的单个图片资源
        #endregion

        #region 知识点二 创建Sprite

        #endregion

        #region 知识点三 Sprite参数

        #endregion

        #region 知识点四 代码设置图片
        //sprite.width = 200;
        //sprite.height = 300;
        //1.改变为当前图集中选择的图片
		// 在同一个图集中，直接改成对应图片的名字就可以了
        sprite.spriteName = "bk";

        //2.改变为其它图集中的图片
        //先加载图集，NGUIAtlas表示是图集，否则可能加载错误，因为还有其他同名的材质球和图片
        NGUIAtlas atlas = Resources.Load<NGUIAtlas>("Atlas/login");
        sprite.atlas = atlas;
        //再设置图片
        sprite.spriteName = "ui_DL_anniuxiao_01";
        #endregion
    }
}
```

- 创建Sprite
![Pasted image 20241127165355.png](https://s2.loli.net/2024/12/01/hoCbcT6sDMut318.png)

- Atlas 选择使用的图集
- Sprite 选择使用图集中的精灵图片
- Material 材质，一般不修改
- Fixed Aspect  固定宽高比
	- 不管如何变化尺寸
	- 适中保持宽高比
- Type 图片类型
	- Simple 普通模式
		- 缩放会拉伸
	- Sliced 切片模式
		- 可九宫格缩放
	- Tiled 平铺模式
		- 图片重复绘制
		- Horizontal 水平
		- Vertical 竖直
		- Radial 90 90度
		- Radial 180 180度
		- Radial 360 360度
	- Filed 填充模式
		- 可以做CD，进度条等。CD的意思就是技能那种时钟类型的等待
	- Advanced 高级模式
		- 可以把图片分成5个部分分别设置模式
- Filp 翻转模式
	- Nothing 不翻转
	- Horizontally 水平翻转
	- Vertically 竖直翻转
	- Both 水平竖直都翻转
- Gradient 渐变色
	- 勾选后上部颜色和下部颜色自动渐变
- Color Tint
	- 颜色叠加
- Pivot
	- 控制对象中心点，决定用于对齐的中心点
	- Pivot左中右、上中下，每个里面选择一个
	- 对应的就是下面第二张图片的九个位置
	- ![Pasted image 20241128215009.png](https://s2.loli.net/2024/12/01/41ECxoSjdsvpb6Z.png)
	- ![Pasted image 20241128215030.png](https://s2.loli.net/2024/12/01/KUM5lWtYOuh3wnG.png)
- Depth 深度层级
	- 用于决定渲染顺序，不过是用在同一个Panel中，表示的是控件之间的渲染顺序，Panel中有自己单独的Depth
	- 值越大越后渲染，显示在前方，深度值的意思是渲染的顺序，值越小，渲染的顺序越靠前，所以值越大，就能够覆盖之前渲染的
- Size 尺寸
	- Snap 原始尺寸显示，也就是图片原始是98x98的大小，把图片也调整为这么大显示在Game/Scene中
- Aspect 宽高比
	- Free 随意缩放
	- Based On Width  基于宽等比变化
	- Based On Height 基于高等比变化


- Sliced模式
![Pasted image 20241127170518.png](https://s2.loli.net/2024/12/01/yjZnpKwtbmfrxuH.png)
- 下面两张图拉伸后的实例
- 设置Sprite Details 中的Border参数的效果
- 实际上就是这四个角不回被拉伸
![Pasted image 20241127170642.png](https://s2.loli.net/2024/12/01/QRKkvdcHTgqb4ta.png)


![Pasted image 20241127170717.png](https://s2.loli.net/2024/12/01/6J1kqtMfHB74lTR.png)


![Pasted image 20241127170835.png](https://s2.loli.net/2024/12/01/mz6o4XKYF1GwnMP.png)
- Tiled 平铺模式
![Pasted image 20241127171039.png](https://s2.loli.net/2024/12/01/pZXcoL6NSKy1mli.png)

- Filed
	- Horizontal
![Pasted image 20241127171239.png](https://s2.loli.net/2024/12/01/XKNvHecLsA65m98.png)
- Vertical
![Pasted image 20241127171301.png](https://s2.loli.net/2024/12/01/VrPFJvpybigO4R2.png)
- Radial 90
![Pasted image 20241127171317.png](https://s2.loli.net/2024/12/01/vYzILJPkxo98HfK.png)

- Radial 180
![Pasted image 20241127171332.png](https://s2.loli.net/2024/12/01/mCZXMDfYKGUd9tN.png)

- Radial 360
![Pasted image 20241127171406.png](https://s2.loli.net/2024/12/01/YNVM5bEsdXFouU4.png)


![Sprite参数1.png](https://s2.loli.net/2024/12/01/EvZ5sDy4jUROJAd.png)

![Sprite参数2.png](https://s2.loli.net/2024/12/01/A47BGrZ6bMF9Hix.png)

![Sprite参数3.png](https://s2.loli.net/2024/12/01/yXLRjnmbsVodNZ2.png)

# Label 文本控件知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson4 : MonoBehaviour
{
    public UILabel label;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Label用来干啥
        //NGUI中所有文本显示都使用Label来显示
        #endregion

        #region 知识点二 创建Label

        #endregion

        #region 知识点三 Label参数

        #endregion

        #region 知识点四 富文本

        #endregion

        #region 知识点五 代码设置Label
        label.text = "123123123123";
        #endregion
    }
}
```

- Font
	- 使用Unity字体可以使用fft的字体文件（`C:\Windows\Fonts`）,可以拖到资源中，然后拖到Label脚本中
	- 也可以使用NGUI字体图集（之后专门讲解）
- Material
	- 材质，一般不会修改
- FontSize 字体大小
	- 这个如果Label很小的话，可能显示的大小比设置的要小，需要自己调整文字框的大小。（默认Modifier 为 Shrink Content模式）
	- Normal 普通 
	- Bold 加粗
	- Italic 斜体
	- Bold And Italic 加粗并斜体
- Text 显示内容
- Modifier 修饰方式，引强制大小写字母
	- None 没有限制
	- To Uppercase 大写字母
	- To Lowercase 小写字母
	- Custom 自定义
- Overflow 
	- Shrink Content 收缩内容
		- 文字显示的大小与Label的大小自动适应
		- Label控件尺寸不足显示较大字体时
		- 字体的尺寸会自动适应Label控件的尺寸
	- Clamp Content 夹紧内容
		- 裁减掉无法显示的字体内容
		- 字体会按照FontSize大小来显示
		- 如果Label控件大小不足以装下字体时
		- 对应内容被裁剪掉不会显示
	- Resize Freely 自动调整大小
		- Labe大小去适应字体内容的大小
	- Resize Height 自动调整高
		- Label控件高度无法手动调节
		- 会随着字体内容的大小需求去适配字体内容
		- 只能手动调节Label控件的宽度
- Alignment 对齐方式
	- Automatic 自动对齐
	- Left 居左
	- Center 中心
	- Right 居右
	- Justified 调整会自动变化
- Keep crispp 动态字体锐化
	- 这是一个动态字体功能，可以确保文本在收缩时清晰
	- Never Never
	- OnDesktop和Always区别不大都会开启
- Gradient  渐变
- Effect 特效
	- None 没有
	- Shadow 阴影效果
	- OutLine和Outline8 边缘线，计算参数不同8粗一点
- Float spacing 开启浮点数间隔调整
	- 默认整形，不勾选
- Spacing 字符间隔
- Max Lines 最大行数
	- 默认0，表示可以有很多行，不限制
- BBCode 是否开启富文本
- Color Tint 字体颜色
- 富文本
	- $[ff0000]颜色[-]$
	- $[b]加粗[/b]$
	- $[i]斜体[/i]$
	- $[u]下划线[/u]$
	- $[s]直线穿过文字[/s]$
	- $[sub]右下角显示[/sub]$
	- $[sup]右上角显示[/sup]$
	- $[url=http://www.baidu.com/][u]超链接[/u]/url]$
		- 希望点击超链接能打开页面
		- 1. 添加碰撞器
			- ![Pasted image 20241128230831.png](https://s2.loli.net/2024/12/01/QvSOui9PwjGz6Zs.png)
		- 2. 添加脚本OpenURLOnClick

- 创建Label
![Pasted image 20241128224814.png](https://s2.loli.net/2024/12/01/5UV12zoAZu8S9K4.png)
![Pasted image 20241128224830.png](https://s2.loli.net/2024/12/01/svQRYon7XyTDk18.png)

![Pasted image 20241128225252.png](https://s2.loli.net/2024/12/01/Wbo2SAkvpNLt7UH.png)

![label_parameters1.png](https://s2.loli.net/2024/12/01/rWnpoIhLG1yc2ax.png)
![label_parameters2.png](https://s2.loli.net/2024/12/01/JMIHdwzheRtlrEa.png)
![rich_text.png](https://s2.loli.net/2024/12/01/g9I6iR5OqdUeQHL.png)
# Texture大图控件

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson5 : MonoBehaviour
{
    public UITexture tex;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Texture用来干啥
        //Sprite只能显示图集中图片 一般用于显示中小图片
        //如果使用大尺寸图片 没有必要打图集
        //直接使用Texture组件进行大图片显示
        #endregion

        #region 知识点二 参数相关

        #endregion

        #region 知识点三 代码设置
        //加载图片
        Texture texture = Resources.Load<Texture>("BK");
        //改变图片
        if (texture != null)
            tex.mainTexture = texture;
        #endregion
    }
}
```

- Texture 图片资源
- Material 材质 一般不改
- Shader 着色器 一般不改
- Type 图片类型 
	- Simple 普通模式，缩放会拉伸
	- Sliced 切片模式，可九宫格缩放
	- Tiled 平铺模式
		- 图片重复绘制
	- Filed 填充模式
		- 可以做CD，进度条等
	- Advanced 高级模式
		- 可以把图片分成5个部分分别设置模式
- Flip 翻转模式
	- Nothing
	- Horizontally 水平翻转
	- Vertically 竖直翻转
	- Both 水平竖直都翻转
- Gradient 渐变色
	- 勾选后，上部颜色和下部颜色，自动渐变
- Color Tint 颜色叠加


- 创建Texture
	- ![Pasted image 20241128231430.png](https://s2.loli.net/2024/12/01/XEqUhoOpjVAareb.png)
	- 


![Texture_parameters.png](https://s2.loli.net/2024/12/01/AFK1lW3GEs5yw6R.png)

# Button按钮组件知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson6 : MonoBehaviour
{
    public UIButton btn;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 所有组合控件的共同特点
        //1.在3个基础组件对象上添加对应组件
        //2.如果希望响应点击等事件 需要添加碰撞器
        #endregion

        #region 知识点二 Button是用来干嘛的
        //UI界面中的按钮 当点击按钮后我们可以进行一些处理
        #endregion

        #region 知识点三 制作Button
        //1.一个Sprite（需要文字再加一个Label子对象）
        //2.为Sprite添加Button脚本
        //3.添加碰撞器
        #endregion

        #region 知识点四 参数相关

        #endregion

        #region 知识点五 监听事件的两种方式
        //1.拖脚本
        //2.代码获取按钮对象监听
        // 这个EventDelegate是无参数，无返回值的
        // 如果想要传入参数，可以封装一层
        // 好像可以传入一个MonoBehaviour子类的类，然后写方法名
        btn.onClick.Add(new EventDelegate(ClickDo2));

        btn.onClick.Add(
            new EventDelegate(() =>
            {
                print("那么大表达式添加的 点击事件处理");
            })
        );

        btn.onClick.Add(new EventDelegate(() => ClickDo2("Hello, World!")));
        #endregion
    }

    public void ClickDoSomthing()
    {
        print("按钮点击");
    }

    public void ClickDo2()
    {
        print("按钮点击2");
    }

    void ClickDo2(string message)
    {
        Debug.Log(message);
    }

    #region 总结
    //1.button的制作流程
    //  3个基础组件构成 任意一个基础组件 往上面添加Button脚本 再添加碰撞器 就可以让它变成一个按钮
    //2.事件的监听
    // 通过 拖曳 或者 代码的形式 可以进行按钮的 点击事件 监听
    #endregion
}
```

- 制作Button 步骤
	- 1.一个Sprite（需要文字再加一个Label子对象）
		- ![Pasted image 20241128234834.png](https://s2.loli.net/2024/12/01/bMndSasTrOvI9fF.png)
	- 2.为Sprite添加Button脚本
		- ![Pasted image 20241128235004.png](https://s2.loli.net/2024/12/01/aIhb7qAlFiOZMQ2.png)
		- ![Pasted image 20241128235029.png](https://s2.loli.net/2024/12/01/U9rpyc1Jzihld3C.png)
	- 3.添加碰撞器
		- ![Pasted image 20241128235051.png](https://s2.loli.net/2024/12/01/qBSMYVOpAP1DIZ5.png)
- Twen Target 按钮控制的目标
	- 一般是3个基础控件之一
	- 会自动设置
- Drag Over
	- 拖动结束后做什么事件
- Transition 过渡效果持续时间
- Colors 按钮各状态颜色设置
	- Normal 普通
	- Hover 经过
	- Pressed 按下
	- Disabled 失活
	- Default 默认
- Sprites 按钮各状态图片设置
	- Normal 普通
	- Hover 经过
	- Pressed 按下
	- Disabled 失活
	- Pixel Snap 自动设置原始大小
		- 勾选后，选择的图片是什么大小，按钮使用时就显示什么样大小的图片
- OnCIick 点击按钮响应脚本
	- ![Pasted image 20241129001126.png](https://s2.loli.net/2024/12/01/r1lnGX7fDP98JUI.png)
![Button_parameters.png](https://s2.loli.net/2024/12/01/16Cs5EqwgKjHf3b.png)

# Toggle单选多选框组件知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson7 : MonoBehaviour
{
    public UIToggle tog1;
    public UIToggle tog2;
    public UIToggle tog3;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Toggle用来干啥
        //单选框 多选框都可以使用它来制作
        #endregion

        #region 知识点二 制作Toggle
        //1.2个Sprite 1父1子
        //2.为父对象添加Toggle脚本
        //3.添加碰撞器
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖代码
        //2.代码进行监听添加

        tog1.onChange.Add(new EventDelegate(Change2));
        tog2.onChange.Add(new EventDelegate(Change2));
        tog3.onChange.Add(new EventDelegate(Change2));
        #endregion
    }

    private void Change2()
    {
        print("代码监听");
    }

    public void Change()
    {
        print("Toggle变化执行的内容");

        if (tog1.value)
        {
            print("tog1选中");
        }
        else if (tog2.value)
        {
            print("tog2选中");
        }
        else if (tog3.value)
        {
            print("tog3选中");
        }
    }
}
```

- Group
	- 多选框分组
	- 多个多选框分为一组则变为单选框
	- StateofNone 单选框状态时是否允许不选中
- Starting State 开始默认状态，勾选Starting State为选中
- Sprite 选中用图片
- Invert State 反转状态
	- 如果选中不显示，不选中显示，就勾选它
- Animator 状态变化时播放动画（新动画系统）
- Animation 状态变化时播放动画（老动画系统）
- Tween 状态变化时缓动
- Transition 过渡模式
- OnValueChange 状态变化时响应脚本


- 2个Sprite 1父1子
	- ![Pasted image 20241129104052.png](https://s2.loli.net/2024/12/01/SoKmBIxYiyCWGML.png)
-  为父对象添加Toggle脚本
	- ![Pasted image 20241129104013.png](https://s2.loli.net/2024/12/01/SAwfZJTmuxatOQo.png)
-  添加碰撞器
	- ![Pasted image 20241129104210.png](https://s2.loli.net/2024/12/01/segwvyktdCrVF7f.png)
- 需要关联的这个对象拖到Toggle上
- 此时运行Unity，点击Game窗口就可以出现单选框的现象了
![Pasted image 20241129104426.png](https://s2.loli.net/2024/12/01/c5K7fQp1EW3NYew.png)
- 如果是多选框，多个是互斥的，则把Toggle脚本上的Group变成一样的数字
- 数字0表示不分组，就是多选框
- State of None表示勾选后是否可以多个都不选，否则至少选择一个
![Pasted image 20241129105459.png](https://s2.loli.net/2024/12/01/G4BOAcM7jDlf596.png)
- 当指定多选框状态变化的时候，调用指定的方法
![Pasted image 20241129110052.png](https://s2.loli.net/2024/12/01/RUm6PeXhWVg2sB9.png)

![Toggle_parameters.png](https://s2.loli.net/2024/12/01/ThvaPIXCuSYz5pB.png)



# Input输入框组件知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson8 : MonoBehaviour
{
    public UIInput input;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Input是啥
        //输入框
        //可以用来制作账号密码聊天输入框
        #endregion

        #region 知识点二 制作Input
        //1.1个Sprite做背景 1个Label显示文字
        //2.为Sprint添加Input脚本
        //3.添加碰撞器
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖曳脚本
        //2.通过代码关联

        input.onSubmit.Add(new EventDelegate(() =>
        {
            print("完成输入 通关代码添加的监听函数");
        }));

        input.onChange.Add(new EventDelegate(() =>
        {
            print("输入变化 通关代码添加的监听函数");
        }));
        #endregion
    }

    public void OnSubmit()
    {
        print("输入完成" + input.value);
    }

    public void OnChange()
    {
        print("输入变化" + input.value);
    }  
}
```

- 1个Sprite做背景 1个Label显示文字
- 为Sprint添加Input脚本
	- ![Pasted image 20241129113650.png](https://s2.loli.net/2024/12/01/AgQnNE1rToZx3DG.png)
- 添加碰撞器
	-![Pasted image 20241129113912.png](https://s2.loli.net/2024/12/01/hSQVE2va8xOwdpW.png)

- Label  关联的文本组件
	- 需要将Label拖给脚本的这个参数
	- ![Pasted image 20241129114454.png](https://s2.loli.net/2024/12/01/LanmfdQZUqAyu9W.png)
- Starting Value 开始默认显示的内容
- Saved As  
	- 若此处填写内容，会使用PlayerPrefs将输入内容作为此处填写的key，实际的内容作为值进行存储
	- 一般不使用
- ActiveTextColor 选中激活时颜色
- Inactive Color 未选中失活时颜色 
- Caret Color 插入光标的颜色
- Selection Color 选中文字的背景颜色
- InputType 输入类型
	- Standard 默认模式
	- Auto Correct 自动更正
	- Password 密码输入 (输入内容看不到）
- Validation 输入限制
	- None：无限制
	- Integer：只能输入整形
	- Float：可以输入浮点数
	- Alphanumeric：只能是数字和字母
	- Username：用户名，只能输入小写字母
	- Name：姓名，大小写字母+空格
	- Filename：文件名，只是多了一些特殊字符的输入
- Mobile Keyboard 手机键盘模式
	- Default 
	- ASCII Capable 英文
	- Numbers And Punctuation 数字符号
	- URL 链接
	- Number Pad 数字
	- Phone Pad 手机
	- Name Phone Pad 名字
	- Email Address 邮箱地址
- Hide Input 键盘下隐藏输入框
	- 输入的内容可以在键盘上看见，如果勾选，只能通过游戏界面看见
	- 不勾选，键盘会自带输入内容的文本框
- On Return Key 完成键（回车键）做什么操作
	- Default 默认操作，根据系统的默认情况指定
	- Submit 结束输入，回车键表示结束输入
		- 一般选择这个
		- 如果想要调用OnSubmit方法，必须选择这个，不然可能无反应
	- New Line 换行
- Character Limit 最大可输入字符数
	- 0表示无限制
- OnSubmit 输入完成时响应脚本
	- 需要On Return Key中设置Submit
- OnChange 输入变化时响应脚本




![Input_parameters1.png](https://s2.loli.net/2024/12/01/vHyzYkO1Z4mX3N7.png)
![Input_parameter2.png](https://s2.loli.net/2024/12/01/KDm3ybs9WOxRLoA.png)


# PopupList下拉列表相关知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson9 : MonoBehaviour
{
    public UIPopupList list;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 PopupList是啥？
        //下拉列表
        #endregion

        #region 知识点二 制作Popuplist
        //1.一个sprite做背景 一个lable做显示内容
        //2.添加PopupList脚本
        //3.添加碰撞器
        //4.关联lable做信息更新，选择Label中的SetCurrentSelection函数
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖曳代码
        //2.代码关联
        // 代码添加选项
        list.items.Add("新加 选项4");

        // 代码添加监听事件
        list.onChange.Add(
            new EventDelegate(() =>
            {
                print("代码添加的监听" + list.value);
            })
        );
        #endregion
    }

    public void OnChange()
    {
        print("选项变化" + list.value);
    }
}
```
- 一个sprite做背景 一个label做显示内容
- 添加PopupList脚本
	- ![Pasted image 20241129164013.png](https://s2.loli.net/2024/12/01/pmWdnZ1JQNIrMO5.png)
- 添加碰撞器
	- ![Pasted image 20241129164034.png](https://s2.loli.net/2024/12/01/UYS1FhQdBVxIe3K.png)
- 关联Label做信息更新，选择Label中的SetCurrentSelection函数
	-  ![Pasted image 20241129170649.png](https://s2.loli.net/2024/12/01/TZQU6YKCrGljVxR.png)
- Options 
	- 下拉列表显示内容（空一行表示加一个）
	- 按照下图的设置，就能run，有一个下拉列表的情况了，但是可能还是有范围的问题
	- ![Pasted image 20241129164710.png](https://s2.loli.net/2024/12/01/UvWxKfuwgTJhO3Y.png)
	- ![Pasted image 20241129164808.png](https://s2.loli.net/2024/12/01/cs3KLb4HThD69WF.png)
- Position 下拉列表出现位置
	- Auto 自动（建议自动让其自动判断）
	- Above 向上，如果会产生出屏幕了，会自己调整
	- Below 向下，如果会产生出屏幕了，会自己调整
- Selection 选中操作
	- On Press 按下选中（默认选这个）
		- 按下的时候会触发Value的事件
		- ![Pasted image 20241129165727.png](https://s2.loli.net/2024/12/01/C7l2cdtSITEaODk.png)
	- On Click 点击选中
- Alignment 下拉列表的对齐方式
	- Automatic 自动对齐
	- Left 左对齐
	- Center 居中对齐
	- Right 右对齐
	- Justified 调整会自动变化
- Font，设置的是下拉列表上字体的选项
	- Font 字体设置
	- Font Size 字体大小
	- TextColor 字体颜色
	- Padding 偏移位置
		- X表示水平偏移，大于0表示右偏，小于0表示左偏
		- Y表示垂直偏移，大于0表示远离Label框，小于0表示接近Label框
	- Modifier 修饰方式，强制大小写字母
		- None 没有限制
		- To Uppercase 大写字母
		- To Lowercase 小写字母
		- Custom 自定义
- Open on 下拉列表打开方式
	- Click Or Tap 点击或者触碰
	- Right Click 右键
	- Double Click 双击
	- Manual 手动（相当于关闭。自己代码处理，一般不选择）
- On Top 始终显示在所有面板之前
	- 默认选择
	- 不选择可能被其他的面板遮挡，导致无法选择下拉列表
- Localized  是否将对弹出列表的值进行本地化
- Keep Value 始终保持有列表中的某个默认值
	- 勾选后，会默认显示下拉列表中的某个值，具体由initial Value设置
- initial Value
	- 可以选择默认初始化选择的列表显示值，需要勾选Keep Value才能生效，这个只是提供给我们一个选择的选项，实际起作用的还是Keep Value
- Atlas
	- Atlas 图集
	- Background 下拉列表背景图
	- Highlight 下拉列表选中图
		- 也就是每个选项选中后，背景显示的图
	- Background 背景颜色叠加
	- Highlight 选中高亮叠加家宴
	- Overlap 弹出窗口边框与打开它的内容重叠的数量
	- Animated 是否有默认的弹出动画，禁用可以节约性能

![PopupList_parameters1.png](https://s2.loli.net/2024/12/01/mG6H7tEI3aBUYfn.png)
![PopupList_parameters2.png](https://s2.loli.net/2024/12/01/OVnsUmJlCp79PQK.png)
![PopupList_parameters3.png](https://s2.loli.net/2024/12/01/uwFtcRb4DoT1ixf.png)

# Slider滑动条控件知识点
> 图片的质量很重要，要对齐，不能上面1像素空白，下面3像素空白，不然对不齐

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    public UISlider slider;
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Slider是啥？
        //滑动条控件
        //主要用于设置音乐音效大小等
        #endregion

        #region 知识点二 制作Slider
        //1.3个sprite 1个做根对象为背景  2个子对象 1个进度 1个滑动块 
        //2.设置层级
        //3.为根背景添加Slider脚本
        //4.添加碰撞器（父对象或者滑块）
        //5.关联3个对象
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖曳脚本关联
        //2.通过代码关联
        // 拖曳中调用
        slider.onChange.Add(new EventDelegate(() => {

            print("通过代码监听" + slider.value);
        }));
		// 拖曳结束时调用
        slider.onDragFinished += () => {
            print("拖曳结束" + slider.value);
        };
        #endregion
    }

    public void OnChange()
    {
        print("值变化" + slider.value);
    }

}
```

- Value 当前值0~1
- Steps 步数将1平分
- Appearance 外观设置
	- Foreground 前景 (用于缩放)
	- Background 背景
	- Thumb 拖动块
	- Direction 拖动方向
		- Left To Right 从左到右
		- Right To Left 从右到左
		- Bottom To Top 从下到上
		- Top To Bottom 从上到下
- OnValueChange 值变化时监听脚本

- 3个sprite 1个做根对象为背景  2个子对象 1个进度 1个滑动块 
	- ![Pasted image 20241130114237.png](https://s2.loli.net/2024/12/01/waJirGL4bQ7MXUf.png)
- 设置层级
	- 滑动块最前面 2
	- 进度条中间 1
	- 背景最后面 0
	- 调整的是Sprite脚本上的Depth
	- ![Pasted image 20241130114446.png](https://s2.loli.net/2024/12/01/PJ63qxQRcVYaleX.png)
- 为根背景添加Slider脚本
	- ![Pasted image 20241130114552.png](https://s2.loli.net/2024/12/01/aedbJqSOk1mxQsr.png)
- 添加碰撞器（父对象或者滑块）
	- ![Pasted image 20241130114646.png](https://s2.loli.net/2024/12/01/o8OpU1GlRXN4qjY.png)
- 关联3个对象
	- ![Pasted image 20241130114800.png](https://s2.loli.net/2024/12/01/iAWZ9JUCvkTtpGY.png)

![Slider_parameters.png](https://s2.loli.net/2024/12/01/NWXdlAj4qKmVrJp.png)

# ScrollBar滚动条和ProgressBar进度条知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson11 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 ScrollBar和ProgressBar用来干啥
        //1.ScrollBar滚动条一般不单独使用 都是配合滚动视图使用 类似VS右侧的滚动条
        //2.ProgressBar进度条 一般不咋使用   一般直接用Sprite的Filed填充模式即可

        //他们的参数和之前的知识很类似
        //所以这两个知识点不是重点 了解如何制作他们即可
        #endregion

        #region 知识点二 制作Scrollbar
        //1.两个Sprite 1个背景 1个滚动条
        //2.背景父对象添加脚本
        //3.添加碰撞器
        //4.关联对象
        #endregion

        #region 知识点三 制作ProgressBar
        //1.两个Sprite 1个背景 1个进度条
        //2.背景父对象添加脚本
        //3.关联对象
        #endregion
    }
}
```

## ScrollBar滚动条
- 两个Sprite 1个背景 1个滚动条
- 背景父对象添加脚本
	- 这里要求Scrollbar和Sprite一样长，是重叠的
	- ![Pasted image 20241130165310.png](https://s2.loli.net/2024/12/01/c3RP7UyBZuIFdXw.png)
- 添加碰撞器
	- ![Pasted image 20241130165358.png](https://s2.loli.net/2024/12/01/bHGnIKW3jDUV1cm.png)
- 关联对象
	- ![Pasted image 20241130165437.png](https://s2.loli.net/2024/12/01/W8UcetuLTbazQfZ.png)
- Steps
	- 表示按分成几步来移动
![Pasted image 20241130165706.png](https://s2.loli.net/2024/12/01/Lli8vGdBckJX1sh.png)

## ProgressBar进度条
- 填充如果是纯色，可以用Sprite脚本上的FIlled类型控制进度
	- 如果不是纯色，更加细节，则需要用ProgressBar控制进度条
- 两个Sprite 1个背景 1个进度条
- 背景父对象添加脚本
	- ![Pasted image 20241130170511.png](https://s2.loli.net/2024/12/01/lsMKpuxA7XTHUjb.png)
- 不需要添加碰撞器，因为不需要手动改变进度条，是代码控制
- 关联对象
	- ![Pasted image 20241130170550.png](https://s2.loli.net/2024/12/01/ycfSoqX3uWwltZR.png)

# ScrollView 滚动视图知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson12 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 ScrollView来干啥
        //滚动视图
        //我们现在用于编程的VS代码窗口就是典型的滚动视图
        //游戏中主要用于 背包、商店、排行榜等等功能
        #endregion

        #region 知识点二 制作ScrollView
        //1.直接工具栏创建即可 NGUI——Create——ScrollView
        //2.若需要ScrollBar 自行添加水平和竖直
        //3.添加子对象 为子对象添加Drag Scroll View和碰撞器
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 自动对齐脚本Grid 参数相关

        #endregion
        // 通过sv控制滚动条更新
        public UIScrollView sv;
	    sv.UpdateScrollbars();
    }
}
```

- 直接工具栏创建即可 NGUI——Create——ScrollView
	- ScrollView不会创建UIRoot，所以需要先创建UIRoot
	- ![Pasted image 20241130171629.png](https://s2.loli.net/2024/12/01/Wv1xMhgjaFV6ecC.png)
	- ![Pasted image 20241130171416.png](https://s2.loli.net/2024/12/01/YRDlGMwdQy8S3T4.png)
- 若需要ScrollBar 自行添加水平和竖直
- 添加子对象 为子对象添加Drag Scroll View和碰撞器
	- ![Pasted image 20241130172617.png](https://s2.loli.net/2024/12/01/8pzmGqVaNdIZCBi.png)
	- 添加碰撞器
	- ![Pasted image 20241130172704.png](https://s2.loli.net/2024/12/01/xZSKqFRcrDoYNVC.png)

- ScrollView使用Panel管理，Clipping是SoftClip
- 只有左边紫色的范围内才能够看到图像，其他的需要滚动显示，多余的会被裁剪掉
![Pasted image 20241130171846.png](https://s2.loli.net/2024/12/01/PHdLwpjsecXWaIT.png)



- Content Origin 内容子对象对其方式
- Movement 拖曳的方向，指的是Sprite那个对象，可以在哪个方向可以拖动
	- Horizontal 水平
	- Vertical 垂直
	- Unrestricted 自由
	- Custom 自定义
- Drag Effect 拖动特效
	- None 不使用任何效果
	- Momentum 动量 (惯性）效果 (差异不明显)
	- Momentum And Spring 动量（惯性）和弹力效果
	- 一般选择`Momentum And Spring`
- Scroll Wheel Factor 滚动因子
	- 如果不为0，鼠标中间滚动可以滚动它,可以通过它控制速度和方向
	- 负数表示方向反向
- Momentum Amount 动量
	- 拖曳一下鼠标，动的快慢
	- 可以理解成惯性大小
- Spring Strength
	- 弹力大小
	- 移动到边缘时弹回时弹力大小
- Dampen Strength
	- 如果比较大，会很难拖动
	- 阻尼强度
	- 影响回弹效果
- Restrict Within Panel
	- 限制在Panel中
	- 不勾选，不会产生弹力效果，也就是不回弹回
- Constrain OnDrag
	- 阻力约束，一般不修改
- Cancel Drag if fits
	- 若勾选，滚动视图的内容是否应该是可拖动的，取决于它们当前子对象大小是否溢出
	- 勾选后，如果对象太小，没有占满屏幕，则不允许拖动
- Smooth Drag Start
	- 平滑拖动，一般不修改
- IOS Drag Emulation
	- IOS阻力模拟，一般不修改
- ScrollBars 滚动条关联
	- Horizontal 水平滚动条
	- Vertical 竖直滚动条
	- ShowCondition 显示时机
		- Always 一直显示
		- Only If Needed 需要时显示
		- When Dragging 拖动时显示

- 自己手动制作一个ScrollBar
	- ![Pasted image 20241130175950.png](https://s2.loli.net/2024/12/01/RwsEy4S5nND1z3T.png)
- 关联到ScrollView上
	- ![Pasted image 20241130180037.png](https://s2.loli.net/2024/12/01/kUJXTzcbqIj9ipy.png)
- Grid脚本
- ![Pasted image 20241130180449.png](https://s2.loli.net/2024/12/01/apK1vrzlUscoAZ6.png)
- Arrangement 排序对齐方式
	- Horizontal 水平
	- Vertical 竖直
	- Cell Snap 元素大小
- Cell width 元素宽
- Cell Height 元素高
- Row Limit 元素个数（只有水平或者竖直才有）
	- 会自动换行
	- 如果选择的是水平，表示列数Row Limit个
	- 如果选择的是竖直，表示行数为Row Limit个
- Sorting 排序顺序
	- None 没有排序
	- Alphabetic 按字母排序
	- Horizontal 水平放置顺序
	- Vertical 垂直放置顺序
	- Custom 自定义
- Inverted 倒转
	- 若选择了排序方式
	- 勾选这里可以翻转排序规则
- Pivot 锚点位置
	- 9宫格9个位置，一般选择左上角
- Smooth Tween 平缓缓动动画
	- 是否会平滑地将其子对象设置为正确的位置
- Hide Inactive  是否隐藏不活动组件
- Constrain To panel 约束面板
	- 是否将网格子对象的更改通知父容器Panel，勾选后会通知
	- 会用于更新ScrollBar等显示信息
![ScrollView_parameters1.png](https://s2.loli.net/2024/12/01/RA4TYraCbNL7vhW.png)
![ScrollView_parameters2.png](https://s2.loli.net/2024/12/01/Fa56kZ1q7jNzXHK.png)

![Grid_parameters.png](https://s2.loli.net/2024/12/01/uPITABFdXv4tGRc.png)


# Anchor锚点组件知识点

## 老版本的Anchor
- UICamera 关联UI摄像机
- Container 控制的容器对象
- Side 9宫格位置
- Run Only Once
	- 是否只对齐一次
	- 一般情况下，设备分辨率都定死的，横屏竖屏也是定死的，所以勾选，除非分辨率要变化才取消勾选
- Relative Offset 相对比例偏移位置（0~1）
	- 一般建议用这个，不同设备像素密度不同，可能有差别
- Pixel Offset 像素偏移位置
- Type 尺寸对齐方式
	- None 无
	- Unified 统一模式
	- Advanced 高级模式
		- 每条边都有一个父对象
- Execute 什么时候执行更新 
	- On Enable 激活的时候更新
	- On Update 每一帧更新
	- On Start 在Update之前的Start()更新
- Target 相对目标（默认选择的是第一个最近的含有Panel脚本的父对象，一般UI Root都有Panel脚本）
	-  Left Sprite左边的蓝色线
	- Right Sprite右边的蓝色线
	- Bottom  Sprite下面的蓝色线
	- Top 上面的蓝色线
	
![Pasted image 20241130222925.png](https://s2.loli.net/2024/12/01/v51r2G9X7jHi6OQ.png)
- 如果不勾选Run Only Once，会在运行的时候重新计算分辨率变化
- 勾选可以解决性能
![Pasted image 20241130224129.png](https://s2.loli.net/2024/12/01/kBt5rx8pwb9XPDJ.png)
 - Anchors中的参数
 - Left蓝色的线，相对于中间竖直的黑色的线的距离是左偏50px
 - Right蓝色的线，相对于中间竖着的黑色的线的距离是右偏50px
 - Bottom蓝色的线，相对于中间横着的黑色的线的距离是下偏50px
 - Top蓝色的线，相对于中间横着的黑色的线的距离是上偏50px
 - 最后实际的效果是，Sprite图案按照这个下面的标准进行位置确定，缩放的话是其他参数
![Pasted image 20241130224923.png](https://s2.loli.net/2024/12/01/2rDFgWBSUwX9vIO.png)
- Anchors中的Left...和Target's Center在哪里
- Left指的是蓝色的四条线，Target's指的是红色的四条线加上两条黑色的线
- ![Pasted image 20241130225057.png](https://s2.loli.net/2024/12/01/tCkwXEWv1fRKFil.png)


![Anchor_parameters.png](https://s2.loli.net/2024/12/01/VQuXgjtapqUYPf1.png)

# EventListener和EventTrigger知识点

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson14 : MonoBehaviour
{
    public UISprite A;
    public UISprite B;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 控件自带事件的局限性
        //目前复合控件只提供了一些常用的事件监听方式
        //比如
        //Button —— 点击
        //Toggle —— 值变化
        //等等
        //如果想要制作 按下 抬起 长按等功能 利用现在的知识是无法完成的
        #endregion

        #region 知识点二 NGUI事件 响应函数
        //添加了碰撞器的对象
        //NGUI提供了一些利用反射调用的函数
        //经过 OnHover(bool isOver)
        //按下 OnPress(bool pressed)
        //点击 OnClick()
        //双击 OnDoubleClick()
        //拖曳开始 OnDragStart()
        //拖曳中  OnDrag(Vector2 delta)
        //拖曳结束 OnDragEnd()
        //拖曳经过某对象 OnDragOver(GameObject go)
        //拖曳离开某对象 OnDragOut(GameObject go)
        //等等等等
        #endregion

        #region 知识点三 更加方便的UIEventListener和UIEventTrigger
        //他们帮助我们封装了所有 特殊响应函数
        //可以通过它进行管理添加
        //1.UIEventListener 适合代码添加
        UIEventListener listener = UIEventListener.Get(A.gameObject);
        listener.onPress += (obj, isPress) =>
        {
            print(obj.name + "被按下或者抬起了" + isPress);
        };

        listener.onDragStart += BeginDrag;

        //2.UIEventTrigger 适合Inspector面板 关联脚本添加


        //UIEventListener和UIEventTrigger区别
        //1.Listener更适合，代码添加监听，Trigger适合拖曳对象添加监听
        //2.Listener传入的参数更具体，Trigger就不会传入参数，我们需要在函数中去判断处理逻辑
        #endregion
    }

    private void BeginDrag(GameObject obj)
    {
        print(obj.name + "开始拖曳");
    }

    public void Press() { }

    //void OnPress(bool pressed)
    //{
    //    if( pressed )
    //    {
    //        print("按下");
    //    }
    //    else
    //    {
    //        print("抬起");
    //    }
    //}

    //void OnHover(bool isOver)
    //{
    //    if( isOver )
    //    {
    //        print("鼠标经过");
    //    }
    //    else
    //    {
    //        print("鼠标离开");
    //    }
    //}

    //void OnClick()
    //{
    //    print("点击相关");
    //}

    //void OnDoubleClick()
    //{
    //    print("双击相关");
    //}

    //void OnDragStart()
    //{
    //    print("开始拖曳");
    //}

    //void OnDrag(Vector2 delta)
    //{
    //    print("拖曳中" + delta);
    //}

    //void OnDragEnd()
    //{
    //    print("拖曳结束");
    //}

    // 这个obj记录的是拖曳的对象，不是经过的对象
    //void OnDragOver(GameObject obj)
    //{
    //    print("拖曳经过" + obj.name);
    //}

    // 这个obj记录的是拖曳的对象，不是经过的对象
    //void OnDragOut(GameObject obj)
    //{
    //    print("拖曳离开" + obj.name);
    //}
}
```

- UI Root中的Camera会自带Event System脚本，他检测鼠标点击到了哪些对象，找到点击的对象后，找到他身上挂载的脚本，执行其中能够符合自己要求的方法
![Pasted image 20241130232259.png](https://s2.loli.net/2024/12/01/7nRQfbNaBjGgSvy.png)

# DrawCall 知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson15 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 DrawCall的概念
        //字面理解DrawCall  就是 绘制呼叫的意思  表示 CPU（中央处理器）通知GPU（图形处理器-显卡）

        //DrawCall 概念
        //就是CPU(处理器)准备好渲染数据（顶点，纹理，法线，Shader等等）后
        //告知GPU(图形处理器-显卡)开始渲染（将命令放入命令缓冲区）的命令

        //简单来说：一次DrawCall就是 CPU准备好渲染数据通知 GPU渲染的这个过程

        //如果游戏中DrawCall数量较高会影响CPU的效率
        //最直接的感受就是游戏会卡顿

        //举例说明  以拷贝文件来进行类比
        //假设我们创建10000个小文件，每个文件大小为1kb，然后把这些文件拷贝到另一个文件夹中
        //你会发现，即使这些文件加起来不超过10MB，但是拷贝花费的时间是很长的
        //如果我们单独创建1个10MB的文件拷贝到另一个文件夹，基本可以瞬间拷贝完毕
        //为什么会这样呢？
        //因为每一个文件赋值动作都需要很多额外的操作，比如分配内存，创建数据等等
        //这些操作就会带来一些额外的性能开销
        //简单理解 文件越多额外开销就越大

        //渲染过程和上面的例子很类似，每次DrawCall，CPU都需要准备很多数据发送给GPU
        //那么如果DrawCall越多那么额外开销就越大，其实GPU的渲染效率是很强大的，往往影响渲染效率的
        //都是因为CPU提交命令的速度
        //如果DrawCall 太多CPU就会把大量时间花在提交DrawCall上 造成CPU过载，游戏卡顿
        #endregion

        #region 知识点二 如何降低DrawCall数量
        //在UI层面上
        //小图合大图——>即多个小DrawCall变一次大DrawCall
        #endregion

        #region 知识点三 制作UI时降低DrawCall的技巧
        //1.通过NGUI Panel上提供的DrawCall查看工具
        //2.注意不同图集之间的层级关系
        // 如果三张图片来自两个图集，Depth分别为0，1，2
        // 0，2是同一个图集，1是另一个图集，DrawCall是3
        // 所以不要让不同图集的Depth插入在同一个图集的Depth中
        //3.注意Label的层级关系
        // 字体也算是一个图集，字体会被打包在同一个图集中
        // 一般把文字Depth放在最上面
        #endregion
    }
}
```

- 查看DrawCall数量
![Pasted image 20241130235502.png](https://s2.loli.net/2024/12/01/8HYL9sPO6uTVCy2.png)
- 
![Pasted image 20241130235644.png](https://s2.loli.net/2024/12/01/NHgIA6tMpEChvxR.png)

# NGUI字体知识点
- 打开NGUI字体制作
![Pasted image 20241201001346.png](https://s2.loli.net/2024/12/01/B2YI6sNXokiFyMZ.png)

- Generated Bitmap
	- 生成Bitmap
- Imported Bitmap
	- 导入Bitmap
- Dynamic
	- 动态字体，已经有Unity的了，不用管这个
![Pasted image 20241201001424.png](https://s2.loli.net/2024/12/01/lnmyW9gcYET1oSA.png)

- Font Maker的设置
- `Generated Bitmap`
	- Create Font会保存这个字体，有用到字体的地方可以拖入生成的文件
		- 但是如果有文字不在图集中，就无法显示了
![Pasted image 20241201001819.png](https://s2.loli.net/2024/12/01/ramjKoG7BktHNSQ.png)

- 使用Bitmap Font生成字体软件
![Pasted image 20241201002108.png](https://s2.loli.net/2024/12/01/qEdlVCLvnwBMZ7g.png)

![Pasted image 20241201002241.png](https://s2.loli.net/2024/12/01/XkRvxwzbhF2dLie.png)

![Pasted image 20241201002256.png](https://s2.loli.net/2024/12/01/eHjQ5nf46s8Ki2F.png)

![Pasted image 20241201002358.png](https://s2.loli.net/2024/12/01/HRLaNSTQYU6wKb1.png)
- 选择想要导出的文字
![Pasted image 20241201002507.png](https://s2.loli.net/2024/12/01/tGQHxY8ZflKS2Pg.png)
- 预览结果
![Pasted image 20241201002438.png](https://s2.loli.net/2024/12/01/ngVY5A7yDe6ZU3m.png)
- 可以选择文件中出现的文字来打图集
- 文件必须是带有BOM的utf-8格式的txt文件
![Pasted image 20241201002552.png](https://s2.loli.net/2024/12/01/zD19BokjPnRaJdU.png)
- 导出图集
![Pasted image 20241201002732.png](https://s2.loli.net/2024/12/01/uthZdGXCAsoFc6I.png)

![Pasted image 20241201002829.png](https://s2.loli.net/2024/12/01/PLbKsq9Zjk8TAGC.png)

- 继续Create the Font，可以产生Unity中的其他图集合并

## 自定义美术字体
- 还是之前的Bitmap Font生成字体软件
![Pasted image 20241201003020.png](https://s2.loli.net/2024/12/01/WluQxzXq8mG9VUD.png)
- 插入美术字体
![Pasted image 20241201003059.png](https://s2.loli.net/2024/12/01/urToBsby2lCS9Av.png)
![Pasted image 20241201003138.png](https://s2.loli.net/2024/12/01/oTpHAvRYeOBPjV2.png)

![Pasted image 20241201003302.png](https://s2.loli.net/2024/12/01/R8DcvzwtUZ2g4FY.png)
- 删除指定的美术字体
![Pasted image 20241201003321.png](https://s2.loli.net/2024/12/01/lHIP8bBXd9DViS7.png)

# NGUI缓动知识点
> 该对象上也要加碰撞器

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson17 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 什么是NGUI缓动
        // NGUI缓动，就是让控件交互时，进行缩放变化，透明变化，位置变化，角度变化等等行为
        // NGUI自带Tween功能来实现这些缓动效果
        #endregion

        #region 知识点二 使用NGUI缓动
        // 1. 关键组件 Tween缓动相关组件
        // 2. 关键组件，Play Tween 可以通过它让该对象和输入事件关联
        #endregion
    }
}
```

- Alpha 指从某个Alpha值变成另一个Alpha值
	- 是透明度的意思
![Pasted image 20241201005139.png](https://s2.loli.net/2024/12/01/dwV1LpqSkWmejvo.png)

- 以Scale为例
![Pasted image 20241201005305.png](https://s2.loli.net/2024/12/01/xVuHUw9ArMbkL8G.png)

- From 开始状态
- To 结束状态
- PlayStyle 播放方式
	- Once 一次
	- Loop 循环
	- Ping Pong 循环从头到尾从尾到头
- Animation Curve 动画曲线
	- 可以调整两个值的变化曲线
- Duration 持续时间
- Start Delay 开始播放前的延迟时间
- Delay Affects 延迟影响
	- Forward 正向播放时才有延迟效果
	- Reverse  反转播放时才有延迟效果
	- Both 都有延迟效果
		- 如果时Ping Pong的话，播放只有正向会延迟，反向不会延迟
		- 只有代码控制的时候，反转播放才会有延迟
- Tween Group 分组ID
	- 用于一个对象多个动画时分组
- Ignore TimeScale 忽略时间暂停
	- 即使timescale变成了0，也不会停止缓动
- Use Fixed Update 使用物理更新更新动画
	- 一般不勾选，物理时间可能会很长
- On Finished
	- 只有选中Once的播放方式才调用这个，其他的播放方式不会停止
![Pasted image 20241201005346.png](https://s2.loli.net/2024/12/01/HzhyXdYm6uLDpM4.png)

![PlayTween_parameters.png](https://s2.loli.net/2024/12/01/cp9GObg6FLZ3EY5.png)
- Play Tween脚本
![Pasted image 20241201010424.png](https://s2.loli.net/2024/12/01/ZDLSya53eYb6nQH.png)
- Tween Target 控制对象
	- 也就是控制哪个对象缓动
	- 这里一般默认是哪个对象加了这个脚本，就选择哪个对象
- Include Children 是否带着子对象一起变化
	- 可能有其他子对象，类似按钮有Label
- Start State 
	- 如果为真，则在激活触发之前,PlayTween将在启动时将所有关联的Tween重置为其起始状态
	- 不要让缓动一走来自动播放，而是控制它什么时候播放
- TweenGroup 控制的是哪一组缓动
	- 和上面的Tween Group对应
- Trigger condition 触发条件
	- 也就是什么时候触发这个缓动，上面的Start State被设置为False，这里可以设置为True，开始缓动播放
	- On Click
	- On Hover
	- On Press
	- On Hover True
	- On Hover False
	- On Press True
	- On Press False
	- On Activate
	- On Activate True
	- On Activate False
	- On Double Click
	- On Select
	- On Select True
	- On Select False
	- Manual
- Play direction 触发的事件
	- Reverse 反转播放
		- 一开始为结束状态，往开始状态转换
	- Toggle 正反状态转换
		- 不断正反状态转换，从开始到结束，结束到开始
	- Forward 正向播放
- If target is disabled 如果控制对象失活处理方式
	- Do Nothing 啥也不做，一般选择这个
	- Enable Then Play 为了播放激活它
	- Ignore Disabled State 忽略失活状态
		- 不管你是不是激活的，继续缓动这个失活的对象
- On activation 激活时
	- Continue From Current 继续当前，一般选这个
	- Restart Tween 重新开始
	- Restart If Not Playing 如果没有播放重新开始
- When finished 播放完毕做啥
	- Disable After Reverse 执行完后隐藏
	- Do Not Disable 什么也不做，一般选这个
	- Disable After Forward 如果是倒着播，播放完后隐藏
- On Finished
	- 播放完做什么

![PlayTween_key_parameters.png](https://s2.loli.net/2024/12/01/ue8oCVFdswTnZjm.png)

![PlayTween_parameters 1.png](https://s2.loli.net/2024/12/01/ZqMOSGniQNgBwJj.png)

# NGUI中显示3D模型和例子特效

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson18 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 NGUI中显示3D模型
        //方法一：
        //使用UI摄像机渲染3D模型
        //1.改变NGUI的整体层级 为 UI层
        //2.改变主摄像机和NGUI摄像机的 渲染层级
        //3.将想要被UI摄像机渲染的对象层级改为 UI层
        //4.调整模型和UI控件的Z轴距离

        //方法二：
        //使用多摄像机渲染 Render Texture
        #endregion

        #region 知识点二 NGUI中显示粒子特效
        //1.让Panel和粒子特效处于一个排序层
        //2.在粒子特效的 Render参数中 设置自己的层级
        #endregion
    }
}
```

## 使用UI摄像机渲染3D模型
- 改变NGUI的整体层级 为 UI层
	- ![Pasted image 20241201012411.png](https://s2.loli.net/2024/12/01/K8UEtMDcuhjwoxa.png)
	- 3D模型也要改为UI层，如果模型不显示，可能是模型太小，改一下Scale
		- ![Pasted image 20241201012455.png](https://s2.loli.net/2024/12/01/6DR7BeZWdMk1fQJ.png)
		- ![Pasted image 20241201012533.png](https://s2.loli.net/2024/12/01/bvtmaPqpURAJyui.png)
- 改变主摄像机和NGUI摄像机的 渲染层级
	- 主摄像机不渲染UI层
	- NGUI摄像机只渲染UI层
	- 另外3D模型的渲染不受Depth影响，之和Z轴的实际距离有关
- 将想要被UI摄像机渲染的对象层级改为 UI层
	- 也就是Tank改成UI层
- 调整模型和UI控件的Z轴距离
	- 就能看到不同层级叠加的情况
## 使用多摄像机渲染 Render Texture
> 这里可以使用3D摄像机，实现近大远小
- 创建一个Render Texture
![Pasted image 20241201012913.png](https://s2.loli.net/2024/12/01/Kj8HVyq29UnWISM.png)

- 将刚刚的Render Texture拖到想要渲染的摄像机那里
- 这里是将摄像机看到的内容都放在了Tank这个Render Texture里面
- 其中左边的Camera和Tank4都是一个Water的Layer层，其他的相机都不渲染这个层
![Pasted image 20241201013039.png](https://s2.loli.net/2024/12/01/tpl5InaRfvgTWBV.png)

- 此时将Render Texture中的内容渲染到Texture上，类似小地图的功能
![Pasted image 20241201013233.png](https://s2.loli.net/2024/12/01/Cr2LxgWzioQJ4ZD.png)

## NGUI中显示粒子特效
- 创建例子特效
	- ![Pasted image 20241201013512.png](https://s2.loli.net/2024/12/01/rIkxMg4VPC6n8Nh.png)
- 让Panel和粒子特效处于一个排序层
	- ![Pasted image 20241201013641.png](https://s2.loli.net/2024/12/01/pEgviIrs27dotFC.png)
- 在粒子特效的 Render参数中 设置自己的层级
	- 改层级或者同层中的优先度都可以改变显示顺序
	![Pasted image 20241201013621.png](https://s2.loli.net/2024/12/01/xjg2XydAE5R9l7W.png)

# 事件触发播放音效、键盘绑定、语言本地化等知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson19 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 NGUI 事件响应 播放音效
        //PlaySound 脚本
        #endregion

        #region 知识点二 NGUI控件和键盘按键绑定
        //KeyBinding 脚本
        #endregion

        #region 知识点三 PC端 tab键快捷切换选中
        //KeyNavigation 脚本
        #endregion

        #region 知识点四 语言本地化
        //Localization脚本
        //1.在Resources下创建一个txt文件 命名必须为Localization
        //2.配置文件
        //3.给想要切换文字的Label对象下挂载Localize 关联Key
        //4.给用于切换语言的下拉列表下添加脚本LanguageSelection
        #endregion
    }
}
```


- 添加播放音效脚本
	- ![Pasted image 20241201014032.png](https://s2.loli.net/2024/12/01/afz8oWbmtTjqJVy.png)
![Pasted image 20241201014151.png](https://s2.loli.net/2024/12/01/1nrjMIfuV3HGtBR.png)

- 添加键盘绑定脚本
	- ![Pasted image 20241201014239.png](https://s2.loli.net/2024/12/01/blZIJVfXKgwtM5P.png)
	- ![Pasted image 20241201014504.png](https://s2.loli.net/2024/12/01/8qfhBbWcLaYldnP.png)
- PC端 tab键快捷切换选中
	- ![Pasted image 20241201014610.png](https://s2.loli.net/2024/12/01/74qDY9dCMNkotKz.png)
	- Sprite这四个对象需要添加碰撞器
	- 按了Tab后，会到开始的右边的按钮，如果还需要按Tab切换，则需要这四个对象都添加Key Navigation脚本
	- ![Pasted image 20241201014701.png](https://s2.loli.net/2024/12/01/JcFmYtoqv32Iu7n.png)

- 语言本地化
- Localization脚本
- 在Resources下创建一个txt文件 命名必须为Localization
	- ![Pasted image 20241201014952.png](https://s2.loli.net/2024/12/01/64KbB2lZPcfYrNW.png)
- 配置文件
	- 表示volume的关键字，对应的中文为音量，英文为volume
	- ![Pasted image 20241201015026.png](https://s2.loli.net/2024/12/01/h9W8l2qNsSaPeuE.png)
- 给想要切换文字的Label对象下挂载Localize 关联Key
	- 对Label组件添加Localize脚本
	- 默认选择配置文件中的第一个
	- ![Pasted image 20241201015140.png](https://s2.loli.net/2024/12/01/pNXaU7n6RsQjd4D.png)
	- ![Pasted image 20241201015213.png](https://s2.loli.net/2024/12/01/fSqVXb9coAUClLN.png)
- 给用于切换语言的下拉列表下添加脚本LanguageSelection
	- 添加一个PopUpList下拉列表
		- 添加PopupList脚本，以及碰撞器
			- 记得设置字体为Unity字体/其他的字体也行，不能为空，否则下拉列表不显示文字
			- 修改On Value Change的触发事件
			- ![Pasted image 20241201015400.png](https://s2.loli.net/2024/12/01/uIj1wP3KJACEHZ8.png)
			- ![Pasted image 20241201015613.png](https://s2.loli.net/2024/12/01/9uj4gPetBa2H81X.png)
		- 添加Language Selection脚本
			- ![Pasted image 20241201015453.png](https://s2.loli.net/2024/12/01/wxrjGetczCDyAsm.png)
- ![Pasted image 20241201015723.png](https://s2.loli.net/2024/12/01/K5ouifPFLcgQpEs.png)
- 也能改图片
- 原理应该是Label标签就改文字，Sprite组件就改图片，但是图集估计要在同一个上面
- ![Pasted image 20241201015847.png](https://s2.loli.net/2024/12/01/GptVJS7gLsEZF8d.png)
- ![Pasted image 20241201015910.png](https://s2.loli.net/2024/12/01/8b3KUg7dEOIyBnM.png)






# 参考文献
[游习堂](https://www.yxtown.com/)
