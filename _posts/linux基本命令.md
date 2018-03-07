title: linux学习(ubuntu)
date: 1/4/2016 9:39:04 PM 
tags: linux
---
## cd命令 ##
用于切换当前目录，参数是要切换的目录的路径，可以是绝对路径，也可以是相对路径

    cd /root/Docements # 切换到目录 /root/Documents
    cd ./path          # 切换到当前目录下的path目录中 .表示当前目录
    cd ../path         # 切换到上层目录中的path目录中 ..表示上一层目录

## ls命令 ##

    
    1. -l ：列出长数据串，包含文件的属性与权限数据等 
    2. -a ：列出全部的文件，连同隐藏文件（开头为.的文件）一起列出来（常用）
    3. -d ：仅列出目录本身，而不是列出目录的文件数据 
    4. -h ：将文件容量以较易读的方式（GB，kB等）列出来
    5. -R ：连同子目录的内容一起列出（递归列出），等于该目录下的所有文件都会显示出来  
    
    上面的命令也可以组合使用
    1. ls -l #以长数据串的形式列出当前目录下的数据文件和目录 
    2. ls -lR #以长数据串的形式列出当前目录下的所有文件  

## 文件命令 ##

 创建一文件夹的话可以用mkdir 命令

	mkdir /tmp/test

创建文件
	touch /tmp/a.txt
 
## 删除命令 ##

	rm -f /var/log/httpd/access.log
 将会强制删除/var/log/httpd/access.log这个文件 ，-f 就是直接强行删除

	rm -rf /var/log/httpd/access
 将会删除/var/log/httpd/access目录以及其下所有文件、文件夹

 **注意**

 使用这个rm -rf的时候一定要格外小心，linux没有回收站的 

## 关机 ##

    shutdown -h now 
    halt 
    halt -p
	-r 表示重启
	-h 表示关机
	halt命令的"-p" 选项表示终止系统后中断电源（需主板硬件支持）

## 重启 ##

    shutdown -r now 
    shutdown -r +15 系统将于15分钟后重启
	reboot