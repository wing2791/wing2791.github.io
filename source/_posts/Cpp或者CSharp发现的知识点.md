---
title: Cpp或者CSharp发现的知识点
math: true
---
1. aspDotNet生命周期（C#）
2. PInvoke 和restful(C#)
3. ~~C++实现反射（C++）~~
4. 编码格式的问题，char接收中文为2字节，宽字符，utf-8中文占用3字节
5. 欧拉角的万向节死锁
6. C++配置文件的编写
7. C++中push_back()和emplace_back()的区别
8. C++，最近做项目，发现用vector\<Tri\>*比vector\<vector\<Tri\>\>，快很多很多，这是为什么呢，是内存不断扩容嘛?后者我也用.resize()了啊，前者new vector\<Tri\>[max_v]
