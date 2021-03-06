# Git - GitHub常用技巧
###### 整理者：hauk0101

>1.如何在GitHub账号中创建新工程目录，并在本地克隆对应的目录？

步骤1：在GitHub下使用New repository创建新的工程目录，并设置需要的目录名称。<br>
步骤2：在本地GitHub仓库中，打开Git命令行窗口，输入如下指令即可：<br>

	git clone git@github.com:账号名/工程目录名.git

>2.如何快速在已存在于GitHub中的工程目录中添加新的文件夹或文件？

步骤1：在本地工程目录需要添加文件夹或文件的目录下，打开Git命令行窗口，输入如下指令：<br>
	
	git add 文件夹名或文件.后缀名
_tip: 如果只是创建了一个空的文件夹，git是无法识别的，需要在空的文件夹中添加任意文件后，git add 文件夹名才会生效_

步骤2：提交至本地仓库,输入如下指令：<br>

	git commit -m "相关备注"

步骤3：提交到远程GitHub对应的工程目录中，输入如下指令：<br>

	git push 工程名 master
	
>3.在将本地的文件上传至GitHub中时，需要注意确认GitHub已经和本地的Git相关联，否则无法正常push,初次push时，可以通过如下指令进行关联：<br>
	
	git remote add 工程名 git@github.com:账号名/本地工程名.git

	git push -u 工程名 master

_tip:其中第一条指令表示将GitHub中的工程与本地的工程相关联，<br>第二条指令中的“-u”不但会把本地的master分支内容推送到远程新的master分支，同时还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化指令。_

>4.删除某个文件可以使用rm命令：

	git rm 文件/文件夹名称

_也可以使用“rm 文件/文件夹名称 ”命令来删除，但这样的操作仅仅是对文件进行了物理操作，不会被git记录下来，因此也不会在	git commit 操作时将删除操作提交上去，所以需要慎用。_


>5.关于更新修改后的文件可以使用如下命令：

	//为方面，以下都用"."标识路径，"."表示当前目录（可省略），路径可以修改。
	//表示将已被git系统跟踪的文件中执行过修改操作的文件和未跟踪新添加的文件将添加到git的暂存区，但不包括删除操作
	git add .
	
	//-u == --update ,表示将已跟踪文件中的修改和删除的文件添加到暂存区，不包括新增加的文件，注意这些被删除的文件被加入到暂存区，再被提交并推送到服务器的版本库之后，这个文件就会从git系统中消失了。
	git add -u .

	//-A == --all ,表示将所有的已跟踪的文件的修改与删除和新增的未跟踪的文件都添加到暂存区。
	git add -A .

>6.如何删除GitHub中的仓库：

打开账户user下的需要删除的对应仓库A,在GitHub页面中，user/A下，找到"Code"同一行的"Settings",打开"Setting"页面后，找到页面下的"Delete this repository",点击后确认是否为你想要删除的仓库，确认后输入你想删除的仓库A，确认即可。
	