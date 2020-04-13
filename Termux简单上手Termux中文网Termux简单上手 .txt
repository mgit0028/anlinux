04.13 15:30
Termux简单上手Termux中文网
Termux简单上手
Posted on  2018年8月10日 by hujinhai
Termux的安装
1. 更新包
apt update
apt upgrade
2. 修改源
export EDITOR=vi
apt edit-sources
在vi编辑器里把第二行替换成以下内容（清华镜像源）

deb [arch=all,arm] http://mirrors.tuna.tsinghua.edu.cn/termux stable main
在vi编辑器里，输入第一个i进入编辑模式（插入），下面会给出提示“INSERT”，此时再打字就是在光标左边插入字符。等修改完毕后，按住音量上，同时输入e，即可退出编辑模式。再输入:wq保存并退出vi。

使用Termux
1. 管理员身份
默认是没有管理员权限的，在执行一些敏感操作时会提示Permission denied，或者在cd到一些目录时会提示无此文件夹。
输入su进入管理员身份，第一次进入时仍会提示Permission denied，但此时Termux已经申请了获取手机的root权限，进入安全中心的root权限管理，给Termux通过即可。此时再次su就可以成功了。

在管理员身份下，输入exit可回到普通用户身份。
但Termux环境的根目录是/data/data/com.termux/files，而在su后的PATH环境变量是/sbin/su:/system/bin，很多命令就用不了。

但按照上个网页的说法，tsu命令修复了PATH变量，但需要先安装。

# 安装tsu
pkg install tsu
2. 终止程序运行
我们都知道用Ctrl+C终止程序，在Termux中，需要使用音量下+c。

 

文章导航
Termux基础命令
发表评论
