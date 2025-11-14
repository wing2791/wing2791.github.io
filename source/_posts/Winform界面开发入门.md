---
title: Winform界面开发入门
math: true
---


# 小总结

|控件名称|作用描述|
|---|---|
|**Button**|按钮控件，触发事件或命令|
|**Label**|标签控件，显示文本或描述性信息|
|**TextBox**|文本框控件，允许用户输入或显示文本|
|**RichTextBox**|富文本框控件，支持格式化文本输入与显示|
|**CheckBox**|复选框控件，多选或布尔值选项|
|**RadioButton**|单选按钮控件，用于实现单选选项|
|**ListBox**|列表框控件，显示多项选择列表|
|**ComboBox**|组合框控件，显示下拉列表或允许输入|
|**PictureBox**|图片框控件，用于显示图像|
|**ProgressBar**|进度条控件，显示操作或加载进度|
|**DataGridView**|数据网格控件，显示、编辑表格数据|
|**TreeView**|树视图控件，以树形结构显示分级数据|
|**ListView**|列表视图控件，不同方式（图标、列表）展示数据|
|**Panel**|面板控件，用于分组或容纳其他控件|
|**TabControl**|选项卡控件，创建选项卡界面|
|**GroupBox**|分组框控件，将控件逻辑分组|
|**Timer**|定时器控件，在指定时间间隔触发事件|
|**TrackBar**|滑动条控件，在预定范围内调整数值|
|**NumericUpDown**|数字增减框控件，调整数值|
|**DateTimePicker**|日期时间选择器控件，选择日期或时间|
|**MenuStrip**|菜单条控件，创建应用程序顶部菜单栏|
|**ToolStrip**|工具条控件，创建工具栏，包含按钮或菜单项|
|**StatusStrip**|状态条控件，显示应用程序的状态信息|


