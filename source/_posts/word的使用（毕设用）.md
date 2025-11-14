---
title: word的使用（毕设用）
math: true
---
## 重要快捷键的使用
```shell
# 有的快捷键，确实不一定都用过
# wing2791(我)也只是，罗列在word中本人觉得重要的
```

```shell
CTRL + H # 替换
# 配合^$ 任意英文字母
# 配合^# 任意数字

SHIFT + CTRL + 8 # MathType显示和隐藏分隔符

F9 # 更新域快捷键
```
- 按Ctrl+H唤出替换菜单

![Pasted image 20250328113856.png](https://s2.loli.net/2025/03/28/Hb8ldTKMN2I7yhk.png)
- 查找内容中可以输入`^#`或者`^$`表示替换所有的数字或者字母
- 如果先选中某些文本，再按`CTRL+H`则可以只替换选中文字的部分
![Pasted image 20250328114120.png](https://s2.loli.net/2025/03/28/yv4XG3e6JRaEpF8.png)

- 这里修改为`Time New Roman`，就能够将所有的数字或者英文字母换成指定字体
- 中文？不知道，论文啥的一般是宋体或者黑体交错使用，不建议直接换，可以先整体换成宋体或者黑体，然后再换里面的数字和字母
	- 操作：选中中英文混合的文字，先改为宋体，再改为Time New Roman
	- 这样中文就全为宋体，英文和数字为新罗马，一般可以用于目录的修改
![Pasted image 20250328120633.png](https://s2.loli.net/2025/03/28/DAiK8BqLW4ksQwm.png)

## 目录
> 目录当然都会用吧，但是还是讲一下
> 也是写给自己看的

### 目录模板的设置
- 目录项显示到3级，两端对齐，段前0行，段后0行，固定值20磅。
	- 一级目录项：中文字体为宋体，英文字体为Times New Roman，四号，加粗，无缩进，左缩进0字符，右缩进0字符
	- 二级目录项：中文字体为宋体，英文字体为Times New Roman，五号，加粗，左缩进1字符，右缩进0字符
	- 三级目录项：中文字体为宋体，英文字体为Times New Roman，五号，左缩进2字符，右缩进0字符
- 注意保持页码右对齐，有前导符，正文修改后注意刷新目录保持一致。

- 目标就是上面的设置
- 引用->目录->自定义目录
![Pasted image 20250720203504.png](https://s2.loli.net/2025/09/02/udR4652Mn7IlShr.png)
-  设置页码显示->页码右对齐->页码前导符->显示级别（也就是只显示几级标题）
![Pasted image 20250720203736.png](https://s2.loli.net/2025/09/18/5vdGlkOQNKLFSiT.png)

- 点击右下角的修改，来修改细节
![Pasted image 20250720204018.png](https://s2.loli.net/2025/09/18/NgaOGyEYMbwPT1c.png)

- 修改一级标题的样式
![Pasted image 20250720204324.png](https://s2.loli.net/2025/09/18/XBPSk3a7Ozd9ylq.png)

- 左下角的格式，选择<font color="red">“字体”</font>
- 字体中中文改为宋体，西文字体改为Times New Roman，加粗
![Pasted image 20250720204614.png](https://s2.loli.net/2025/09/18/SAVw6D75M9GaE8W.png)

- 左下角格式选择<font color="red">“段落”</font>
- 对齐方式为两端对齐，缩进左侧和右侧均为0字符，大纲级别正文文本，间距段前和段后均为0行
![Pasted image 20250720205016.png](https://s2.loli.net/2025/09/18/pjnYalve3rWLhUx.png)
- 另外两级标题类似，不继续做过多记录

### 插入目录
![Pasted image 20250918203108.png](https://s2.loli.net/2025/09/18/jV7DAKnfZlb3Uke.png)
- 点击确定
![Pasted image 20250720211723.png](https://s2.loli.net/2025/09/18/Tro8GPeE7uIzjlc.png)

- 最后结果如下图所示，仔细观察，一级标题为黑体。。。有很多问题，还需要继续修改
- 选中所有目录，字体先宋体->后Times New Roman，此时右侧的页码字体格式大小有误，都需要设置为五号，Times New Roman，
![Pasted image 20250720205330.png](https://s2.loli.net/2025/09/18/58WAyjSbUOwveck.png)
### 继续细节修改目录格式
- 选中三级标题，双击这个格式刷，表示后续的格式和三级标题的一致
- 格式刷双击后会一直可以使用，如果不可以，再往下有解决方法
![Pasted image 20250720205612.png](https://s2.loli.net/2025/09/18/1G92SRVDMvNlsLT.png)

- 开启下图所示的按钮，就能在目录中看到空白符
- 有了格式刷，可以在右箭头的左侧双击，然后右侧双击
![Pasted image 20250720210622.png](https://s2.loli.net/2025/09/18/jIlQv3K7fU2X4bO.png)
- 如果格式刷使用失效，可以参考下面图片设置
- 一般是加载项出问题了
![Pasted image 20250720210458.png](https://s2.loli.net/2025/09/18/nCathS1gm5eDJ2z.png)

## 页眉页脚脚注
### 页眉
#### 不同小节不同
- 演示文档是两页，第一页后面添加了分节符（下一页）
- 两个分节符之间的页数是一个小节（如果只有一个分节符，上下分别是两个小节）
![Pasted image 20250727225408.png](https://s2.loli.net/2025/09/18/PMXZECcqtyTSAHp.png)
- 来到第二页，双击上面的页眉页脚，打开页眉页脚选项卡，取消`链接到前一节`
- 此时在两页的页眉页脚中输入内容，不会相互同步
- 如果想要相互同步，点击`链接到前一节
- 如果想要奇偶页不同，这里勾选`奇偶页不同`就可以了`
![Pasted image 20250727225829.png](https://s2.loli.net/2025/09/18/Yrdml8RLJFobxDu.png)

#### 设置（取消）一条横杠
- 双击某一页上方，打开页眉页脚
![Pasted image 20250727225139.png](https://s2.loli.net/2025/09/18/MoDxZb4j9tUIEWY.png)
- 打开页眉页脚->选中文字->`设计`选项卡->页面边框->边框->自定义->预览中选择要添加的方向（我选择的是下边框）->应用于`段落`->确定
![Pasted image 20250727230543.png](https://s2.loli.net/2025/09/18/fDu9CYWkycBrO53.png)
![Pasted image 20250727230631.png](https://s2.loli.net/2025/10/16/BPfyFG6LVMjDSge.png)

### 页脚
> 主要是页码设置

#### 不同小节不同
- 双击某页下方，可以打开页眉页脚，取消`链接到前一节`
- 如果想要奇偶页不同，这里勾选`奇偶页不同`就可以了
![Pasted image 20250727231321.png](https://s2.loli.net/2025/10/13/H7AsocnfImq9jwD.png)
#### 设置页码
- 打开页眉和页脚->页码->设置页码格式
![Pasted image 20250727231539.png](https://s2.loli.net/2025/10/13/hILTptb6cH4wuPQ.png)
- 设置页码格式和是否接上上一节
![Pasted image 20250727231720.png](https://s2.loli.net/2025/10/13/PK4UQkN2WboZrMv.png)
#### 添加页码
- 一般都是底端，中间
- 页眉和页脚->页码->页码底端
![Pasted image 20250727231820.png](https://s2.loli.net/2025/10/13/WOjhZAdukG3m4ST.png)


### 脚注
- 引用->右下角箭头->修改格式->应用
![Pasted image 20250727223712.png](https://s2.loli.net/2025/10/13/qkzpgmsbFX2aO98.png)
- 引用->插入脚注
![Pasted image 20250727224049.png](https://s2.loli.net/2025/10/13/iKPT2jOWkZMuo4t.png)

## 页面布局（包括打印版，左右页边距不同）
- 页面布局设置如下
- 布局->页边距->自定义页边距->页码范围(多页->对称页边距/普通)
> 因为论文设置一般页面偏右，具体数据如下图所示
> 如果是打印版本，要求奇偶页不同，那么奇数页要求偏右，偶数页要求偏左，选择`对称页边距`，电子版本选择`普通`

![Pasted image 20250727232140.png](https://s2.loli.net/2025/10/13/386ngz4YD1vcfyL.png)

## 公式书写
> 如果论文公式很多，建议mathtype或者overleaf模板写
> 不是很多，并且改动的不是很频繁，可以用我下面的方法

### word自带的
- 菜单栏`插入`
![Pasted image 20250328121103.png](https://s2.loli.net/2025/03/28/NPBbXtjAVG9aoiq.png)

- 新出现的地方，选中，菜单栏会多一个`公式`，然后进行自己的公式插入
![Pasted image 20250328121544.png](https://s2.loli.net/2025/03/28/OFH9R3hZUxWYLqA.png)


#### 其他辅助工具
[Latex转换为word里面的公式](https://temml.org/)

- 打开上述网站
![Pasted image 20250328121750.png](https://s2.loli.net/2025/03/28/snU3CJtbq8SOAYz.png)

- 例如，Input中输入，参数设置必须和上图中一样
- 点击上图中的复制，粘贴到word中，截图如下图所示
```shell
u \overset {I_{\cup}}{\rightarrow} v
```
![Pasted image 20250328122028.png](https://s2.loli.net/2025/03/28/tZkhrRXE9egCTHf.png)

#### 公式标序号
- 建议选择“专业”而不是“全部专业”，”全部专业“比较卡
![Pasted image 20250402203140.png](https://s2.loli.net/2025/04/02/l6gfJNv4PdkwXEy.png)

![Pasted image 20250402203409.png](https://s2.loli.net/2025/04/02/52nRAquXEYiwozy.png)

### MathType
> MathType是一个word插件，需要自己安装

[最原理的一集——Mathtype公式编号设置（Mathtype7.8+Word）](https://blog.csdn.net/Mr_Tang4/article/details/138506022)

- 比较常用的按钮是下面三个（其他的也用，但是稳定后用的少）
![Pasted image 20250720212248.png](https://s2.loli.net/2025/10/13/vDUzKcxjLBiuesh.png)

### 一个例子讲解公式的插入
- 这个是基本示例，后续内容使用该文档举例
![Pasted image 20250721185150.png](https://s2.loli.net/2025/10/13/KvJntREIgVw37Pz.png)

#### 插入分隔符
![Pasted image 20250721190644.png](https://s2.loli.net/2025/10/13/JewZoGP3F2aKyjg.png)

- 设置第一章的分隔符格式
![Pasted image 20250721190813.png](https://s2.loli.net/2025/10/13/hoJ6L7cQnNjZEyU.png)
- 下一章的分隔符设置
![Pasted image 20250721210603.png](https://s2.loli.net/2025/10/13/UeSjKAwNGR2baWu.png)
- 如果插入的方式在“第二章”的文字后面，生成目录就会多这几个字
- 建议不要在一级、二级、三级标题上加MathType的分隔符
- 就加在蓝色框上面
- 快捷键Ctrl+Shift+8来显示/隐藏 MathType分隔符（空白符也显示和隐藏）
![Pasted image 20250721210949.png](https://s2.loli.net/2025/10/14/KDfe8JH3ZYqIkgd.png)

#### 格式化公式
- MathType->插入编号->格式化...
![Pasted image 20250721211453.png](https://s2.loli.net/2025/10/14/TBfsICrQFomxELt.png)

- 我这里演示的是
	- (x.y) 其中x是章节，y是该章节中第y个公式
![Pasted image 20250721211705.png](https://s2.loli.net/2025/10/14/POoZNX2g8yhnqLC.png)

#### 插入公式
![Pasted image 20250721212750.png](https://s2.loli.net/2025/10/14/bYMfhaScKToeGkx.png)

![Pasted image 20250721213013.png](https://s2.loli.net/2025/10/14/GJRvEdZhUg83rNm.png)
- 勾上“允许从键盘输入TeX语言”，使用的时候需要加$
![Pasted image 20250721213036.png](https://s2.loli.net/2025/10/14/fNivXmI2xjy67QG.png)
- 输入$x_{i}
- 然后回车
![Pasted image 20250721213154.png](https://s2.loli.net/2025/10/14/n3VQ1bNpT8IPYCh.png)

- 记得按Ctrl+S保存
![Pasted image 20250721213204.png](https://s2.loli.net/2025/10/14/JXtxRcGNDYeUihB.png)

## 表格/图
### 表例/图例的自动化标注（交叉引用）
#### 表例/图例插入

![Pasted image 20250726181520.png](https://s2.loli.net/2025/10/14/MpGcOCjntIusUz6.png)
- 先引用->插入题注->新建标签
![Pasted image 20250726181752.png](https://s2.loli.net/2025/10/14/xfrz4Mc5jJhA3Fi.png)
- 输入想要添加的标签文字（如果有的话，不需要重复添加）
- 如果想要按照章节论文表x.y
- 直接就添加“表1.”
- 例如第二章的第三张图，会自动标为表1.3
![Pasted image 20250726181823.png](https://s2.loli.net/2025/10/14/9py76bLjMwn3eQI.png)

- 会自动更新是第几张表，下面两张图可以看见，会多一个`空格`在表和数字之间，需要手动删除
![Pasted image 20250726182040.png](https://s2.loli.net/2025/10/14/zfJ9PTIeEjRM75v.png)
![Pasted image 20250726182102.png](https://s2.loli.net/2025/10/14/GZJe8BNpXAYDi6j.png)


#### 表例/图例引用
- 插入->交叉引用->修改`引用类型`->修改`引用内容`->选择`引用哪一个题注`
![Pasted image 20250726182523.png](https://s2.loli.net/2025/10/16/8htv5V6B7qmawJD.png)
- 然后点击`插入`
- 因为上面勾选了`插入为超链接`，按住Ctrl，点击表1，可以定位到上面表1的位置
![Pasted image 20250726182845.png](https://s2.loli.net/2025/10/16/DIhA2Z6OdVqRuUo.png)
#### 表例/图例更新
- 选中某个需要更新的表例或者图例（插入的和引用的均适用），`右键->更新域`
- 更新域快捷键`F9`，可以Ctrl+A选中所有，然后`F9`更新（可能会很慢，不推荐）
![Pasted image 20250726183015.png](https://s2.loli.net/2025/10/16/tYUF398lXLq7mQZ.png)

### 跨表格的设置

### 表头的设置
- 表布局->重复标题行
- 希望重复的表头，可以选中，然后再按`重复标题行`
- 如果选中多行，会在跨页部分重复多行（下列图片展示的是重复了单行）
![Pasted image 20250726225742.png](https://s2.loli.net/2025/10/16/muDLKnfbGry4JQ2.png)

![Pasted image 20250726225830.png](https://s2.loli.net/2025/10/16/G3SeTPMRaZhWzBs.png)

### 续表的设置
- 鼠标光标放在第二页表的第一行（不是续表的表头）
![Pasted image 20250726230323.png](https://s2.loli.net/2025/10/16/VGWIZoC8HNadkzq.png)
- 插入->文本框->简单文本框
![Pasted image 20250726230609.png](https://s2.loli.net/2025/10/16/4IfuskEzjlCObGN.png)

- 让添加的文本框的上面的水平线和左右的竖直线对齐下面第二张图的`左上角`和`右上角`两个位置
![Pasted image 20250726230825.png](https://s2.loli.net/2025/10/16/DgkHJzW4qF81rVt.png)

![Pasted image 20250726230852.png](https://s2.loli.net/2025/10/16/4nAlM7TOmBoGNEX.png)
- 选中文本框，内部内容可以按照格式要求填写，支持居中等等操作
	- 表1 （续）
	- Table 1 (continued)
- 选择`形状格式`->形状轮廓->无轮廓
- 注意，如果插入文本框后，表格+文本框横跨三页，则无法将文本框拖到续表最上方
	- 只支持续表跨一页，且第二页加上文本框后不能导致多加一页
![Pasted image 20250726231421.png](https://s2.loli.net/2025/10/16/d8yN9roEBzuXalm.png)

## 引用参考文献
> 本文zotero使用版本是`7.0.15`
### 下载zotero
- [zotero下载链接](https://www.zotero.org/)

- 下面两个都要下载，第一个是zotero本体，第二个是谷歌浏览器上的插件
- 推荐谷歌浏览器，后面以谷歌浏览器为例
![Pasted image 20250327230941.png](https://s2.loli.net/2025/03/28/STDOJgaWtomGicy.png)

### zotero安装茉莉花插件
- [茉莉花插件链接（github链接）](https://github.com/l0o0/jasminum)
- 下图点击右下角框中的发布版本

![Pasted image 20250327232127.png](https://s2.loli.net/2025/03/28/u2GFfvP75loqiBY.png)
- 下载`.xpi`后缀文件
![Pasted image 20250327232315.png](https://s2.loli.net/2025/03/28/hZXso3YnQUquySW.png)
- 打开`zotero`->菜单栏`工具`->插件
![Pasted image 20250327234106.png](https://s2.loli.net/2025/03/28/7nUNsBGeWXtEJzd.png)
- 点击`设置`齿轮
![Pasted image 20250328000149.png](https://s2.loli.net/2025/03/28/pH6tAsQN1rekUz3.png)
- 选择`从文件中安装插件`，就是上面下载的`.xpi`文件
![Pasted image 20250328000235.png](https://s2.loli.net/2025/03/28/2oEps6TBOqiWKbN.png)
![Pasted image 20250328000351.png](https://s2.loli.net/2025/03/28/pflJCsWauV7YU1d.png)

### zotero添加文献
> 记得先打开软件zotero

![Pasted image 20250328001046.png](https://s2.loli.net/2025/03/28/IygYRbXfS98OBrW.png)

#### 知网
- 点击下列图片右上角的文档图标
![Pasted image 20250328001110.png](https://s2.loli.net/2025/03/28/F1pVJwiCrB7eus3.png)
- 这样就添加进去了
![Pasted image 20250328001211.png](https://s2.loli.net/2025/03/28/SvTuBjx6bE4iwtn.png)

#### 谷歌学术
- 右上角点击这个按钮
![Pasted image 20250328001521.png](https://s2.loli.net/2025/03/28/OFiAdx7fNjt3CIM.png)
- 勾选想要的论文
![Pasted image 20250328001740.png](https://s2.loli.net/2025/03/28/Z3Ozv6mWR8sUYSN.png)
- 然后就可以了，当然我下面演示的是失败的，如果失败后续会说如何手动添加
![Pasted image 20250328001825.png](https://s2.loli.net/2025/03/28/4z19FhE3egVrpXR.png)

### 手动添加文献
> 这里我也不会，自己本来就不看文献，只能给一些经验，给实在是没办法的时候使用
> 谷歌学术为例

- 以下列论文为例，在谷歌学术上找到了，但是zotero插件添加失败
![Pasted image 20250328154353.png](https://s2.loli.net/2025/03/28/aZADWGkndwoFxOY.png)
- 查看论文中的名字，谷歌学术中有的名字是省略的
![Pasted image 20250328154632.png](https://s2.loli.net/2025/03/28/ByfR7Axj8tETHpi.png)
- 打开zotero->文件->新建条目->引用文献的类型
![Pasted image 20250328154754.png](https://s2.loli.net/2025/03/28/RymUaqQJvwZLGkp.png)

- 上述举例子的论文，可以添加的，添加结果如下
- 主要讲一下作者和语言
	- 作者中，需要查看原论文，论文中是名->姓的顺序
	- 后面的第一个单词作为姓，先添加，第一个才是名
![Pasted image 20250328154852.png](https://s2.loli.net/2025/03/28/4WOt6kYwoqxy3iV.png)
- 语言，需要改为en，否则会出现`等`，而不是`et.al`
![Pasted image 20250328155316.png](https://s2.loli.net/2025/03/28/26JRvY4ajeZkXrn.png)

### word引用文献
> 提醒一遍，注意打开zotero软件
> ꒰ঌ( ⌯' '⌯)໒꒱

[在Word 中插入参考文献---zotero中文社区](https://zotero-chinese.com/user-guide/ms-word-plugin.html)
[参考博客—几个Zotero引文样式csl（GB/T 7714-2015）](https://zhuanlan.zhihu.com/p/654098728)
[上一个博客的样式下载链接](https://gitee.com/eduyob/citation-styles)

#### 安装样式文件
> 这里的样式文件我也没检查，不能说完全符合所有人需求
> 更合适的需要自己寻找
> [从上面的链接中下载样式文件](https://gitee.com/eduyob/citation-styles)
> [这个是我自己使用的样式文件](https://pan.baidu.com/s/15zuZ4kT66TJBX1--BpJGFw?pwd=0312)

- 打开zotero软件->编辑->设置
![Pasted image 20250328133349.png](https://s2.loli.net/2025/03/28/NROfG23VMKCUQz8.png)
- 引用-> `+`
![Pasted image 20250328133434.png](https://s2.loli.net/2025/03/28/KTzgH1E5LMFdvoP.png)

- 选择`zotero-styles`文件夹中的`.csl`样式文件
- 推荐这两个，是没有doi的样式
![Pasted image 20250328133904.png](https://s2.loli.net/2025/03/28/Dt1srqjx5eP7WVf.png)
### 开始引用文献
> 当然，需要打开zotero软件，以及添加几篇文献

- 只介绍我自己用的几个，其他不介绍，参考中文社区文档
	- Add/Edit Citation 添加或编辑引用
	- Add/Edit Bibliography 添加或编辑参考文献
	- Document Preferences 文档首选项，也就是引用样式文件设置
	- Refresh 刷新，更新引用和参考文献
	- Unlink Citations 移除所有引用和参考文献链接（只是不链接，但是引入的[1]，以及参考文献样式不会删除）
![Pasted image 20250328135009.png](https://s2.loli.net/2025/03/28/Ezqr8dfoCWbje9u.png)

- 点击左上角的Add/Edit citation，每个文档第一次添加引用都会弹出设置文档首选项
- 如果想修改，自己点击Document Preferences是一样的
- 这里我使用多作者省略，没有doi的版本
- 点击ok

![Pasted image 20250328135320.png](https://s2.loli.net/2025/03/28/NUY3rq4SKaoQGdv.png)

- 这里如果插入之前，选中了zotero中的某个文献
- 会自己显示选中的文献
![Pasted image 20250328135656.png](https://s2.loli.net/2025/03/28/V2u8qONBaUkRivS.png)
- 如果不是自己想要的文章，输入作者信息或者文献名称，会有下拉选项选择
![Pasted image 20250328135727.png](https://s2.loli.net/2025/03/28/W8M6XRFo4gGiUBb.png)
- 如果顺序有误，可以拖动，改变顺序
- 点击右箭头完成插入
![Pasted image 20250328135851.png](https://s2.loli.net/2025/03/28/cQmKwYrR4PgLAlt.png)
- 结果如下图所示
![Pasted image 20250328135919.png](https://s2.loli.net/2025/03/28/4RBjahfPlULEtHN.png)

- 告诉zotero，参考文献放哪里，先光标放在指定位置
- 点击Add/Edit Bibliography
![Pasted image 20250328135947.png](https://s2.loli.net/2025/03/28/Ejq1plF3mwyscSa.png)

- 结果如下图所示
- 但有个问题是，如果删除所有的[]引用，删除时不会一起把参考文献删掉的
![Pasted image 20250328140045.png](https://s2.loli.net/2025/03/28/IE1rWbYiUPgpZH7.png)
- 如果只有一个地方引用了，删除多个中的一个，点击Refresh，会进行提示，注意啊
- 此时也不要删除，只有一个引用的地方也不会进行刷新
![Pasted image 20250328140242.png](https://s2.loli.net/2025/03/28/41AKSRrQghBUNjn.png)
- 删除多个时，点击Add/Edit Citation，可以删除里面的部分引用，如果删除多个，做不到

![Pasted image 20250328140725.png](https://s2.loli.net/2025/03/28/YMyNEjvXblqc76V.png)
![Pasted image 20250328140735.png](https://s2.loli.net/2025/03/28/z7otqhDgNJ5b2wL.png)
- 删除后情况
![Pasted image 20250328140747.png](https://s2.loli.net/2025/03/28/CrdKRjleA53Ju7U.png)
- 可以直接文档中删除，但是只能删除全部，下列例子不止一个引用，没有问题
	- 如果只有一个引用，问题也会有，上面说了
- 删除多个里面的部分，只能够Add/Edit Citation，否则更新会出问题
![Pasted image 20250328140945.png](https://s2.loli.net/2025/03/28/z8cdGevKomVYty6.png)

- 不小心误删，点击了是
- 此时无论点击Refresh都不会去掉[2]的参考文献
![Pasted image 20250328141345.png](https://s2.loli.net/2025/03/28/WU84GO5Xqarb6em.png)
- 在误删的地方重新点击`Add/Edit Citation`
![Pasted image 20250328141430.png](https://s2.loli.net/2025/03/28/AI341LY9hHZDcPd.png)
- 点击是->`->`，会重新更新
![Pasted image 20250328141508.png](https://s2.loli.net/2025/03/28/yRq6FHTzJbDahcN.png)
- 按照上述步骤，用`Add/Edit Citation`来删除引用
![Pasted image 20250328141602.png](https://s2.loli.net/2025/03/28/wVFWACjPi719Ums.png)




## 参考博客
[最原理的一集——Mathtype公式编号设置（Mathtype7.8+Word）](https://blog.csdn.net/Mr_Tang4/article/details/138506022)
