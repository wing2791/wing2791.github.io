---
title: LaTex表示数学符号
math: true
---

#### 长期更新,慢慢添加

- 因为自己有时候会学一些数学相关的东西,会用公式表示,用 LaTex 的好看啊
- 目录在右边,可以导航用
- 可能一些自己用的比较熟悉的就不添加了，目前主要添加平时会用到，但是会忘记的表示方法

#### 上标,下标

$$
a^b
$$

```shell
a^b
```

$$
a_b
$$

```shell
a_b
```

$$
a^{ab}
$$

```shell
a^{ab}
```

$$
a_{ab}
$$

```shell
a_{ab}
```

$$
\overset{B}{\rightarrow}
$$

```shell
\overset{B}{\rightarrow}
```

$$
\underset{B}{\rightarrow}
$$

```shell
\underset{B}{\rightarrow}
```
#### 空格

>  强制一个空格
> 如果需要强制插入一个空格（通常在命令中需要分隔时使用），使用 \ （反斜杠+空格）：

$$
This\ is\ a\ sentence\ with\ manual\ spaces.
$$

```LaTex
This\ is\ a\ sentence\ with\ manual\ spaces.
```

##### 不同宽度的空格

>  调整空格的宽度

| 命令         | 宽度               | 示例                          |
| ------------ | ------------------ | ----------------------------- |
| `\,`         | 1/6 个四分之一空格 | `a\,b` → $a ba\,bab$          |
| `\:`         | 1/2 个四分之一空格 | `a\:b` → $a ba\:bab$          |
| `\;`         | 一个四分之一空格   | `a\;b` → $a  ba\;bab$         |
| `\quad`      | 一个全宽空格       | `a\quad b` → $aba\quad bab$   |
| `\qquad`     | 两个全宽空格       | `a\qquad b` → $aba\qquad bab$ |
| `\hspace{n}` | 自定义宽度空格     | `\hspace{10pt}`               |

##### 不间断空格

>  这两个单词不会分开换行

$$
a~b
$$

```latex
a~b
```





