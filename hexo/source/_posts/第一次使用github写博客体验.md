## 欢迎来到小强博客 ##

**闲聊-------** 首先我先吐槽一下：今天三个室友跑出去了，自己没钥匙锁门，把自己关家里一天了，唉，就此打住吧，说多了都是累。

### 说一说我使用hexo搭建github博客时遇到的坑吧 ###

**第一步**

首先我们得准备资料：1、注册github。2、下载git。3、下载node.js。好了基本就这些基本资料


**第二步**

1.注册github很简单，百度输入github，点击注册，需要邮箱。

2.下载git [(downloads)](https://git-scm.com/downloads)

3.下载node.js [(下载)]()

4.上面的工具都准备好后，我们就可以操作了，首先，我们打开刚刚安装好的**Git bash**，当然，我们第一次打开是要登录的，就是要登录在github上注册的账号。现在就可以操作了，输入下面的命令：

**npm install -g hexo **
就能够安装hexo

5.现在我们到我们电脑的任意一个盘下面建立保存我们写博客的地方如：F:/Bolgs，在这个文件下右击选择git Bash，打开对话框后输入hexo init

6.安装依赖包输入命令 ： npm install

7.当命令执行完成后，我们就可以基本上完成了一半的工程了。然后输入命令：hexo generate
  这个命令主要目的当你再本地操作文件是对文件的保存，现在我们可以先享受一下我们本地搭建的bolg了，输入命令： hexo server 启动服务器，然后再游览器中输入localhost:4000 就可以看见我们本地搭建的bolg了。


**第三部**

1.先在核心的来了，首先在github官网上建立我们的Create a new repository，登录github官网
点击那个[+]号，选择Create a new repository ,首先在创建这里我必须强调一下，

Repository name 必须是你github的账号名如：sudeqiangxx.github.io这是我在搭建时遇到的坑，如果你想尝试一下，我不拦你，我们都在不断的跳坑中成长。完成这一步之后

2.现在到最关键的时候了，现在到我们创建的F:/Bolgs下的_config.yml，编辑这个文件，找到文件最末尾处的

	deploy:

  	type: git

  	repository: https://github.com/sudeqiangxx/sudeqiangxx.github.io.git
  
	branch: master

修改repository，把sudeqiangxx/sudeqiangxx.github.io.git,修改成：用户名/用户名.github.io.git 好了这一步完成后我们基本完成了本次hexo的搭建

3.别忘了保存，输入hexo generate ,

4.输入命令部署 hexo deploy

5.打开游览器输入http://用户名.github.io 就可以看见我们的bolg了，是不是感觉很有意思，必须有意思啊。


#hexo 搭建三部曲已完成，如有写得不是很清楚的地方，请到csdn博客进行讨论[csdn博客访问地址](http://blog.csdn.net/smallsmallrookie/article/details/51290925)#


**下周更新安卓弹框**
##欢迎各位游客来访，本人博客会每周都更新##

    




	
