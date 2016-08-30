---
layout: post
title: Apache+Tomcat集群
category: apache tomcat
tags: [apache tomcat]
---

## tomcat多个实例  

1 在tomcat安装文件夹中，建立一个文件夹instances用于存放实例,例如tomcat1与tomcat2,分别建立文件夹tomcat1,tomcat2  

2 复制tomcat中的conf文件夹放置在tomcat1与tomcat2,修改conf中的server.xml配置文件，修改3处端口  

![]({{ site.baseurl }}/files/images/20160830/111.png) 
![]({{ site.baseurl }}/files/images/20160830/222.png)   
![]({{ site.baseurl }}/files/images/20160830/001.png)  

  

{% highlight css %}
   input[type=text],input[type=password],select{width:200px,border:1px.....省略}
{% endhighlight %}
  

這是一箇屬性選擇器，它會覆蓋 class="pin_form"這箇類選擇器的樣式，修改全局樣式不現實，所以要另謀方法  

## 方法

如果想在單獨一箇表單頁面上使用一箇特別的CSS樣式的話，必須要使用能否覆蓋屬性選擇器的樣式或選擇器：  

1.內聯樣式  
2.ID選擇器  
3.僞類選擇器  

但是，表單元素較多，以上選擇器適合使用，所以隻剩以下選擇器：  

1.屬性選擇器  
2.類選擇器  
3.元素選擇器  
4.通用選擇器  

但是，由於屬性選擇器會覆蓋它后面的選擇器，所以最後隻能夠用屬性選擇器  

可是，如何覆蓋全局類選擇器樣式，而又不影響其它頁面的樣式呢？  

方法是，找到全局樣式所在css文件中，在文件的最后加上你要的樣式，寫在一箇類選擇器中，如下  

{% highlight css %}
  input[class='pinfo_form']{
  display: block;
  width: 100%;
  height: 34px;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857143;
  color: #555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 4px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075);
          box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075);
  -webkit-transition: border-color ease-in-out .15s, -webkit-box-shadow ease-in-out .15s;
       -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
          transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
}
{% endhighlight %}  

爲什麼可以覆蓋呢？因爲后面的樣式可以覆蓋間前面的樣式   


input[class='pinfo_form']對應form表單中 class="pinfo_form"元素  


















