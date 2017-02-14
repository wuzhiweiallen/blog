---
layout: post
title: 工作遇到的程序方面的问题的汇总
category: blog
---

## 经常遇见的问题的总结

* ng-click其实可以同时执行表达式和函数的，中间用分号隔开就好。

    ng-click="cruiseResults.sortData = s;cruiseResults.doSort(s.value)"


* 用chrome调试的时候老是找不到要调试的js文件，这个就很郁闷了。其实可以在如下图的localhot:8080处点击右键 然后点击 Add folder to workspace 手动把要调试的js文件加进去，然后找到它就可以去debug了
![addToWorkspace](/blog/images/addToWorkspace.png)

* parseInt ( cString )从字符串cString非空字符开始转换得到的整数,遇到小数点或其他0-9外的字符就停止，如“-1234a”,“-1234.0”都将返回 -1234；如果除第一个符号外一个0-9字符都不是，将返回NaN,如“-a”、“abc”等；
Number( cString )从字符串cString转换得到的数字，包括Int和Float类型，如：“-123”返回-123，“123”和“00123”都返回 123，“234。56”返回234。56等。cString必须是合法的数字串，否则返回NaN；如“.123”、“1.23.45”、 “--123”、“123a"都返回NaN.在js中转换变量为整型的时候最好用Number.

* java中对于String的判空：
  判断一个字符串是否为空，首先就要确保他不是null，然后再判断他的长度。 
	 String str = xxx; 
	 if(str != null && str.length() != 0) { 
	 return true; }
1，直观，效率低
	 if(s == null || s.equals("")){
	 }
2，比较字符串长度, 效率高。
	 if(s == null || s.length() <= 0);
     }
3，Java SE 6.0 才开始提供的方法, 效率和方法二几乎相等
	 if(s == null || s.isEmpty());
	 } 
	 