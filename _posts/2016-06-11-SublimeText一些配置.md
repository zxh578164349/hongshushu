---
layout: post
title: Sublime Text一些配置
category: sublime
tags: [sublime]
---

##关闭升级提示
进入【Preferences】--【Settings-user】添加如下  

{% highlight ruby %}
 {
    update_check:false
 }
{% endhighlight %}

##安装package control组件
按【Ctrl+`】调出console,并复制如下代码
{% highlight ruby %}
   import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
{% endhighlight %}
