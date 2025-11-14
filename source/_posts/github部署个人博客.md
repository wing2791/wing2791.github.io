---
title: github部署个人博客
---
#### 参考链接

[Hexo 博客搭建并部署到 GitHub Pages(2024 最新详细版)](https://blog.csdn.net/clearloe/article/details/139879493)

#### 安装 VsCode

[VSCode 安装配置使用教程（最新版超详细保姆级含插件）一文就够了](https://blog.csdn.net/msdcp/article/details/127033151)

[VsCode 下载链接](https://code.visualstudio.com/Download)

- 如果无法下载，可以选择在电脑的应用商城里面下载，我记得是要挂梯子才能官网下载
- 安装就是正常安装，建议别 C 盘

#### 安装 Git

[Git 安装详解（写吐了，看完不后悔）](https://blog.csdn.net/qq_45730223/article/details/131693287)

- 很常用的软件，程序员必装，按照教程装就好了，不过有时候验证有问题，也要梯子

#### 安装 nodejs

[node.js 安装及环境配置超详细教程【Windows 系统安装包方式】](https://blog.csdn.net/weixin_44893902/article/details/121788104)

> 搬运上述链接评论
> Node.js Express 安装报错总结
> express 4.x 版本之前 全局安装 express 命令是 npm install express -g
> express 4.x 版本之后 全局安装 express 命令是 npm install -g express-generator

#### 安装 hexo

[Win10 任意目录下默认快速以管理员身份运行 CMD](https://www.cnblogs.com/zhujingxiu/articles/7462025.html "发布于 2017-09-01 10:34")
[安装 hexo 参考博客链接](https://blog.csdn.net/clearloe/article/details/139879493)
[运行 npm install 不动时](https://blog.csdn.net/qq_42786011/article/details/123895927)

```shell
//  配置nmp代理来提高速度，如设置淘宝镜像
npm config set registry  https://registry.npmmirror.com/

// 查看配置是否成功
npm config get registry

// 成功后重新npm install安装
// npm install 。。。
```

#### 更新文章

```shell
# 提交远程仓库更新
hexo c && hexo g && hexo d
```

```shell
# 本地更新
hexo c && hexo g && hexo s
```

#### 遇上的错误，解决方法供参考

##### fatal: detected dubious ownership in repository

[参考博客](https://stackoverflow.com/questions/73408170/git-fatal-detected-dubious-ownership)

![202407281829173](https://s21.ax1x.com/2024/09/10/pAm8py6.png)

- 解决方法

```shell
git config --global --add safe.directory *
```

##### 安装 Pandoc，运行 Pandoc 有误，说找不到命令

> 主要问题是我的 360 会自动删除 pandoc.exe,我在 360 软件管家中的隔离区恢复了就行

[pandoc 安装地址](https://github.com/jgm/pandoc/blob/master/INSTALL.md)

正常安装是有下面的四个文件，如果少了 exe 需要按照上面的进行恢复

![202407281832443](https://s21.ax1x.com/2024/09/10/pAm8iwD.png)

##### Hexo 使用 markdown 插入图片无法显示解决方法（废弃，直接找网络图床，https 的）

> 免费图床：https://imgse.com/
> 每天八张

[参考博客](https://www.jianshu.com/p/04814a816caf)

- 安装插件

  - 先进入自己 blog 的目录
    ![image-20240728203701626](https://s21.ax1x.com/2024/09/10/pAm8PeO.png)
  - 用命令安装（如果第二次使用还是失效，重新运行下列命令，会重新安装一遍，再弄就是可以了，具体的解决方案没找到）

    ```bash
        # npm install https://github.com/7ym0n/hexo-asset-image --save # 已经失效，换一个
        npm install hexo-asset-img --save
    ```
  - 运行下列命令，提交本地查看结果

    ```shell
    hexo clean && hexo g && hexo s # 本地查看
    hexo clean && hexo g && hexo d # 上传到github
    ```
- 修改 hexo 根目录下的\_config.yml 文件

  ```bash
    post_asset_folder: true

    # URL
    ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
    url: http://yourname.github.io
  ```
- 插入图片

  - 我用的是 typora，直接设置如下，然后复制图片，直接到文件中即可

  ![image-20240728204034558](https://s21.ax1x.com/2024/09/10/pAm8FTe.png)

  ![image-20240728204107912](https://s21.ax1x.com/2024/09/10/pAm8AFH.png)

  ```shell
  ![xx](xx/xx.png)
  ```

#### 其他可能用得上的网址

[Hexo 官方中文文档](https://hexo.io/zh-cn/docs)
[Hexo Fluid 用户手册](https://hexo.fluid-dev.com/docs/)
