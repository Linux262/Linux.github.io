---
layout:     post
title:      快速搭建个人博客网页
subtitle:   手把手教你在半小时内搭建自己的个人博客(如果不踩坑的话🙈🙊🙉)
date:       2017-08-20
author:     miki
header-img: img/post-bg-build-blog.jpg
catalog: true
tags:
    - Blog
---

> 正所谓前人栽树，后人乘凉。
> 
> 感谢[BY](https://github.com/qiubaiying)提供的博客模板
> 
> [我的博客](http://happymiki.top)

#一、前言
本来想自己用Ubuntu+LAMP搭建一个服务器，在安装好LAMP环境之后，发现居然需要n多知识，比如PHP,JavaScript，CSS,HTML，MySQL，Apache等众多的技能，学完之后搭建起来可能黄花菜都凉了，于是乎在网上找到了github，居然可以用github pages来搭建自己的静态网站，就立马开始忙乎，从 Jekyll 到 GitHub Pages 中间踩了不少的坑，终于把我的个人博客[Mlin Blog](http://happymiki.top)搭建出来了。。。

本教程针对的是不懂技术又想搭建个人博客的小白，操作简单暴力且快速。当然懂技术那就更好了。

废话不多说了，开始进入正文。

# 二、快速开始

### 2.1 从注册一个Github账号开始

我采用的搭建博客的方式是使用 [GitHub Pages](https://pages.github.com/) + [jekyll](http://jekyll.com.cn/) 的方式。

要使用 GitHub Pages，首先你要注册一个 [GitHub](https://github.com/) 账号，GitHub 是全球最大的同性交友网站(吐槽下程序员~)，你值得拥有。

![](http://upload-images.jianshu.io/upload_images/6757403-97fbb59e5609ddfe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.2 拉取我的博客模板

注册完成后搜索 `happymiki.github.io` 进入[我的仓库](https://github.com/happymiki/happymiki.github.io)

![](http://upload-images.jianshu.io/upload_images/6757403-045e040cd88f3613.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


点击右上角的 **Fork** 将我的仓库拉倒你的账号下

稍等一下，点击刷新，你会看到**Fork**了成功的页面

![](http://upload-images.jianshu.io/upload_images/6757403-2b3c188c71c48c21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.3 修改仓库名

点击**settings**进入设置

![](http://upload-images.jianshu.io/upload_images/6757403-042afbab11db2b16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<p id = "Rename"></p>
修改仓库名为 `你的Github账号名.github.io`，然后 Rename


![](http://upload-images.jianshu.io/upload_images/6757403-fa168c3d374bfbac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时你在在浏览器中输入 `你的Github账号名.github.io` 例如:`happymiki.github.io`

你将会看到如下界面

![](http://upload-images.jianshu.io/upload_images/6757403-9d0d250cd988e577.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说明已经成功一半了😀。。。当然，还需要修改博客的配置才能变成你的博客。

若是出现

![](http://upload-images.jianshu.io/upload_images/6757403-0b435b88fd556ba6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

则需要 [检查一下你的仓库名是否正确](#Rename)

### 2.4 整个网站结构

修改Blog前我们来看看Jekyll 网站的基础结构，当然我们的网站比这个复杂。

```
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
├── img
└── index.html
```

很复杂看不懂是不是，不要紧，你只要记住其中几个OK了

- `_config.yml` 全局配置文件
- `_posts`  放置博客文章的文件夹
- `img` 存放图片的文件夹

其他的想继续深究可以[看这里](http://jekyll.com.cn/docs/structure/)



### 2.5 修改博客配置

来到你的仓库，找到`_config.yml`文件,这是网站的全局配置文件。

![](http://upload-images.jianshu.io/upload_images/6757403-00413a3170983488.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击修改

![](http://upload-images.jianshu.io/upload_images/6757403-ef8ee01535db68a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后编辑`_config.yml`的内容

![](http://upload-images.jianshu.io/upload_images/6757403-170cd7490738a2eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来我们来详细说说以下配置文件的内容：

#### 2.6 基础设置

```
# Site settings
title: You Blog                     #你博客的标题
SEOTitle: 你的博客 | You Blog        #显示在浏览器上搜索的时候显示的标题
header-img: img/post-bg-rwd.jpg     #显示在首页的背景图片
email: You@gmail.com    
description: "You Blog"              #网站介绍
keyword: "Mlin, Mlin Blog, happymiki" #关键词
url: "https://happymiki.github.io"          # 这个就是填写你的博客地址
baseurl: ""      # 这个我们不用填写

```
#### 2.7 侧边栏

```
# Sidebar settings
sidebar: true                           # 是否开启侧边栏.
sidebar-about-description: "说点装逼的话。。。"
sidebar-avatar: img/avatar-by.JPG      # 你的个人头像 这里你可以改成
我在img文件夹中的两张备用照片 img/avatar-m 或 avatar-g
```

#### 2.8 社交账号
展示你的其他社交平台

![](http://upload-images.jianshu.io/upload_images/6757403-90e8c0ffb6ac9828.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在下面你的社交账号的用户名就可以了，若没有可不用填

```
# SNS settings
RSS: false
weibo_username:     username
zhihu_username:     username
github_username:    username
facebook_username:  username
jianshu_username:   jianshu_id
```
新加入了**简书**，`jianshu_id` 在你打开你的简书主页后的地址如：`http://www.jianshu.com/users/3f05018752b8`中，后面这一串数字：`3f05018752b8 `

#### 2.9 评论系统

我们博客的评论系统切换到 [Disqus](https://disqus.com/),在官网注册帐号之后，在下面的填写你多说的用户名的就可以使用。

![](http://upload-images.jianshu.io/upload_images/6757403-3399ea4fc7e9c7cc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# Disqus settings（https://disqus.com/）
disqus_username: happymiki
```

#### 2.10 网站统计

集成了 [Baidu Analytics](http://tongji.baidu.com/web/welcome/login) 和 [Google Analytics](http://www.google.cn/analytics/)，到各个网站注册拿到track_id替换下面的就可以了

这是我的 Google Analytics

![](http://upload-images.jianshu.io/upload_images/6757403-8f0afcf1d655449d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

若不想启用统计，直接删除或注释掉就可以了

```
# Analytics settings
# Baidu Analytics
ba_track_id: xxxxxxxxxxxxxxxxxxxxxxxx

# Google Analytics
ga_track_id: 'xxxxxxxxxxxxx'            # Format: UA-xxxxxx-xx
ga_domain: auto
```

#### 2.11 好友

```
friends: [
    {
        title: "简书·happymiki",
        href: "http://www.jianshu.com/u/e71990ada2fd"
    },{
        title: "CSDN",
        href: "http://my.csdn.net/"
    }
]
```

#### 2.12 保存
讲网页拉倒底部，点击 `Commit changes` 提交保存

![](http://upload-
![15.预览.png](http://upload-images.jianshu.io/upload_images/6757403-d878785326b5607e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)images.jianshu.io/upload_images/6757403-c8dbaa331618f939.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

再次进入你的主页，

![](http://upload-images.jianshu.io/upload_images/6757403-cf0b6792676084cd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

恭喜你，你的个人博客搭建完成了😀。

# 三、写文章

利用 Github网站 ，我们可以不用学习[git](https://git-scm.com/)，就可以轻松管理自己的博客

对于轻车熟路的程序猿来说，使用git管理会更加方便。。。

### 3.1 创建
文章统一放在网站根目录下的 `_posts` 的文件夹中。

![](http://upload-images.jianshu.io/upload_images/6757403-dec2a7bea1801e3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建一个文件

![](http://upload-images.jianshu.io/upload_images/6757403-a09d49b2b11908cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在下面写文章，和标题，还能实时预览，最后提交保存就能看到自己的新文章了。

![](http://upload-images.jianshu.io/upload_images/
![15.预览.png](http://upload-images.jianshu.io/upload_images/6757403-b79de745ed18a287.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)6757403-6418dbe1db4fadc4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.2 格式

每一篇文章文件命名采用的是`2017-02-04-Hello-2017.md`时间+标题的形式，空格用`-`替换连接。

文件的格式是 `.md` 的 [**MarkDown**](http://sspai.com/25137/) 文件。

我们的博客文章格式采用是 **MarkDown**+ **YAML** 的方式。

[**YAML**](http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt) 就是我们配置 `_config`文件用的语言。

[**MarkDown**](http://sspai.com/25137/) 是一种轻量级的「标记语言」，很简单。[花半个小时看一下](http://sspai.com/25137)就能熟练使用了

大概就是这么一个结构。

```
---
layout:     post                    # 使用的布局（不需要改）
title:      My First Post               # 标题 
subtitle:   Hello World, Hello Blog #副标题
date:       2017-02-06              # 时间
author:     BY                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 生活
---

## Hey
>这是我的第一篇博客。

进入你的博客主页，新的文章将会出现在你的主页上.
```

按格式创建文章后，提交保存。进入你的博客主页，新的文章将会出现在你的主页上.

![](http://upload-images.jianshu.io/upload_images/6757403-0d1ea5f61809d6bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.3 首页标签

在首页可以看到这些特色标签，当你的文章出现相同标签（默认相同的**标签数量大于1**），才会自动生成。

所以当你只放一篇文章的时候是不会出现标签的。

建站的初期，博客比较少，若你想直接在首页生成比较多的标签。你可以在 `_congfig.yml`中找到这段：

```
# Featured Tags
featured-tags: true                     # 是否使用首页标签
featured-condition-size: 1              # 相同标签数量大于这个数，才会出现在首页
```

将其修改为`featured-condition-size: 0`, 这样只有一个标签时也会出现在首页了。

相反，当你博客比较多，标签也很多时，这时你就需要改回 `1` 甚至是 `2` 了。


到这里，恭喜你！

你已经成功搭建了自己的个人博客以及学会在博客上撰写文字的技能了（是不是有点小激动🙈）。


# 四、自定义域名

搭建好博客之后 你可能不想直接使用 [baiyingqiu.github.io](http://baiyingqiu.github.io) 这么长的博客域名吧, 想换成想 [qiubaiying.top](http://qiubaiying.top) 这样简短的域名。那我们开始吧！

### 4.1 购买域名
首先，你必须购买一个自己的域名。

我是在[阿里云](https://wanwang.aliyun.com/domain/?spm=5176.8006371.1007.dnetcndomain.q1ys4x)购买的域名

![](http://upload-images.jianshu.io/upload_images/6757403-a52d87e45af9b889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用**阿里云** app也可以注册域名，域名的价格根据后缀的不同和域名的长度而分，比如我这个 `happymiki.top` 的域名第一年才只要3元~

域名尽量选择短一点比较好记住，注意，不能选择中文域名，比如 `张三.top` ,GitHub Pages **无法处理中文域名**，会导致你的域名在你的主页上使用。

注册的步骤就不在介绍了

###  4.2 解析域名

注册好域名后，需要将域名解析到你的博客上

管理控制台 → 域名与网站（万网） → 域名

![](http://upload-images.jianshu.io/upload_images/6757403-ea62cb97e6db8ac9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择你注册好的域名，点击解析

![](http://upload-images.jianshu.io/upload_images/6757403-b33f0eed5a5ddcc5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加解析

分别添加两个`A` 记录类型,

一个主机记录为 `www`,代表可以解析 `www.qiubaiying.top`的域名

另一个为 `@`, 代表 `qiubaiying.top`

记录值就是我们博客的IP地址，是 GitHub Pagas 在美国的服务器的地址 `151.101.100.133`

![](http://upload-images.jianshu.io/upload_images/6757403-8150244331d94476.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可以通过 [这个网站](http://ip.chinaz.com/)  或者直接在终端输入`ping 你的地址`，查看博客的IP

    ping qiubaiying.github.io

细心地你会发现所有人的博客都解析到 `151.101.100.133` 这个IP。

然后 GitHub Pages 再通过 CNAME记录 跳转到你的主页上。


### 4.3 修改CNAME

最后一步，只需要修改 我们github仓库下的 **CNAME** 文件。

选择 **CNAME** 文件

![](http://upload-images.jianshu.io/upload_images/6757403-c584610c509151d0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![19.应用界面.png](http://upload-images.jianshu.io/upload_images/6757403-3f3a0a12c56daf92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
使用的注册的域名进行替换,然后提交保存

![](http://upload-images.jianshu.io/upload_images/6757403-ffc4598409f821be.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时，输入你自己的域名，就可以解析到你的主页了。

大功告成！

# 五、进阶

若你对博客模板进行修改，你就要看看 Jekyll 的[开发文档](http://jekyll.com.cn),是中文文档哦，对英语不好的朋友简直是福利啊（比如说我😀）。

还要学习 **Git** 和 **GitHub** 的工作机制了及使用。

你可以先看看这个[git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)，对git有个初步的了解后，那么相信你就能将自己图片传到GitHub仓库上，或者可以说掌握了 **使用git管理自己的GitHub仓库** 的技能呢。

对于轻车熟路的程序猿来说，这篇教程就算就结束了，因为下面的内容对于你们来说 so eazy~

但相信很多小白都一脸懵逼，那我们继续👇。

# 六、利用GithHub Desktop管理GitHub仓库

[GithHub Desktop](https://desktop.github.com/) 是 **GithHub** 推出的一款管理GitHub仓库的桌面软件，换句话说就是将你在**Github**上的文件同步到本地电脑上，并将修改后的文件同步到**Github**远程仓库。

### 6.1 下载

点击图片进入下载页面，选择对应的平台进行下载

![](http://upload-imag
![24.仓库的变化.png](http://upload-images.jianshu.io/upload_images/6757403-c223932a6624e20c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)es.jianshu.io/upload_images/6757403-ad40b78777d46892.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面以**Windows**平台为例：

### 6.2 安装

将下载好的文件直接点击安装，就可以在桌面找到图标

![](http://upload-images.jianshu.io/upload_images/6757403-ad89731b834cd3e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6.3 登录

点开应用,会弹出这个界面，

![](http://upload-images.jianshu.io/upload_images/6757403-a8c117b97f999e81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击file->options,输入你的**GitHub**账号和密码进行登录

![](http://upload-images.jianshu.io/upload_images/6757403-d4515a043612ab65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 6.4 克隆仓库

选择你的仓库克隆到本地，填写github上的URL，进行克隆。

![](http://upload-images.jianshu.io/upload_images/6757403-2577c97705704bd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6.5 管理仓库

现在文件夹中打开，使用Sublime Text打开文件夹

打开后你会的发现文件结构和Github上的一模一样~

![](http://upload-images.jianshu.io/upload_images/6757403-9a54fef0c2354ce0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你最先关心的可能是你的头像~在**img**文件夹中把替换我的头像就好了。

![](http://upload-images.jianshu.io/upload_images/6757403-679558b18c97df8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不仅是图片，所有在Github上的的操作都可以进行。

### 6.6 保存修改

当你对仓库文件夹的文件下进行修改、添加或删除时，都可以在 **GitHub Desktop** 中看到

例如我在 `img` 中添加了一张图片 `avatar-demo.png` 添加了一张图片

就可以在看到**GitHub Desktop**显示了我的修改

保存修改只要按 **Commit to master**，然后可以写上你的修改说明

![](http://upload-images.jianshu.io/upload_images/6757403-6175551fd2cc70aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6.7 同步

将修改同步到 **GitHub** 远程仓库上只需要一步：点击右上角的**同步按钮**

![](http://upload-images.jianshu.io/upload_images/6757403-75b5cca3b31c0f53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6.8 完成

打开你的GitHub上的仓库，你就可以看到已经和本地同步了

可以看到你提交的详情

这样，你已经能轻松管理自己的博客了。

想上传头像，背景，或者是删掉你不要的图片（我的头像😏）已经是 so eazy了吧~

### 6.9 注意
你在 **GitHub** 网站上进行 **Commit** 操作后，需要在**GitHub Desktop**上按一下 **同步按键** 才能同步网站上的修改到你的本地。


# 七、常见问题

最近有很多人给我提问题，我这边总结一下

### 7.1 配置文件修改后没有效果
刷新几遍浏览器就好了~

不行的话，先清除浏览器缓存再试试。

### 7.2 404错误

1. 检查你的仓库名是否有按照要求填写
2. 确定 **Fork** 的是不是我的仓库~

### 7.3 修改CNAME文件，域名还是不变

清除浏览器缓存就OK~

### 7.4 其他问题

直接在评论中提出来或私信我，我会一一替大家解决的😀


#  八、 在本地调试博客

有心的同学在 [jekyll官网](http://jekyllcn.com/) 就会发现 `jekyll` 的 提供的实例代码。

```
~ $ gem install jekyll bundler
~ $ jekyll new my-awesome-site
~ $ cd my-awesome-site
~/my-awesome-site $ bundle install
~/my-awesome-site $ bundle exec jekyll serve
# => 打开浏览器 http://localhost:4000
```


这段命令创建了一个默认的 `jekll` 网站，然后在本机的 4000 窗口展示。聪明的你应该发现怎么做了吧~

安装 `jekyll`和 `jekyll bundler`

```
$ gem install jekyll
$ gem install jekyll bundler
```

进入你的 **Blog 所在目录**，然后创建本地服务器

```
$ jekyll s

```

然后会显示 

```
 Auto-regeneration: enabled for '/Users/baiying/Blog'
Configuration file: /Users/baiying/Blog/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

你就可以在 <http://127.0.0.1:4000/> 看到你的博客，你对本地博客的修改都会在这个地址进行显示，这大大提高了对博客的配置效率。

使用`ctrl+c`就可以停止 **serve**

# *********************Star**************************

若本教程顺利帮你搭建了自己的个人博客，请不要 **害羞**，给我的 [github仓库](https://github.com/qiubaiying/qiubaiying.github.io) 点个 **star** 吧！

### **别无他求，点个 [Star](https://github.com/happymiki/happymiki.github.io) 吧**！

![](http://upload-images.jianshu.io/upload_images/6757403-39078b1012317965.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**心满意足！**