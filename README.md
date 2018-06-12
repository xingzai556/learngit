初始化设置全局变量信息:
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

一、创建版本库
首先，选择一个合适的地方，创建一个空目录：
$ cd /d/appData //进入d盘的appData文件夹
$ mkdir gitRepo //在进入的文件夹内创建子文件夹gitRepo
$ cd gitRepo //进入建立好的子文件夹gitRepo
$ pwd        //显示当前进入的文件夹是什么
/d/appData/gitRepo
##pwd命令用于显示当前目录
然后，通过git init命令把这个目录变成Git可以管理的仓库：
$ git init   //对创建好的仓库进行初始化
Initialized empty Git repository in /d/appData/gitRepo/.git/

二、把文件添加到本地仓库中。

$ git add index.txt //添加文件，仓库文件夹内需要已经存在文件index.txt
$ git commit -m "add index.txt" //提交文件并添加注释
## add 命令把文件提交到缓存区
## commit 把缓存区文件提交到版本库，-m 参数是指定comments
## 可以add多次，一次commit

三、查看状态
$ git status
该命令查看到的结果分为两部分：
一，add到缓存区内等待被commit到版本库的更改。
二，工作区内的还未add到缓存区的更改

四、查看工作区的更改内容
$ git diff index.txt
$ git diff HEAD -- index.txt
$ git diff --cached index.txt
$ git diff 287d9bd -- index.txt

## 1、git diff：工作区和缓存区比较
## 2、git diff --cached：缓存区和HEAD比较
## 3、git diff HEAD：工作区和HEAD比较
## 4、git diff commitID：工作区和某一版本的比较

五、查看commit历史
$ git log
commit 287d9bd301f8aa18d638021926ae690b6ba35507
Author: rchm <rchm8519@sina.com>
Date:   Sat Apr 25 16:50:58 2015 +0800

    5 查看工作区更改内容

commit 53468841ca9e73786567772efbbaafdfe6a30482
Author: rchm <rchm8519@sina.com>
Date:   Sat Apr 25 16:22:49 2015 +0800

    add index.txt

commit 17a85a95ef019058f04d320a157ef5a218d069f2
Author: rchm <rchm8519@sina.com>
Date:   Sat Apr 25 15:58:17 2015 +0800

    add of files
## 其中 287d9b... 一串字符 叫做 commit_id 版本号
## 如果信息太多，想要显示简洁一写，可以试试加上--pretty=oneline参数：
$ git log --pretty=oneline
287d9bd301f8aa18d638021926ae690b6ba35507 5 查看工作区更改内容
53468841ca9e73786567772efbbaafdfe6a30482 add index.txt
17a85a95ef019058f04d320a157ef5a218d069f2 add of files

六、版本回退
$ git reset --hard HEAD^
## git中用HEAD表示当前版本，HEAD^表示上一版本，HEAD^^表示上一版本
    HEAD~100表示上100个版本
## 或者直接指定版本号（不用全输，只许前面几位即可）

$ git reset --hard 534688
## 如果要单独回退某一个文件，可在最后加上文件名称，但不能加 --hard参数
$ git reset HEAD^ index.txt

当使用reset命令回退到以前的版本后，发现回退多了，或者想撤销回退操作
那只能使用指定版本号的方式了
可是git的版本号这么变态，谁能记得住？好吧，git帮你记！
查看版本号的命令：
$ git reflog
6163ac9 HEAD@{0}: reset: moving to 6163ac
287d9bd HEAD@{1}: reset: moving to HEAD~2
6163ac9 HEAD@{2}: commit: ##
186103f HEAD@{3}: commit: 7 版本回退”
287d9bd HEAD@{4}: commit: 5 查看工作区更改内容
5346884 HEAD@{5}: commit: add index.txt

七、废弃工作区修改

## 单个文件废弃
$ git checkout -- index.txt
## 整个工程废弃
$ git checkout -f
## 这个命令会把你工作区中的修改回退到最后一次add命令之前的状态
## 即如果缓存区有内容，则回退到和缓存区一直
## 如果缓存区为空，则回退到和版本库一致

八、把缓存区内容撤回工作区
$ git reset HEAD index.txt
## 该命令的执行不会使工作区中新的更改丢失

九、删除文件找回
$ git rm index.txt
## 该命令执行后，工作区内文件直接删除，操作指令放到缓存区
## 若执行commit，则版本库中文件被删除
## 若想取消删除，则需要先执行reset HEAD命令，再执行checkout命令找回
## 若执行commit，从版本库删除后，还想找回被删文件，这时HEAD版本中已经没了
则需要先执行reset HEAD^ 命令，从上一个版本号回退，再执行checkout命令找回

十、远程仓库
GitHub是一个提供Git仓库托管服务的网站
参考文档：http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000
一、注册GitHub网站
二、创建SSH Key
    $ ssh-keygen -t rsa -C "youremail@example.com"
    ##命令执行成功后，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，
    ##这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
三、设置SSH Key
    登陆GitHub，打开“Account settings”，“SSH Keys”页面：
    然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容；

在GitHub上创建一个新的仓库后，可以把本地仓库的内容推送到GitHub仓库
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
推送过一次后直接用  git push
$ git pull[remote] [branch] 拉取远程仓库的数据

首先你要知道一个远程仓库的地址，然后
$ git clone git@server-name:path/repo-name.git
##Git支持多种协议，默认的git://使用ssh，也可以使用https等其他协议，
##但通过ssh支持的原生git协议速度最快


十一、分支
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

