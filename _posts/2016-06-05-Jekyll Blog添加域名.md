---
layout: post
title: Jekyll Blog 绑定个人域名
category: jekyll
tags: [jekyll,域名]
---

## GoDaddy申请域名
* __登录[GoDaddy](https://sg.godaddy.com/)申请个人域名__
* __申请成功后，要求填写跳转的网站，也就是你自己的网站__
* ![]({{ site.baseurl }}/files/images/godaddy/01.png)
* __Gighub项目中添加一个不带扩展名的文件:CNAME__
* __内容：上面申请到的域名（例如我的：hongshushu.xyz）__
* __注意，最后还要修改相对应的url和baseurl参数__
