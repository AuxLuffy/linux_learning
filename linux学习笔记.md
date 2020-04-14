linux学习笔记
linux的目录结构：
home:普通用户的家目录文件在home下。例如：一个用户Tom在此系统上的目录就是Tom
root:超级管理员的根目录
etc:存放配置文件的目录
usr:存放共享文件的资源
mnt:系统管理员安装临时文件系统的安装点
var:用于存放运行时需要改变数据的文件

[hk@localhost ~]$ ls -a .bash*
.bash_history  .bash_logout  .bash_profile  .bashrc

#  .bash_history  记录了当前用户执行过的历史命令
#  .bash_logout   表示退出当前Shell时需要执行的命令
#  .bash_profile  表示登录当前Shell时需要执行的命令
#  .bashrc        表示每次打开新的Shell时需要执行的命令
.bash_profile 只在会话开始时被载入，而 .bashrc 在每次打开新的终端时都要被读取。一般为了统一设置，可以把所有设置都放进 .bashrc 。

以上这些文件是每一位用户的设置。系统级的设置存储在 /etc/profile、/etc/bashrc 及目录 /etc/profile.d 下的文件中，这些文件的编辑需要具备 root 权限，所以一般通过用户自己的环境变量定义文件设置属于该用户的环境变量。当系统与用户级的设置发生冲突时，将优先采用用户的设置。


linux文件权限：

文件权限(组)及数字对应关系：
	【文件或文件夹】 【owner权限】 【group权限】 【others权限】
	【文件是-文件夹是d】 【r/w/x相加】 【r/w/x相加】 【r/w/x相加】
	r 代表读，数字 4
	w 代表写，数字 2
	x 代表可执行，数字 1
	示例：
		d rwx  rwx  rwx  =777  表示目录的操作权限
		-  rwx  rwx  rwx = 777  表示文件的操作权限
	没有权限的用"-"表示, eg:
		- rwx r-x rw- =756	表示所有者对其有读/写/执行的权限，组群对其有读/执行权限，其他人对它有读/写的权限
	修改权限命令：
		chmod o w a.txt		授予其他人(o)对文件(a.txt)写(w)的权限
		chmod go-rw a.txt 	删除(-)组群和其他人(go)对文件(a.txt)读写(rw)的权限
		chmod -R 700 a/b	对a/b文件夹及其下所有文件增删权限(只有自己有所有权限，组群和其他人没有任何权限)
	其中：
		u 代表所有者
		g 代表所有者所在的群组
		o 代表其他人，但不是u和g
		a 代表全部的人，包括u,g和o
修改用文件所有者
 	chown root a.txt
修改文件所属组
	chgrp root a.txt
同时修改所属组和所有者
	chown root:root a.txt
	


linux常用命令：
	切换目录：cd
		cd app	切换到app目录
		cd app/d	切换到app/d目录
		cd .. 	上一级目录
		cd / 	所有用户的根目录
		cd ~	切换到用户主目录
		cd -	切换到上一个所在目录

	创建目录和删除目录
		mkdir	创建目录
			mkdir test	创建test目录
				man mkdir	查看mkdir命令的格式, 由此可查看命令的参数格式等
			mkdir -p test/1/2/3		如果不存在中间的文件夹会自动创建
		rmdir	删除目录
			rmdir test	删除test目录，只能删除一个空目录
	展示目录下文件列表
		ls
			ls	展示的能看见的文件(和目录)的名称
			ls -a 	展示所有的文件的名称(以.开头的表示隐藏的文件)
			ls -l 	以列表的形式展示所有的文件的详细信息
			ls -i 	显示文件的id
			ls -h 	人性化显示文件大小，如2M100K
			ls -lh	友好的展示所有文件的详细信息
			ll		同ls -l
			ll -h 	同ls -lh



	浏览文件
		cat:显示文件所有内容
			cat config.ini	显示config.ini的内容
		more:分页显示	
			空格显示下一页	回车显示下一行
		less:分页显示
			pgdn/pgup 	上下翻页
		tail(*):查看文件的最后的内容
			tail -2 config.ini 	查看config.ini的后两行
			tail -f config.ini 	动态的查看config.ini(通过ctrl+c来停止)

nl命令
nl命令用来计算文件中行号。nl可以将输出的文件内容自动加上行号！其默认的结果与cat -n有点不太一样，nl可以将行号进行比较多的显示设计，包括位数是否自动补齐0等等的功能 
命令格式：
nl [选项]... [文件]...
参数
	-b:	指定行号计算方式，主要有两种
		-b a:	不论是否为空行都显示行号，类似cat -n
		-b t:	空行的话不显示行号（默认此方式）
	-n:	列出行号显示的格式，主要有三种：
		-n ln:	行号显示在最左侧
		-n rn:	行号显示在最右侧，且不加0（默认）
		-n rz:	行号显示在自己栏位的最右方显示，且加0
	-w:	指定行号占用的位数
	-p:	在逻辑界定符处不重新开始计算
	