#### 初始化git
```
git config --global user.name "name"
git config --global user.email "email"
```

#### 创建仓库test_git
```
mkdir ~/test_git
cd ~/test_git
git init
```

#### 在test_git中创建一个文件README.md，输入"README file"
```
touch README.md
vim README.md
输入"README file"
保存文件
```

#### 查看当前工作区的状态
```
git status
```

#### 将README.md添加到缓存区中
```
git add README.md
```

#### 查看当前工作区的状态
```
git status
```

#### 将README.md从缓存区删除
```
git rm --cached README.md
```

#### 查看当前工作区的状态
```
git status
```

#### 将README.md添加到缓存区，然后提交全部提交到master分支中，提交信息为"add README.md"
```
git add README.md
git commit -m "add README.md"
```

#### 查看当前工作区的状态
```
git status
```

#### 修改 README.md 内容为 "update README file"
```
vim README.md
更改内容为"update README file"
```

#### 查看当前工作区状态以及有哪些修改
```
git status
git diff
```

#### 添加修改到暂存区，并提交到master，信息为"update README.md"
```
git add README.md
git commit -m "update README.md"
```

#### 查看README.md的内容
```
less README.md
```

#### 查看历史提交记录
```
git log
```

#### 返回第一个版本的 README.md，并查看历史提交记录
```
git reset --hard HEAD^ 
或者
git reset --hard 版本号
```

#### 查看README.md的内容
```
less README.md
```

#### 查看全局历史提交命令
```
git reflog
```

#### 回到最新版本的 README.md
```
由git reflog查找到最后一个commmit的版本号
git reset --hard 版本号
```

#### 查看README.md文件内容是否改变
```
less README.md
```

#### 创建本机的ssh秘钥
```
ssh-keygen -t rsa -C "Email"
```

#### 将秘钥添加到github账户中
```
进入setting中配置即可
```

#### 将test_hit上传到Github
```
在Github中新建空的仓库
git remote add origin git@...
git push -u origin master
```

#### 从Github中将项目下载到本地
```
先初始化一个新的本地仓库
git clone git@...
```

#### 分支可以类比为什么，并想象出相应的图
```
分支可以类比成平行世界
```

#### 查看所有分支
```
git branch
```

#### 创建新分支dev
```
git branch dev
```

#### 查看所有分支
```
git branch
```

#### 切换到dev分支
```
git checkout dev
```

#### 查看所有分支
```
git branch
```

#### 创建新的文件new.txt，内容为new file
```
touch new.txt
vim new.txt
编辑为new file
```

#### 将修改添加到暂存区并提交
```
git checkout dev
git add new.txt
git commit -m "update"
```

#### 切换为master分支，将dev分支合并到当前分支
```
git checkout master
git merge dev
```

#### 删除dev分支
```
git branch -d dev
```

#### 查看所有分支
```
git branch
```

#### 创建新的空文件.gitignore，添加到暂存区并提交
```
touch .gitignore
git add .gitignore
git commit -m "update"
```

#### 创建ig.txt文件和igdir/igdirig.txt，并查看当前状态
```
touch ig.txt
mkdir igdir
cd igdir
touch igdirig.txt
cd ..
git status
```

#### 忽略ig.txt和igdir/
```
vim .gitignore
添加内容ig.txt
```

#### 查看当前状态
```
git status
```

#### 强制将ig.txt和igdir/添加到暂存区，并查看状态
```
git add -f ig.txt igdir/
git status
```

#### 查看所有标签
```
git tag
```

#### 为当前版本打上标签"version1.0"
```
git tag version1.0
```

#### 查看所有标签
```
git tag
```

#### 新建文件newnew.txt添加提交，并查看当前目录下有什么文件和目录，打上标签version2.0
```
touch newnew.txt
ls
```

#### 切换到version1.0，查看当前目录下有什么文件和目录
```
git checkout version1.0
ls
```

#### 切换到version2.0，查看当前目录下有什么文件和目录
```
git checkout version1.0
ls
```

#### 删除标签version 1.0
```
git tag -d version1.0
```

#### 查看所有标签
```
git tag
```