# 创建Winform项目
![Pasted image 20241112202617.png](https://s2.loli.net/2024/11/13/WromYGP8XBRj9Mn.png)


![Pasted image 20241112205721.png](https://s2.loli.net/2024/11/13/9EaYdg1SnTtCAVq.png)


![Pasted image 20241112205115.png](https://s2.loli.net/2024/11/13/GgEb4IeJyOk8HW7.png)

# 认识WinForm中基本创建内容
- Program.cs
```csharp
namespace StudyWinform
{
    internal static class Program
    {
        /// <summary>
        /// 应用程序的主入口点。
        /// </summary>
        [STAThread]
        static void Main()
        {
            // [STAThread]：这个属性表明应用程序的主线程是单线程单元（Single Thread Apartment）模式。对于 Windows 窗体应用程序，这是一个常见要求，因为它保证 UI 组件在线程间的安全访问。
            // Application.EnableVisualStyles()：启用应用程序的视觉样式，使应用程序中的控件以系统主题的样式呈现，提供更现代的外观。
            // Application.SetCompatibleTextRenderingDefault(false)：指定应用程序中使用的文本渲染方式。false 表示使用 GDI+渲染文本，通常适用于 Windows 窗体应用的默认设置。
            // Application.Run(new Form1())：启动应用程序的主窗口，并将 Form1 作为主窗体加载。Application.Run() 方法会启动消息循环（Message Loop），保持应用程序运行，直到用户关闭 Form1。
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

![Pasted image 20241112205843.png](https://s2.loli.net/2024/11/13/tBL8TGbR7udsIN9.png)

![Pasted image 20241112210057.png](https://s2.loli.net/2024/11/13/9OTvyhQKgswe2VU.png)

![Pasted image 20241112210139.png](https://s2.loli.net/2024/11/13/3fvStZk6ITrcFz9.png)

![Pasted image 20241112210302.png](https://s2.loli.net/2024/11/13/dC7rlROJFqByu21.png)

![Pasted image 20241112210431.png](https://s2.loli.net/2024/11/13/uMzpySP92FfHxgh.png)

- 是两个分部类，分部类不知道的看之前的C#笔记
![Pasted image 20241112210738.png](https://s2.loli.net/2024/11/13/G47irLDSkFKzmfO.png)
![Pasted image 20241112210751.png](https://s2.loli.net/2024/11/13/D7I9fiFHGqVcjAn.png)


# 工具箱的位置
![Pasted image 20241112211805.png](https://s2.loli.net/2024/11/13/WVGqBuditxk6beM.png)
![Pasted image 20241112211949.png](https://s2.loli.net/2024/11/13/aRPNWOLHYFB7ovu.png)

# 实际WinForm操作（可视化）
- 实际操作开始了
![Pasted image 20241112213250.png](https://s2.loli.net/2024/11/13/veIElrF3tySqw1V.png)
- 双击这个按钮，能够跳转到代码编写部分（话说之前需要写界面的时候就试试C#了，比java的windowsbuilder要简单啊）
![Pasted image 20241112213318.png](https://s2.loli.net/2024/11/13/dMQAyskcNFtrHUz.png)
- 这个(Name)其实就是这个按钮在代码中的变量名
![Pasted image 20241112213711.png](https://s2.loli.net/2024/11/13/i6nPH3Q1qOfvYCl.png)

![Pasted image 20241112214231.png](https://s2.loli.net/2024/11/13/bN5cj1COZ6EFwmM.png)

# 实际WinForm操作（代码）
- 给Click事件添加了一个委托，委托里面注册的是this.button1_Click()函数
- 点击事件触发时，就会调用button1_Click()函数
```csharp
this.button1.Click += new System.EventHandler(this.button1_Click);
```

```csharp
private void button1_Click(object sender, EventArgs e)
{

}
```

![Pasted image 20241112215708.png](https://s2.loli.net/2024/11/13/98bkjxYLmnBXZfl.png)

- 创建一个按钮，并且进行操作的代码
```csharp
private void InitializeComponent()
{
    // 创建一个新的 Button 控件实例，赋值给 button1 字段
    this.button1 = new System.Windows.Forms.Button();
    this.SuspendLayout(); // 暂停布局逻辑，用于批量设置控件属性
    
    // 设置 button1 的外观和行为属性
    this.button1.ForeColor = System.Drawing.SystemColors.ControlText; // 设置按钮文本颜色
    this.button1.Location = new System.Drawing.Point(173, 109); // 按钮位置 (x:173, y:109)
    this.button1.Name = "button1"; // 控件名称，用于代码中引用
    this.button1.Size = new System.Drawing.Size(103, 42); // 控件大小 (宽:103, 高:42)
    this.button1.TabIndex = 0; // Tab 键的顺序
    this.button1.Text = "第一个按钮"; // 按钮上显示的文本
    this.button1.UseVisualStyleBackColor = true; // 启用按钮的系统视觉样式

    // 事件绑定 - 设置点击和鼠标进入事件的处理方法
    this.button1.Click += new System.EventHandler(this.button1_Click); // 点击事件，调用 button1_Click
    this.button1.MouseEnter += new System.EventHandler(this.button1_MouseEnter); // 鼠标进入事件，调用 button1_MouseEnter

    // 窗体的其他属性设置
    this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 12F); // 缩放尺寸
    this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font; // 缩放模式
    this.ClientSize = new System.Drawing.Size(863, 427); // 设置窗体的宽度和高度
    this.Controls.Add(this.button1); // 将 button1 添加到窗体的控件集合中
    this.Name = "Form1"; // 窗体名称
    this.Text = "Form1"; // 窗体标题栏显示的文本

    this.ResumeLayout(false); // 恢复布局逻辑
}

// 字段定义一个button1的按钮变量
private System.Windows.Forms.Button button1;
```
- 子集实例化按钮的例子
```csharp
namespace StudyWinform
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            // InitializeComponent();

            // 手动创建5个按钮
            for (int i = 0; i < 5; i++)
            {
                Button button = new Button();
                button.Text = "按钮" + i.ToString();
                // Point是一个struct，即他是一个值类型
                // Location是一个Point类型的属性，get回来的是值在栈上的副本，并不是引用
                // 所以Location.X的修改是对值类型副本的修改，而非值本身的修改，是无意义的，故提示错误。
                // button.Location.X = 50;
                button.Location = new Point(i * 50, 0);
                button.Size = new Size(50, 50);
                // 两个效果一样
                //button.Click += Button_Click;
                button.Click += new EventHandler(Button_Click);
                // button.Click += Button_Click;
                // 利用了编译器的简化语法，直接添加了 Button_Click 方法
                // 编译器会自动推断 Button_Click 是符合 EventHandler 委托类型的
                // 所以可以简化写法，不用显式创建 EventHandler 对象。
                // button.Click += new EventHandler(Button_Click);
                // 这是显式写法，明确创建一个 EventHandler 实例并将 Button_Click 方法作为参数传递进去
                // 这种写法更详细，但在功能上没有任何额外效果

                this.Controls.Add(button);

            }
        }

        private void Button_Click(object sender, EventArgs e)
        {
	        MessageBox.Show("我被点击了");
            Button button = sender as Button;
            button.Text = "被点击";
        }
    }
}
```

- 设置字体
- 后续还有好多都是类似的，除非很重要或者特殊，不然不展示了
![Pasted image 20241112230046.png](https://s2.loli.net/2024/11/13/ABDfLHye4uascJ6.png)

![Pasted image 20241112230056.png](https://s2.loli.net/2024/11/13/FHmcEfPouBYOb7a.png)


- 可以将输入变成\*的属性设置
![Pasted image 20241112230800.png](https://s2.loli.net/2024/11/13/237HDg6wvKOduai.png)

- 新建窗体
![Pasted image 20241112231425.png](https://s2.loli.net/2024/11/13/CasW3drJEAbqilO.png)
![Pasted image 20241112231453.png](https://s2.loli.net/2024/11/13/5QKcb9PMT71Z68l.png)

![Pasted image 20241112231511.png](https://s2.loli.net/2024/11/13/hm7NPTSguWJO8CY.png)

```csharp
    public partial class Form1 : Form
    {
        public Form1()
        {
            // InitializeComponent();

            // 手动创建5个按钮
            for (int i = 0; i < 5; i++)
            {
                Button button = new Button();
                button.Text = "按钮" + i.ToString();
                // Point是一个struct，即他是一个值类型
                // Location是一个Point类型的属性，get回来的是值在栈上的副本，并不是引用
                // 所以Location.X的修改是对值类型副本的修改，而非值本身的修改，是无意义的，故提示错误。
                // button.Location.X = 50;
                button.Location = new Point(i * 50, 0);
                button.Size = new Size(50, 50);
                // 两个效果一样
                //button.Click += Button_Click;
                button.Click += new EventHandler(Button_Click);
                // button.Click += Button_Click;
                // 利用了编译器的简化语法，直接添加了 Button_Click 方法
                // 编译器会自动推断 Button_Click 是符合 EventHandler 委托类型的
                // 所以可以简化写法，不用显式创建 EventHandler 对象。
                // button.Click += new EventHandler(Button_Click);
                // 这是显式写法，明确创建一个 EventHandler 实例并将 Button_Click 方法作为参数传递进去
                // 这种写法更详细，但在功能上没有任何额外效果

                this.Controls.Add(button);

            }
        }

        private void Button_Click(object sender, EventArgs e)
        {
            // MessageBox.Show("按钮被点击了");
            Button button = sender as Button;
            button.Text = "被点击";
            // Application.Run(new Index())：
            // 启动消息循环，显示窗体，并让应用程序保持运行直到窗体关闭。
            // new Index().Show()：
            // 仅显示窗体，且不启动消息循环，程序仍然会继续执行。
            // 表示一个新窗口，一个线程上只能有一个，使用会报错，关闭主窗口，子窗口也会关闭
            // Application.Run(new Index());
            new Index().Show();
            // 这里this是表示Main里面的new Form1()
            // 隐藏主窗口
            this.Hide();
        }
    }
