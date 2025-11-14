---
title: VStudio的一些设置
math: true
---
> 本人用VStudio和VsCode用的比较多，所以就对他们的配置记录一下吧，如果有其他编译器的，也会记录在这里

# VStudio 修改默认的编码格式
[EditorConfig官网](https://editorconfig.org/)

> 这个方法只适合单个项目，不是修改VStudio这个软件

- 先进入项目根目录
![Pasted image 20241118154745.png](https://s2.loli.net/2024/11/18/UCym9RbG2gpsLew.png)

- 创建`.deitorconfig`文件
![Pasted image 20241118154834.png](https://s2.loli.net/2024/11/18/jGYPbsolncAR1Ud.png)
- 输入以下内容，然后保存。
- 所有的文件编码格式都会统一变为utf-8，那是不是如果需要统一改成其他的也可以啦。具体以后用到再说吧。之前创建的文件也会被修改为utf-8
```
root = true  # 所在目录是项目根目录，此目录及子目录下保存的文件都会生效    
 
[*]  # 对于所有文件
indent_style = tab  # 缩进风格
tab_width = 4  # 缩进宽度
charset = utf-8  # 文件编码格式
end_of_line = crlf  # 行尾格式，Windows一般为CRLF，Linux一般为LF，根据需要更改
insert_final_newline = true   #文件结尾添加换行符，以防警告
```
![Pasted image 20241118154902.png](https://s2.loli.net/2024/11/18/3YrKkobfFizZtCO.png)
















# 参考博客
[VS(Visual Studio)更改文件编码](https://blog.csdn.net/NSJim/article/details/123253659)