
### 基本操作

- 提交代码流程
```git
git add .
git commit -m "提交内容备注"
git pull
git push

# 查看仓库当前的状态，显示有变更的文件
git status
```


### 分支管理

- 查看分支
```git
# 查看远程所有分支
git branch -a
# 查看当前本地分支
git branch
# 查看本地分支与远程分支的关联关系
git branch -vv
```


- 新建分支

```git
# 创建新分支：方法1（先建一个新分支+切换到新的分支）
git branch [newBranchName]
git checkout [newBranchName]

# 创建新分支：方法2（新建分支并切换到该新分支）
git checkout -b [newBranchName]
```

- 将新分支推送到远程仓库
```git
git push

# 如果有报错提示没有设置上游的远程仓库，只要按照提示执行即可。
# $ git push
# fatal: The current branch download-api has no upstream branch.
# To push the current branch and set the remote as upstream, use
# 
#     git push --set-upstream origin newBranchName

git push --set-upstream origin newBranchName
```

- 切换到远程分支
```git
# 查看远程所有分支
git branch -a
# 从远程拷贝分支到本地新分支
git checkout -b [newBranchName] origin/[branchName]
# 如果不需要重新起名字，还是直接用远程的名字，可以直接checkout
git checkout branchName


# 假设切换到远程的 dev-1.2.1 分支
# 从远程拷贝分支到本地新分支
git checkout -b dev-1.2.1 origin/dev-1.2.1
# 然后可以查看当前分支与远程分支是否是相关联的
git branch -vv
```

- 强制切换分支
```git
# 强制切换分支
# 即放弃本地修改，强制切换，用于丢弃本地更改
git checkout -f newBranchName
```

- 暂无判断
```git
# 将本地新增加的分支添加到远程服务器
git push origin [newBranchName]
```

- 合并分支
```git
# 将 A分支的提交记录合并到 B分支上
# 先处于B分支的节点上，然后合并A分支
git pull
git checkout B
git pull
git merge A
git push
```


### 仓库管理

- 显示所有远程仓库
```git
git remote -v
```

- 重新关联远程仓库地址
```git
git remote set-url origin [url]
```

- 将当前修改的内容提交到新的分支上
```git
##步骤1：在当前的有内容被修改的分支上，将修改暂存起来
git stash

##步骤2：暂存修改后，在本地新建分支（newBranchName为新分支的名字）
git checkout -b [newBranchName]

##步骤3：将暂存的修改放到新建分支中
git stash pop

##步骤4：使用命令进行常规的add、commit步骤
git add.
git commit -a "修改内容"

##步骤5：将提交的内容push到远程服务器(在远程也同步新建分支develop_backup)
git push origin [newBranchName]

##步骤6：将当前分支与远程分支关联
git branch --set-upstream-to=origin/[newBranchName]
```


#### git将当前分支上修改的东西转移到新建分支


##### 第一种方法
我们不需要在A分支做commit,只需要在A分支新建B分支，然后切换过去。这个时候你会发现修改的东西在A，B分支都有。这个时候在B分支commit，那么这些修改保留在B分支上，再切换到A分支上会发现修改都没有保留下来。


##### 第二种方法
使用git stash 将A分支暂存起来，然后在某一个分支（如master分支）新建一个分支B，然后在B分支上使用git stash pop 将修改弹出到B分支上，然后这些修改就在B分支上了。