#Git - GitHub常用技巧
######整理者：hauk0101
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

其中第一条指令表示将GitHub中的工程与本地的工程相关联，<br>第二条指令中的“-u”不但会把本地的master分支内容推送到远程新的master分支，同时还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化指令。