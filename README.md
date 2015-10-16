#easyops

----

运维相关的工具

##easyssh

一个自动ssh登录的脚本工具，免去每一次都要输入密码的烦恼 

	easyssh ip [root] [username] [password]

* ip：目的主机的ip
* root：是否以root身份登录，任何非空字符串都会认识需要以root登录
* username：用户名，如果不传采用脚本中写好的用户名
* password：密码，如果不传采用脚本中写好的密码

**注意：脚本开头的两个变量*username*和*password*在使用时赋值成相应的用户名和密码**

关于退出:在命令行输入zzzz即可快速退出

----

##easylogin

easylogin是一个从指定配置目录读取服务器列表，让用户选择登录ip的工具

依赖easyssh工具

###配置文件

配置文件位于$HOME/.easylogin/iplist处（由basedir决定）

以目录为层级结构，位于目录深处的文件中含有服务器列表，此文件每一行为一台服务器

一行中以|分隔开两个字段，前一个字段为ip或者域名，第二个字段为注释

###使用

	easylogin [-r] [-s regex] [p1 [p2 ...]]
	
	-r         以root用户登录服务器
	-s regex   搜索服务器列表中匹配的行(grep)
	p1,p2...   目录的层级，或者是文件名，这里的顺序为目录的层级，从配置目录的根目录开始
	
###例子

目前手头上有2个项目，分别是a项目和b项目，a项目有两个集群，分别是ac1和ac2，b项目下有两个子项目sub1和sub2，sub1有三个集群sub1c1、sub1c2、sub1c3，sub2有两个集群sub2c1、sub2c2。

以上这种情况下，可以创建如下的目录层级结构
	
	~/.easylogin/iplist
	|-- a
	|   |-- ac1
	|   `-- ac2
	`-- b
	    |-- sub1
	    |      |-- sub1c1
	    |      |-- sub1c2
	    |      `-- sub1c2
	    `-- sub2
	           |-- sub2c1
	           `-- sub2c2
	
其中带有c的文件名为保存服务器列表的文件

比如集群sub1c2中有以下服务器

	1.1.1.1 nginx服务器
	2.2.2.2 mysql服务器
	3.3.3.3 redis服务器

则配置文件~/.easylogin/iplist/b/sub1/sub1c2的内容为

	1.1.1.1|nginx服务器
	2.2.2.2|mysql服务器
	3.3.3.3|redis服务器

配置完成后，执行**easylogin**则列出a、b两个项目，按提示输入数字或者直接输入项目名，则到第二级选择，依次选取

如果明确需要列出b项目下的sub1下的所有集群，则可以用如下命令
	
	easylogin b sub1

如果需要搜索b项目下的所有nginx服务器，则运行

	easylogin -s "nginx服务器" b

----

##importxshell

importxshell是将xshell的配置文件导入到easylogin的配置文件中

###使用
	
	importxshell *xshell/session/dir*

**如果对应的文件夹下既有文件又有目录，则文件里的Ip列表会存入_文件中**

