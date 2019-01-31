# Git 记录

## 1. 常用命令行

#### 1.1 初始设置

```
git config --global user.name "<用户名>" #设置用户名
git config --global user.email "<电子邮件>" #设置电子邮件
```

#### 1.2 本地操作

```
git init #初始化本地仓库
git add [-i] #保存更新到暂存区，-i为逐个确认。
git status #检查更新。
git commit [-a] -m "<更新说明>" #提交更新，-a为包含内容修改和增删，-m为说明信息，也可以使用 -am。
git log #查看提交日志
git diff #查看详细修改内容
git show #显示某次提交的内容
```

#### 1.3 远端操作

```
git clone  #克隆到本地。
git fetch #远端抓取。
git merge #与本地当前分支合并。
git pull [<远端别名>] [<远端branch>] #抓取并合并,相当于第2、3步
git push [-f] [<远端别名>] [<远端branch>] #推送到远端，-f为强制覆盖
git remote add <别名>  #设置远端别名
git remote [-v] #列出远端，-v为详细信息
git remote show <远端别名> #查看远端信息
git remote rename <远端别名> <新远端别名> #重命名远端
git remote rm <远端别名> #删除远端
git remote update [<远端别名>] #更新分支列表
```

#### 1.4 分支相关

```
git branch [-r] [-a] #列出分支，-r远端 ,-a全部
git branch <分支名> #新建分支
git branch -b <分支名> #新建并切换分支
git branch -d <分支名> #删除分支
git checkout <分支名> #切换到分支
git checkout -b <本地branch> [-t <远端别名>/<远端分支>] #-b新建本地分支并切换到分支, -t绑定远端分支
git merge <分支名> #合并某分支到当前分支
```

#### 待续……