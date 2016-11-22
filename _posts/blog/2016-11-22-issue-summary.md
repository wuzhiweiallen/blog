---
layout: post
title: 工作遇到的程序方面的问题的汇总
category: blog
---

## 经常遇见的问题的总结

* ng-click其实同事执行表达式和函数的，中间用分号隔开就好。我相信会帮助很多。

    ng-click="cruiseResults.sortData = s;cruiseResults.doSort(s.value)"


* 用chrome调试的时候老是找不到要调试的js文件，这个就很郁闷了。其实可以在如下图的localhot:8080处点击右键 然后点击 Add folder to workspace 手动把要调试的js文件加进去，然后找到它就可以去debug了
![addToWorkspace](/blog/images/addToWorkspace.png)