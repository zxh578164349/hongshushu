---
layout: post
title: Jekyll Blog
category: jekyll
tags: [jekyll]
---

## 申请 Github 账号

登录 [Github](https://github.com/),使用个人邮箱申请一个 Github 帐号

![]({{ site.baseurl }}/files/images/20160519/github_index.png)

## 新建一个 repository

点击右上角的加号

![]({{ site.baseurl }}/files/images/20160519/create.png)  
填写 repository 名称,点击 Create repository  

![]({{ site.baseurl }}/files/images/20160519/info.png)

## 创建 repository 主页

![]({{ site.baseurl }}/files/images/20160519/create index.png)

## 选择喜欢的模版

![]({{ site.baseurl }}/files/images/20160519/select layout.png)

***
***

**以上内容只是说明如何在 Github 上创建一个 repository 以及创建它的主页 index ,这只是准备工作,后面会用到**

***
***

## 使用git创建本地仓库

git 是一个版本控制器，在这里不作详细介绍。我们主要通过它来上传代码，上传内容。

### 首先，新建一个放内容的文件夹

{% highlight ruby %}
 mkdir hongshushu
{% endhighlight %}
### 把这个文件夹变成 git 可管理的仓库

{% highlight ruby %}
git init
{% endhighlight %}
### **<font style="color:red">(PS:补充创建一个没有父节点的分支gh-pages )</font>**

{% highlight ruby %}
git checkout --orphan gh-pages
{% endhighlight %}
### 在这个仓库中新建几个主机的文件夹，配置文件 *_config.yml*，首页 *index.html*

{% highlight ruby %}
mkdir _layouts  //存放模板
mkdir _posts     //存放文章内容
{% endhighlight %}
### 在_layouts中创建模板文件 default.html ，以后的创建的网页都可以用到它

![]({{ site.baseurl }}/files/images/20160519/default.png)
*(PS: 由于Jekyll直接把代码中 Liquid 标记编译出来，为有截图处理)*

### 在_posts文件夹中添加博客内容，可以是markdown,html文件

{% highlight ruby %}
  ---
  layout: default
  title: 第一次使用 Jekyll
  ---
  Jekyll 建博客，简洁美观大方，很棒！
{% endhighlight %}
### 在根目录下创建首页 index.html

![]({{ site.baseurl }}/files/images/20160519/index.png)

*(PS: 由于Jekyll直接把代码中 Liquid 标记编译出来，为有截图处理)*

### 把以上所有操作提交到本地缓存区

{% highlight ruby %}
git add .
{% endhighlight %}
### 提交到版本库

{% highlight ruby %}
git commit -m "ok"
{% endhighlight %}
### 建立远程仓库连接

{% highlight ruby %}
git remote add origin git@github.com:zxh578164349/hongshushu.git
{% endhighlight %}

### 最后，提交

{% highlight ruby %}
git push -u origin gh-pages
{% endhighlight %}


