---
layout: post
title: Web Service簡單實例
category: Web Service 
tags: [Web Service]
---
## 建立Web Service項目  



1.建立一箇 Web Service 項目,名字爲：WebService  

![]({{ site.baseurl }}/files/images/Web Service/001.png) 

2.建立一箇 Web Service 類,名字爲：Service  

{% highlight ruby %}
  @WebService
public class Service{

	/**
	 * @Title: main
	 * @Description: 供客戶調用的方法
	 * @param @param args
	 * @return void
	 * @throws
	 * @author web
	 * @date 2016/10/12
	 */
	public String getValue(String name){
		return "我的名字叫："+name;
	}
	public static void main(String[] args){
		// TODO Auto-generated method stub
		Endpoint.publish("http://172.17.18.173:8080/WebService/Serviec",new Service());
		System.out.println("Service success!");

	}

}
{% endhighlight %}  

3.開啓 Web Service 服務  

![]({{ site.baseurl }}/files/images/Web Service/002.png)  


4.在瀏覽器輸入以下 <font style="font-color:red;font-style:italic">http://172.17.18.173:8080/WebService/Serviec?wsdl</font>  測試結果,如下：  

 ![]({{ site.baseurl }}/files/images/Web Service/003.png) 











