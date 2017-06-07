# 常用Windows系统小技巧汇总
###### 整理者：hauk0101
## 说明

本文档主要收集了在windows系统中一些常见的小技巧

>1. 查看某个端口是否被占用，以及被那个程序/进程占用。

解决方法如下：
	
进入cmd命令窗口，分别输入以下指令，得到相应数据，

* 查询端口PORT是否被占用
	
		cmd->netstat -ano|findstr PORT

* 利用上述步骤的PID（进程标识符）查看占用端口的对应程序/进程，或者配合任务管理器查看

		cmd->tasklist|findstr PID

* 终止对应PID的进程,xxx为具体的PID数值，或使用任务管理器进行终止

		taskkill /T /F /PID XXX
