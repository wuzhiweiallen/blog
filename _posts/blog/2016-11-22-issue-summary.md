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

*  java中对于String的判空：
   判断一个字符串是否为空首先就要确保他不是null然后再判断他的长度。

   ```
   String str = xxx; 
   if(str != null && str.length() != 0) { 
   return true; 
   }
   ```
   1.  直观，效率低   
   ```
   if(s == null || s.equals("")){
   }
   ```
   2.  比较字符串长度, 效率高。
   ```
   if(s == null || s.length() <= 0);
   }
   ```
   3.  Java SE 6.0才开始提供的方法,效率和方法二几乎相等
   ```
   if(s == null || s.isEmpty());
   } 
   ```

* js中判断为空：
   1.  判断undefined: 
   ```
   if (typeof(tmp) == "undefined"){ 
   alert("undefined");
   ```
 说明：typeof 返回的是字符串，有六种可能："number"、"string"、"boolean"、"object"、"function"、"undefined" 
   2.  判断null: 
   ```
   if (!tmp && typeof(tmp)!="undefined" && tmp!=0){ 
   alert("null"); 
   ```
   3. 判断NaN:
   ```
   var tmp = 0/0; 
   if(isNaN(tmp)){ 
   alert("NaN");  
   ```
    说明：如果把 NaN 与任何值（包括其自身）相比得到的结果均是 false，所以要判断某个值是否是 NaN，不能使用 == 或 === 运算符。 
    提示：isNaN() 函数通常用于检测 parseFloat() 和 parseInt() 的结果，以判断它们表示的是否是合法的数字。当然也可以用 isNaN() 函数来检测算数错误，比如用 0 作除数的情况。  
   4.  判断undefined和null:  
   ```
   var tmp = undefined; 
   if (tmp== undefined) 
   { 
   alert("null or undefined"); 
   ```
说明：null==undefined 
   5.  判断undefined、null与NaN: 
   ```
   var tmp = null; 
   if (!tmp) 
   { 
   alert("null or undefined or NaN"); 
   ```
提示：一般不那么区分就使用这个足够。
*  bug描述：页面上drop down list控制旁边图片的变化。（在一个div中），当这个div的height超出1000，就angularjs的指令会把它收缩起来。但是
   判断高度的代码$('#tab-height').height()要等图片加载好才能获得正确的高度。问题就来了。怎样确保图片加载好再执行呢？我使用$timeout，可以指定等待加载的时间
   但是这个图片的加载跟网速有关的。有时可能会比较慢。$timeout也不能确保。我使用image的onload方法。出入图片的url，但是其他页面也用了这个指令的，不能每次都要传入图片的url撒。
   用angularjs的$interval 时间设置100(ms),let images = $('#tab-height img'); 循环图片  直到img.complete  再加上if (typeof myimage.naturalWidth != "undefined" && img.naturalWidth == 0) （图片完全加载好了才能判断的）
   双管齐下确保图片加载好。
	```
	if(isLoaded){
		callback();//调用闭包函数 就是获得图片高度的代码 还有验证逻辑啥的
		$interval.cancel(timer);//关闭interval
	}//双击666
	```
*  记一次提交代码出现的幺蛾子，到处都是坑啊！！！改好代码之后我熟练地add commit。再review的时候出现这个幺蛾子：
![commitError](/blog/images/commitError.png)
   要先git fetch一下。取下远程的分支的改动。再review就可以了！（想不通这里，我在commit之后明明pull --rebase过，为啥还要再去git fetch一下。）
   还有一次，页面的效果和QA的不一样，但是明明感觉我们的代码都应该是一样的啊，也是pull过master上的代码了。后面git fetch了一下后再pull，有没得问题了！
*  修改commit的message：
   不需要有代码的改动，直接commit --amend，按Insert键进去编辑模式。改好后Esc键回到命令模式。:wq保存并退出。:q!只退出不保存！
*  JQuery实现动态的回滚到表单验证出错的地方。
	假如我们需要去定位一个动态生成的div，我们需要为它指定一个动态的id,例如:
	前台使用EL进行迭代LIST生成div，为其添加动态的id,生成之后变成下面样式
	我们在使用Jquery获取某个div时需要这样写
	
	$("#" + 所定义的id变量名);
	而不能写成这样
	$("#所定义的id变量名");
    ```
            function invalidDOB(isInvalidDOB) {
                for(let i in isInvalidDOB){
				//获取input前面label标签到页面顶部的距离
                    let topPos = $("#"+"DOBLabel_"+i).offset().top;
                    if(isInvalidDOB[i]){
					//屏幕滚动到label标签
                        window.scrollTo(0, topPos);
                        return false;
                    }
                }
                return true;
            }	
    ```
	 