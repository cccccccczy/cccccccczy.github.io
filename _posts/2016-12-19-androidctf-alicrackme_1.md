---
layout: post
title:  "安卓 AliCrackme_1"
date:   2016-12-19 17:35:52
categories: CTF
excerpt: AliCrackme_1
---

* content
{:toc}

* 昨天晚上方轶给我们讲了两道安卓题，嗯...作为什么都不会的小白终于找到了一点感觉，感觉还是挺有收获的。感谢大佬~
    看到破解成功的那一刻感觉很爽hhhhhhhhh


​	![0x01](http://www.secpulse.com/wp-content/uploads/2015/04/0x01.jpg)

​	百度了一下找到了各种各样的解法，从看起来很复杂的到很简便的动态分析都有。看来方法也很重要呢。这次用的是adb的一些操作命令

​	//这是一个很小白很小白的wp

​	还没装好用的虚拟机，直接用了AndroidStudio。

​	打开里面的虚拟机，然后点开terminal，安装软件：输入 **adb install 路径**  回车显示success即安装成功。

​	使用jeb打开apk，找到MainActivity, 反编译得到Java源码。

​	![](http://images.cnitblog.com/blog/407791/201501/271214346597224.png)

​	并不是特别长，可以看它的具体逻辑来获取密码。

​	获得正确注册码的代码逻辑为：

​	1. 从logo.png这张图片的偏移89473处，读取一个映射表，768字节编码成UTF-8，即256个中文表

​	2. 从偏移91265处读取18个字节编码的UTF-8（即6个中文字符）为最终比较的密码。然后通过输入的字符的转换，转换规则就是ASCII字符编码，去比较是否和最终密码相等。
<br/>


#### 静态分析

​	抄一个网上的解密程序：

```java
 def getCodesFromPic():
      
      with open('logo.png','r') as f:
          v0 = f.read()
      return v0[89473:89473+768].decode('u8'),v0[91265:91265+18].decode('u8')       
  
  def aliCodeToBytes(codeTable,strCmd):
      pwd = ''
      for i in strCmd:
         pwd += chr(codeTable.find(i))
     return pwd
 
 if __name__=="__main__":
     table, pwdCode = getCodesFromPic()
     print table, pwdCode
     pwd = aliCodeToBytes(table, pwdCode)
     print pwd
```

​	运行得到的代码如下：

​	![](http://images.cnitblog.com/blog/407791/201501/271218242848009.png)

​	好吧......连Java都不是很熟的我目前还不会写这种Orz，所以还是用巧妙的动态调试吧！

<br/>

#### 动态分析	

 ![](http://ww3.sinaimg.cn/large/006pzljrgw1fawl5fe08yj30mc0bx78v.jpg)

​	仔细观察一下，这个日志带有标签“lil”，因此使用**adb logcat -s lil**过滤一下就可以得到日志了~

​	大概长成酱紫：

​	![](http://ww4.sinaimg.cn/large/006pzljrgw1fawl8tl27ij30fm0bi41w.jpg)

​	输入1234567890进行比对，发现enPassword中有对应的文字输出

​	1 - 么

​	2 - 广

​	3 - 亡

​	4 - 门

​	5 - 义

​	6 - 之

​	7 - 尸

​	8 - 弓

​	9 - 己

​	通过观察Logcat输出可知，最终目标pw应为*义弓么丸广之*，根据上述table中的对应关系，我们可以得到最终密码为：

```
581026
```



当然，绝大多数情况下还是要仔细的看smali代码，毕竟很多操作都是需要通过smali来进行，不可能每次都能用别人的log来猜测。然鹅我还不会Orz。。。任重而道远啊啊啊啊啊！

撒花~
