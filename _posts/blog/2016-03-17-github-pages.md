---
layout: post
title: 使用jekyll+markdown+github+git搭建个人博客
description: Github本身就是不错的代码社区，他也提供了一些其他的服务，比如Github Pages，使用它可以很方便的建立自己的独立博客，并且免费。
category: blog
---

机器环境:Win7 64位 

## 前言

一直想尝试搭建个自己的博客，自己写觉得太麻烦了，在网上去下载别人的项目，经常会遇到jdk版本啊，以及各种包的问题，总之是真的很恼火，后来在同事的建议下试着用jekyll+markdown+github+git来做。发现真TM是个神器。网页是静态的，你不用去考虑数据库，空间，域名等问题，在本地用markdown工具编辑好在push到你的github上就可以啊。巴适得板。下面是搭建过程。

## 安装rubyinstaller.

到http://rubyinstaller.org/downloads/下载ruby安装文件，这里下载rubyinstaller-2.2.3-x64.exe，按照提示安装，勾选Add Ruby executables to your PATH.



Win7 64位默认安装位置：C:\Ruby22-x64.

验证ruby是否安装成功：cmd中
ruby -v
显示ruby版本号说明ruby安装成功.

安装rubygems.

官网下载安装包: [https://rubygems.org/pages/download](https://rubygems.org/pages/download)

![installRuby](/blog/images/installRuby.png)

解压rubygems-2.4.8.zip到指定目录，为了方便管理解压后放到C:\Ruby22-x64\目录下.

cmd进入rubygems-2.4.8目录下(快捷键：打开C:\Ruby22-x64\rubygems-2.4.8目录，shift+鼠标右键，点击”在此处打开命令行窗口”),运行
    ruby setup.rb
.cmd 中
    gem -v
显示版本号则说明正常.

安装DevKit-mingw64

下载相应版本[http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/),在C:\Ruby22-x64\目录下新建DevKit文件夹，运行DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe后会提示解压目录，选择C:\Ruby22-x64\DevKit.

在C:\Ruby22-x64\DevKit中打开cmd，运行
    ruby dk.rb init
，会提示配置config.yml.

打开C:\Ruby22-x64\DevKit目录下的config.yml，将ruby根目录加入到配置文件中，这里是C:/Ruby22-x64.如果有了就不需要再加.注意格式.
![config](/blog/images/config.png)

执行

    ruby dk.rb install.

替换rubyGem库地址（相当重要，因为国内访问外网有线路问题，不仅更新速度慢，而且还会导致更新失败）

    gem sources –remove https://rubygems.org/

    gem sources -a https://ruby.taobao.org/ (注意：一定是https，淘宝已暂停http的ruby服务)

    gem sources -l

验证一下. 

![sources](/blog/images/sources.png)

安装rails

cmd运行
    gem install rails.

cmd运行rails -v显示rails版本号说明安装成功.

安装jekyll 

cmd运行

    gem install jekyll

cmd运行jekyll -v验证，显示版本号说明安装成功.

环境配置完整之后，下面进入正题，如何新建博客:

运行命令:jekyll new 文件夹名，比如jekyll new blog，会在当前目录生成blog文件夹.

![blog](/blog/images/blog.png)

在生成的blog文件夹根目录下运行命令:

    jekyll serve –-watch

浏览器中打开localhost:4000，命令运行过程中没有错误提示，浏览器中显示默认页面说明安装成功.
![jekyll](/blog/images/jekyll.png)


## 配置和使用Github
Git是版本管理的未来，他的优点我不再赘述，相关资料很多。推荐这本[Git中文教程][4]。

要使用Git，需要安装它的客户端，推荐在Linux下使用Git，会比较方便。Windows版的下载地址在这里：[http://code.google.com/p/msysgit/downloads/list](http://code.google.com/p/msysgit/downloads/list "Windows版Git客户端")。其他系统的安装也可以参考官方的[安装教程][5]。

下载安装客户端之后，各个系统的配置就类似了，我们使用windows作为例子，Linux和Mac与此类似。

在Windows下，打开Git Bash，其他系统下面则打开终端（Terminal）：
![Git Bash](/blog/images/githubpages/bootcamp_1_win_gitbash.jpg)

###1、检查SSH keys的设置
首先我们需要检查你电脑上现有的ssh key：

    $ cd ~/.ssh

如果显示“No such file or directory”，跳到第三步，否则继续。

###2、备份和移除原来的ssh key设置：
因为已经存在key文件，所以需要备份旧的数据并删除：

    $ ls
    config	id_rsa	id_rsa.pub	known_hosts
    $ mkdir key_backup
    $ cp id_rsa* key_backup
    $ rm id_rsa*

###3、生成新的SSH Key：
输入下面的代码，就可以生成新的key文件，我们只需要默认设置就好，所以当需要输入文件名的时候，回车就好。

    $ ssh-keygen -t rsa -C "邮件地址@youremail.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>

然后系统会要你输入加密串（[Passphrase][6]）：

    Enter passphrase (empty for no passphrase):<输入加密串>
    Enter same passphrase again:<再次输入加密串>

最后看到这样的界面，就成功设置ssh key了：
![ssh key success](/blog/images/githubpages/ssh-key-set.png)

###4、添加SSH Key到GitHub：
在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。

用文本编辑工具打开id_rsa.pub文件，如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。

在GitHub的主页上点击设置按钮：
![github account setting](/blog/images/githubpages/github-account-setting.png)

选择SSH Keys项，把复制的内容粘贴进去，然后点击Add Key按钮即可：
![set ssh keys](/blog/images/githubpages/bootcamp_1_ssh.jpg)

PS：如果需要配置多个GitHub账号，可以参看这个[多个github帐号的SSH key切换](http://omiga.org/blog/archives/2269)，不过需要提醒一下的是，如果你只是通过这篇文章中所述配置了Host，那么你多个账号下面的提交用户会是一个人，所以需要通过命令`git config --global --unset user.email`删除用户账户设置，在每一个repo下面使用`git config --local user.email '你的github邮箱@mail.com'` 命令单独设置用户账户信息

###5、测试一下
可以输入下面的命令，看看设置是否成功，`git@github.com`的部分不要修改：

    $ ssh -T git@github.com


如果是下面的反应：

    The authenticity of host 'github.com (207.97.227.239)' can't be established.
    RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
    Are you sure you want to continue connecting (yes/no)?


不要紧张，输入`yes`就好，然后会看到：

    Hi <em>username</em>! You've successfully authenticated, but GitHub does not provide shell access.

###6、设置你的账号信息
现在你已经可以通过SSH链接到GitHub了，还有一些个人信息需要完善的。

Git会根据用户的名字和邮箱来记录提交。GitHub也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。

    $ git config --global user.name "你的名字"
    $ git config --global user.email "your_email@youremail.com"

####设置GitHub的token

2012-4-28补充：新版的接口已经不需要配置token了，所以下面这段可以跳过了

有些工具没有通过SSH来链接GitHub。如果要使用这类工具，你需要找到然后设置你的API Token。

在GitHub上，你可以点击*Account Setting > Account Admin*：
![set ssh keys](/blog/images/githubpages/bootcamp_1_token.jpg)

然后在你的命令行中，输入下面的命令，把token添加进去：

    $ git config --global user.name "你的名字"
    $ git config --global user.token 0123456789your123456789token

如果你改了GitHub的密码，需要重新设置token。

###成功了
好了，你已经可以成功连接GitHub了。

## 使用GitHub Pages建立博客
与GitHub建立好链接之后，就可以方便的使用它提供的Pages服务，GitHub Pages分两种，一种是你的GitHub用户名建立的`username.github.io`这样的用户&组织页（站），另一种是依附项目的pages。

###User & Organization Pages
想建立个人博客是用的第一种，形如`beiyuu.github.io`这样的可访问的站，每个用户名下面只能建立一个，创建之后点击`Admin`进入项目管理，可以看到是这样的：
![user pages](/blog/images/githubpages/user-pages.png)
而普通的项目是这样的，即使你也是用的`othername.github.io`：
![other pages](/blog/images/githubpages/other-pages.png)

创建好`username.github.io`项目之后，提交一个`index.html`文件，然后`push`到GitHub的`master`分支（也就是普通意义上的主干）。第一次页面生效需要一些时间，大概10分钟左右。

生效之后，访问`username.github.io`就可以看到你上传的页面了，[beiyuu.github.com][7]就是一个例子。

关于第二种项目`pages`，简单提一下，他和用户pages使用的后台程序是同一套，只不过它的目的是项目的帮助文档等跟项目绑定的内容，所以需要在项目的`gh-pages`分支上去提交相应的文件，GitHub会自动帮你生成项目pages。具体的使用帮助可以参考[Github Pages][]的官方文档：

###绑定域名
我们在第一部分就提到了在DNS部分的设置，再来看在GitHub的配置，要想让`username.github.io`能通过你自己的域名来访问，需要在项目的根目录下新建一个名为`CNAME`的文件，文件内容形如：

    zhiweiwu.com

你也可以绑定在二级域名上：

    blog.zhiweiwu.com

需要提醒的一点是，如果你使用形如`zhiweiwu.com`这样的一级域名的话，需要在DNS处设置A记录到`207.97.227.245`（**这个地址会有变动，[这里][a-record]查看**），而不是在DNS处设置为CNAME的形式，否则可能会对其他服务（比如email）造成影响。

设置成功后，根据DNS的情况，最长可能需要一天才能生效，耐心等待吧。

##Jekyll模板系统
GitHub Pages为了提供对HTML内容的支持，选择了[Jekyll][]作为模板系统，Jekyll是一个强大的静态模板系统，作为个人博客使用，基本上可以满足要求，也能保持管理的方便，你可以查看[Jekyll官方文档][8]。

你可以直接fork[我的项目][11]，然后改名，就有了你自己的满足Jekyll要求的文档了，当然你也可以按照下面的介绍自己创建。

###Jekyll基本结构
Jekyll的核心其实就是一个文本的转换引擎，用你最喜欢的标记语言写文档，可以是Markdown、Textile或者HTML等等，再通过`layout`将文档拼装起来，根据你设置的URL规则来展现，这些都是通过严格的配置文件来定义，最终的产出就是web页面。

基本的Jekyll结构如下：

    |-- _config.yml
    |-- _includes
    |-- _layouts
    |   |-- default.html
    |   `-- post.html
    |-- _posts
    |   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
    |   `-- 2009-04-26-barcamp-boston-4-roundup.textile
    |-- _site
    `-- index.html


简单介绍一下他们的作用：
####_config.yml
配置文件，用来定义你想要的效果，设置之后就不用关心了。

####_includes
可以用来存放一些小的可复用的模块，方便通过`{ % include file.ext %}`（去掉前两个{中或者{与%中的空格，下同）灵活的调用。这条命令会调用_includes/file.ext文件。

####_layouts
这是模板文件存放的位置。模板需要通过[YAML front matter][9]来定义，后面会讲到，`{ { content }}`标记用来将数据插入到这些模板中来。

####_posts
你的动态内容，一般来说就是你的博客正文存放的文件夹。他的命名有严格的规定，必须是`2012-02-22-artical-title.MARKUP`这样的形式，MARKUP是你所使用标记语言的文件后缀名，根据_config.yml中设定的链接规则，可以根据你的文件名灵活调整，文章的日期和标记语言后缀与文章的标题的独立的。

####_site
这个是Jekyll生成的最终的文档，不用去关心。最好把他放在你的`.gitignore`文件中忽略它。

####其他文件夹
你可以创建任何的文件夹，在根目录下面也可以创建任何文件，假设你创建了`project`文件夹，下面有一个`github-pages.md`的文件，那么你就可以通过`yoursite.com/project/github-pages`访问的到，如果你是使用一级域名的话。文件后缀可以是`.html`或者`markdown`或者`textile`。这里还有很多的例子：[https://github.com/mojombo/jekyll/wiki/Sites](https://github.com/mojombo/jekyll/wiki/Sites)

###Jekyll的配置
Jekyll的配置写在_config.yml文件中，可配置项有很多，我们不去一一追究了，很多配置虽有用但是一般不需要去关心，[官方配置文档][10]有很详细的说明，确实需要了可以去这里查，我们主要说两个比较重要的东西，一个是`Permalink`，还有就是自定义项。

`Permalink`项用来定义你最终的文章链接是什么形式，他有下面几个变量：

* `year` 文件名中的年份
* `month` 文件名中的月份
* `day` 文件名中的日期
* `title` 文件名中的文章标题
* `categories` 文章的分类，如果文章没有分类，会忽略
* `i-month` 文件名中的除去前缀0的月份
* `i-day` 文件名中的除去前缀0的日期

看看最终的配置效果：

* `permalink: pretty` /2009/04/29/slap-chop/index.html
* `permalink: /:month-:day-:year/:title.html` /04-29-2009/slap-chop.html
* `permalink: /blog/:year/:month/:day/:title` /blog/2009/04/29/slap-chop/index.html

我使用的是：

* `permalink: /:title` /github-pages

自定义项的内容，例如我们定义了`title:BeiYuu的博客`这样一项，那么你就可以在文章中使用`{ { site.title }}`来引用这个变量了，非常方便定义些全局变量。

###YAML Front Matter和模板变量
对于使用YAML定义格式的文章，Jekyll会特别对待，他的格式要求比较严格，必须是这样的形式：

    ---
    layout: post
    title: Blogging Like a Hacker
    ---

前后的`---`不能省略，在这之间，你可以定一些你需要的变量，layout就是调用`_layouts`下面的某一个模板，他还有一些其他的变量可以使用：

* `permalink` 你可以对某一篇文章使用通用设置之外的永久链接
* `published` 可以单独设置某一篇文章是否需要发布
* `category` 设置文章的分类
* `tags` 设置文章的tag

上面的`title`就是自定义的内容，你也可以设置其他的内容，在文章中可以通过`{ { page.title }}`这样的形式调用。

模板变量，我们之前也涉及了不少了，还有其他需要的变量，可以参考官方的文档：[https://github.com/mojombo/jekyll/wiki/template-data](https://github.com/mojombo/jekyll/wiki/template-data "Jekyll Template Data")


## 多说评论系统

多说国内使用最多的评论系统。

在做以下步骤之前，先去 duoshuo.com 上注册一个帐号，获取 short_name（我的是wuzhiweiallen）

首先按照如下格式编辑 _config.yml

comments :
provider : duoshuo
duoshuo :
short_name : wuzhiweiallen

其次进入 _includes/ 目录创建目录 custom 以及在刚创建的 custom 目录下创建文件 duoshuo

$ cd _includes; mkdir custom; cd custom ; touch duoshuo

填充如下内容

<!-- Duoshuo Comment BEGIN -->
    <div id="comments">
        <div class="ds-thread" {% if page.id %}data-thread-key="{{ page.id }}"{% endif %}  data-title="{% if page.title %}{{ page.title }} - {% endif %}{{ site.title }}"></div>
    </div>
<!-- Duoshuo Comment END -->

在post.html中{{ content }}后面放置
	
	<div class="ds-thread" data-thread-key="1" data-title="create blog" data-url="http://wuzhiweiallen.github.io/blog/github-pages"></div>
    </div>
在body标签外放置下面的js代码

    <script type="text/javascript">
	var duoshuoQuery = {short_name:"wuzhiweiallen"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
    </script>

然后你会看到这样的效果表示你的博客引入多说成功了。

![duoshuoComment](/blog/images/duoshuoComment.png)

## github常用的命令

发布项目到github

    $ git init
    $ git checkout --orphan gh-pages
    $ git add .
    $ git commit -a -m "v0.0.1 first blood"
    $ git remote add origin https://github.com/(github用户名)/(jekyll项目名称).git
    $ git push origin gh-pages

修改项目

	$ git add .
	$ git commit -a -m "自己的提交注释"
	$ git push origin gh-pages

## 结语

上面的本人搭建博客的全部过程，希望对你有帮助，谢谢！。

如果有问题，也请在下面留言告诉我，再次感谢！




[Github]:   http://github.com "Github"
[jQuery]:   https://github.com/jquery/jquery "jQuery@github"
[Twitter]:  https://github.com/twitter/bootstrap "Twitter@github"
[Github Pages]: http://pages.github.com/ "Github Pages"
[Godaddy]:  http://www.godaddy.com/ "Godaddy"
[Jekyll]:   https://github.com/mojombo/jekyll "Jekyll"
[DNSPod]:   https://www.dnspod.cn/ "DNSPod"
[Disqus]: http://disqus.com/
[多说]: http://duoshuo.com/
[1]:    {{ page.url}}  ({{ page.title }})
[2]: http://markdown.tw/    "Markdown语法"
[3]:    http://baike.baidu.com/view/65575.htm "A记录"
[4]: http://progit.org/book/zh/ "Pro Git中文版"
[5]: http://help.github.com/mac-set-up-git/ "Mac下Git安装"
[6]: http://help.github.com/ssh-key-passphrases/
[7]: http://beiyuu.github.com
[8]: https://github.com/mojombo/jekyll/blob/master/README.textile
[9]: https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter
[10]: https://github.com/mojombo/jekyll/wiki/configuration
[11]: https://github.com/beiyuu/beiyuu.github.com
[12]: http://docs.disqus.com/developers/universal/
[13]: http://mihai.bazon.net/projects/javascript-syntax-highlighting-engine
[14]: http://code.google.com/p/google-code-prettify/
[15]: https://github.com/mojombo/jekyll/wiki/Install
[16]: https://rvm.io/rvm/install/
[17]: http://jekyllbootstrap.com/
[18]: http://chxt6896.github.com/blog/2012/02/13/blog-jekyll-native.html
[a-record]: https://help.github.com/articles/my-custom-domain-isn-t-working
