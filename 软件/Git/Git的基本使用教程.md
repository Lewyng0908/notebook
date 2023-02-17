# Git的基本使用教程

## 1.Git的必要配置

相关命令：

查看配置
```shell
git config -l
```

![image-20230217152821632](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217152821632.png)

查看不同级别的配置文件：
```shell
# 查看系统config
git config --system --list

# 查看当前用户(global)配置
git config --global --list
```

**Git相关的配置文件：**

1）、Git\etc\gitconfig  ：Git 安装目录下的 gitconfig   --system 系统级

2）、C:\Users\Administrator\ .gitconfig   只适用于当前登录用户的配置  --global 全局

![image-20230217153253055](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217153253055.png)

可以直接编辑配置文件，通过命令修改后会响应到这里。



## 设置用户名和密码（用户标识，必要操作）

每次使用Git提交都会使用当前用户的标识信息，意在记录是哪一个用户提交文件。

```shell
git config --global user.name "xxxxxx"  #名称
git config --global user.email 123456@asd.com   #邮箱
```

邮箱不一定要真实存在，使用真实邮箱最好

而如果希望在一个特定的项目中使用不同的名称或e-mail地址，你可以在该项目中运行该命令而不要--global选项。总之--global为全局配置，不加为某个项目的特定配置。



## 2.Git的基本理论

Git在本地有三个工作区域

- 工作区(Working Directory)
- 暂存区(Stage)
- 本地仓库(Repository/Git Directory)

加上远程的Git仓库(Remote Directory)就会有四个工作区域

而这些区域的作用：

- Workspace：工作区，平时项目代码存放的目录
- Index/Stage：暂存区，临时存放改动，只是一个文件，保存的是文件列表信息
- Repository：本地仓库，安全存放数据的位置，在这里有提交过的所有版本的数据。HEAD指向最新存放进仓库的版本‘
- Remote：远程仓库，托管代码的服务器，一般会使用GitHub/Gitee



## 3.工作流程

git的工作流程一般是这样的：

１、在工作目录中添加、修改文件；（文件变为已修改状态`modified`）

２、将需要进行版本管理的文件放入暂存区域；（文件变为已暂存状态`staged`）

３、将暂存区域的文件提交到git仓库。（文件变为已提交状态`committed`）



## 4.Git项目搭建

通常会将自己的项目通过Git来上传至远程仓库进行管理，因此我们需要使用一个公钥来在上传仓库这一操作时实现免密码登录。

> GIT为什么要配置公钥和私钥

Git使用https协议，每次pull, push都要输入密码，会十分的繁琐
使用git协议，然后使用ssh密钥。这样可以省去每次都输密码。

### 生成公钥

打开终端，输入
```shell
ssh-keygen -t rsa -C "user.email"
```

![image-20230217163844182](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217163844182.png)

此时我们的公钥就已经生成完了，接下来即可将公钥添加至远程仓库的账户当中。

打开公钥文件

![image-20230217170025026](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217170025026.png)

复制其中的内容

![image-20230217170117809](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217170117809.png)

然后依次打开`Github网站` -> `Settings` -> `SSH And GPG Keys`

点击`New SSH Key`按钮

添加`Title`和`Key`

![Snipaste_2023-02-17_17-04-28](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/Snipaste_2023-02-17_17-04-28.png)

可以测试一下是否连接上

git终端输入

```shell
ssh -T git@github.com
```

![image-20230217170839568](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217170839568.png)

这样即为连接成功



### 本地仓库与远程仓库对接

有两种方案

> 第一种：先创建本地仓库，然后再与远程仓库进行连接

创建一个文件夹，名字与远程仓库的名称同步

打开Git Bash后输入

```shell
git init
```

![image-20230217165517206](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217165517206.png)

然后创建一个远程仓库

[如何创建Github仓库]()

将仓库的路径复制下来

![image-20230217171323730.png](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217171323730.png)

返回终端输入

```shell
git remote add origin git@github.com:yourName/repositoryname.git #后面即为SSH路径
git remote add origin https://github.com/yourName/repositoryname.git #HTTP路径
```

![image-20230217171929413](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217171929413.png)

做完这一步就相当于是已经连接成功了，可以从远程仓库拉取文件测试一下。

![image-20230217172040214](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217172040214.png)

远程仓库的main分支当中有一个README.md文件

终端中输入

```shell
git pull origin "分支名" # 分支名中主分支默认为main
```

![image-20230217172301681](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217172301681.png)

运行完之后，本地文件夹当中出现了远程仓库当中的文件。



> 第二种：先创建远程仓库，然后直接将远程仓库克隆到本地仓库

选择一个项目存放的位置，无需创建文件夹

直接在终端中输入

```shell
git clone git@github.com:yourName/repositoryname.git # 后面为仓库的SSH路径
```

![image-20230217173004877](https://lewyngnotebookimage.oss-cn-shenzhen.aliyuncs.com/img/img_notebook/image-20230217173004877.png)

打开目录即可看到克隆成功。



## 5.本地仓库同步到远程仓库

```shell
git add filename
git commit -m ""
git push origin develop(远程分支名称)
```



[详细教程](https://mp.weixin.qq.com/s/Bf7uVhGiu47uOELjmC5uXQ)
