
@[TOC](Git工具安装配置)

## 关于工具
 - git官方手册：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)
 - 图解git：[http://marklodato.github.io/visual-git-guide/index-zh-cn.html](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)

## Git工具下载
下载地址：[https://git-scm.com/download/win](https://git-scm.com/download/win)

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-FbvJMjkk-1630410889484)(git_files/1.jpg)\]](https://img-blog.csdnimg.cn/ff780b56ef4a429e88677f84be3d3c42.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_20,color_FFFFFF,t_70,g_se,x_16)

## Git工具安装
 - 双击工具Git
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/358aa83c4dec4a0482b63a9768debb46.jpg)
 - 傻瓜式操作，一路**Next**

![在这里插入图片描述](https://img-blog.csdnimg.cn/728de6408a4644cfadc598468914f968.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_18,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/45b890d0545e405586beb53b4114add2.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_18,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/565be1d3e0ef49a399213cc92151e3cd.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_18,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e637c46284f04e6fa53f00b7f4eb2545.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_18,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/af05a60bbee14f6cb95feaa4a7f81311.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_18,color_FFFFFF,t_70,g_se,x_16)

 - 完成安装

![在这里插入图片描述](https://img-blog.csdnimg.cn/97e211272e55405298c06e620bea1ec7.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_18,color_FFFFFF,t_70,g_se,x_16)


## Git基本配置
 - 运行cmd命令行工具
 - 验证git安装 **git --version**
	  ```cs
	  C:\buerYouth>git --version
	  git version 2.33.0.windows.2
	  ```
 - 设置用户名 **git  config --global  user.name  'gitee的用户名'**
		 ```cs
		 C:\buerYouth>git config --global user.name '***'
		 ```
 - 设置用户邮箱 **git  config --global  user.email  'gitee绑定的邮箱'**
	 ```cs
	 C:\buerYouth>git config --global user.email '******@email address'
	 ```
 - 查看个人配置 **git config --global --list**
	 ```cs
	 user.name='***'
	 user.email='******@email address'
	 ```
 - 更多配置
	 ```cs
	# 在Vim编辑器中编辑Git配置文件	
	git config -e (--global)
	# git status等命令自动着色
	git config --global color.ui true 
	 ```
 ## Git基本操作
 - 初始化仓库

|命令|作用|
|:----|:----|
|git init	|初始化仓库。|
|git clone	|拷贝一份远程仓库，也就是下载一个项目。	|
```cs
# 在当前目录新建git仓库
git init
# 新建目录，并初始化为git仓库
git init (project-name)
# 克隆git仓库的项目
git clone <url>
```
 - 提交和修改操作

|命令|作用|
|:----|:----|
|git add	|添加文件到仓库。|
|git status	|查看仓库当前的状态，显示有变更的文件。	|
|git diff	|比较文件的不同，即暂存区和工作区的差异。|
|git commit	|提交暂存区到本地仓库。|
|git reset	|回退版本。|
|git rm		|删除工作区文件。	|
|git mv		|移动或重命名工作区文件。|
```cs
# 从工作区添加指定文件到暂存区
git add <file1> <file2> <file3>...
# 把当前目录的所有文件添加到暂存区
git add .
# 显示暂存区和工作区文件的差异
git diff <file>
# 显示暂存区和上一次提交的差异
git diff --cached <file>
# 把暂存区文件全部提交到仓库区
git commit -m [message]
# 从暂存区提交指定文件到仓库区
git commit <file1> <file2>...-m "message"
# 跳过暂存区，从工作区把文件提交到仓库区
git commit -a -m "message"
```
 - 远程操作

|命令|作用|
|:----|:----|
|git remote	|远程仓库操作。|
|git fetch	|从远程获取代码库	。|
|git pull	|下载远程代码并合并。|
|git push	|上传远程代码并合并。|

 - git完整命令手册：[http://git-scm.com/docs](http://git-scm.com/docs)

## Git查询表
![在这里插入图片描述](https://img-blog.csdnimg.cn/bbda21ffab3344f09aa2e8c986d453ea.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQnVlcllvdXRo,size_20,color_FFFFFF,t_70,g_se,x_16)


## 推荐工具
 - Sourcetree
 - TortoiseGit
