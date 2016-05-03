#Git - GitHub常用技巧
######整理者：hauk0101
>1.如何在GitHub账号中创建新工程目录，并在本地克隆对应的目录？

步骤1：在GitHub下使用New repository创建新的工程目录，并设置需要的目录名称。<br>
步骤2：在本地GitHub仓库中，打开Git命令行窗口，输入如下指令即可：<br>

	git clone git@github.com:账号名/工程目录名.git

>2.如何快速在已存在于GitHub中的工程目录中添加新的文件夹或文件？

步骤1：在本地工程目录需要添加文件夹或文件的目录下，打开Git命令行窗口，输入如下指令：<br>
	
	git add 文件夹名或文件.后缀名

步骤2：提交至本地仓库,输入如下指令：<br>

	git commit -m "相关备注"

步骤3：提交到远程GitHub对应的工程目录中，输入如下指令：<br>

	git push 工程名 master
	