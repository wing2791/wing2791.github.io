---
title: VSCode基本使用和Emmet语法
---



### VSCode基本使用和Emmet语法

#### Emmet语法

>Emmet用于加快HTML和CSS代码的编写速度。
>
>能够通过简短的表达式就可以生成HTML或CSS代码片段。
>
>截至2022年，主流的编辑器工具如Visual Studio Code、WebStorm都已经继承了`Emmet`工具，无需手动安装即可使用
>
>文档地址：<https://docs.emmet.io/cheat-sheet/>

test.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <!-- 单个标签 -->
  <!-- p -->
  <p></p>

  <!-- 父子关系标签 -->
  <!--   div>ul>li -->
  <div>
    <ul>
      <li></li>
    </ul>
  </div>

  <!-- 生成多个标签 -->
  <!--   div>ul>li*5 -->
  <div>
    <ul>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>

  <!-- 兄弟关系标签 -->
  <!-- div+p -->
  <dic></dic>
  <p></p>

  <!-- 属性 -->
  <!-- #box -->
  <div id="box"></div>

  <!-- p#box -->
  <p id="box"></p>

  <!-- dic.cls .cls -->
  <div class="cls"></div>

  <!-- 同时生成id和类 -->
  <!-- div.title.#header -->
  <!-- div.title#header -->
  <div class="title" id="header"></div>

  <!-- 自动生成内容 -->
  <!-- p{Hello你好！} -->
  <p>Hello你好！</p>

  <!--   p{Hello你好$}*5 -->
  <p>Hello你好1</p>
  <p>Hello你好2</p>
  <p>Hello你好3</p>
  <p>Hello你好4</p>
  <p>Hello你好5</p>

  <!-- a[href="https//www.baidu.com"] -->
  <a href="https//www.baidu.com"></a>

  <!-- input[data-content='AAAA'] -->
  <input type="text" data-content="AAAA">

  <!-- div>p*2+ul>li*2 -->
  <div>
    <p></p>
    <p></p>
    <ul>
      <li></li>
      <li></li>
    </ul>
  </div>

  <!-- p>span.cls$*6 -->
  <p>
    <span class="cls1"></span>
    <span class="cls2"></span>
    <span class="cls3"></span>
    <span class="cls4"></span>
    <span class="cls5"></span>
    <span class="cls6"></span>
  </p>


</body>

</html>
```



#### 快捷键以及其他命令

```shell
code #在windows中的命令行输入code会打开vscode软件
CRTL + / #单行注释或取消
ALT + UP/DOWN # 移动行
SHIFT + ALT + UP/DOWN # 复制当前行
CTRL + '+或-' # 设置IDE整体字体大小
CTRL + ALT + UP/DOWN # 多行编辑
SHIFT + CTRL + K # 删除当前行
```



#### VSCode插件推荐

- Auto Rename Tag

  >自动将标签全部修改
  >
  ><p></p> 修改其中的p，另一个p也会修改

- Code Runner

  > 运行多种语言的代码段或代码文件,实际上还要安装其他插件，不知道是不是自带的

- Code Translate

  > 能翻译代码中的英文

- ESLint

  > vue代码格式化

- JavaScript(ES6) code snippets

  > js语法报错提示

- Live Server

  > 使用服务器（本地）运行html文件

- Material Icon Theme

  > 修改文件目录的图标

- open in browser

  > 使用浏览器打开html文件

- Prettier Code formatter

  > 代码格式化
  >
  > 参考<https://huaweicloud.csdn.net/638ee1f6dacf622b8df8d8c0.html>

- Vetur 

  > Vue代码格式化

- Vue Language Feature(Volur)

  > Vue代码格式化

[![pSjl7RI.png](https://s1.ax1x.com/2023/02/21/pSjl7RI.png)](https://imgse.com/i/pSjl7RI)

[![pSjlOL8.png](https://s1.ax1x.com/2023/02/21/pSjlOL8.png)](https://imgse.com/i/pSjlOL8)

#### VSCode配置

左下角齿轮->setting

过一段时间自动保存

[![pSjlxoQ.png](https://s1.ax1x.com/2023/02/21/pSjlxoQ.png)](https://imgse.com/i/pSjlxoQ)

去除右上角的迷你窗口

[![pSj1pJs.png](https://s1.ax1x.com/2023/02/21/pSj1pJs.png)](https://imgse.com/i/pSj1pJs)

增加Ctrl+鼠标滑轮缩放代码字体大小

[![pSj19Wn.png](https://s1.ax1x.com/2023/02/21/pSj19Wn.png)](https://imgse.com/i/pSj19Wn)