```

# 设置子窗口关闭时关闭主窗口
- 新建的Index窗口添加这个事件
- 关闭的时候关闭所有程序
```csharp
this.FormClosed += new System.Windows.Forms.FormClosedEventHandler(this.Index_FormClosed);
```

```csharp
private void Index_FormClosed(object sender, FormClosedEventArgs e)
{
	// 让整个程序全部退出
	Application.Exit();
}
```

# GDI
- GDI绘图的基本步骤
1. 在窗体或者空间上创建画家（确定画在哪里，由谁来画）
2. 创建或使用已有的画笔或画刷、以及要画图像相关的坐标点
3. 使用画家对象的相关方法绘制图像
4. 销毁画家及画笔等
- 需要放在From中的Paint事件中
```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace StudyWinform
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            this.Paint += Form1_Paint;
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            // 使用Paint事件传递的Graphics对象，避免手动释放资源
            Graphics g = e.Graphics;
            // 创建一个画笔
            using (Pen pen = new Pen(Color.Red, 5))
            {
                // 准备两个点
                Point p1 = new Point(50, 80);
                Point p2 = new Point(250, 80);

                // 画直线
                g.DrawLine(pen, p1, p2);
            }
        }

    }
}
```

# 俄罗斯方块（联网）





# 参考文献
[【Winform入门教程 Visual Studio 2022】Winform界面开发入门](https://www.bilibili.com/video/BV1JY4y1G7jo)
[中国象棋游戏开发实战（C#）](https://zhuanlan.zhihu.com/p/483896504)
