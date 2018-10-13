### 本地仓库初始化

* 命令：git init
* 注意：**.git**文件夹中存放git工作所需要的信息，不能删除和随意更改该文件夹



### 本地仓库状态查看

* 命令：git status



### 将文件添加到暂存区

* 命令：git add [filename]



### 将文件由暂存区添加到本地库

* 命令：git commit -m "信息" [filename]



### 将更改后的文件直接添加到本地库

* 命令：git commit -m "信息" -a
* 注意：必须是之前已经在本地库中的文件才能使用



### 版本快照与索引值

- git用快照的方式管理数据，每次提交时git都会对全部文件制作一个对应的快照，并用哈希算法(sha-1)生成唯一的标识值，称为索引号

- git中有个 HEAD 指针指向某个索引号，表示当前工作区中是哪一个快照(版本)

- 为了高效，如果文件没有被修改，git不再生成新的快照，而是保存一个链接，链接指向之前的快照

- 快照示意图

  ![](https://github.com/gothicrush/learning/blob/master/Git/03.git%E6%A0%B8%E5%BF%83%E6%93%8D%E4%BD%9C/images/%E7%89%88%E6%9C%AC%E5%BF%AB%E7%85%A7.PNG)



### 提交历史查看

| 命令                     | 特点                                     | 能否看见【未来】                           |
| ------------------------ | ---------------------------------------- | ------------------------------------------ |
| git log                  | 最详细，内容最多                         | 只能看到过去的记录，看不到【未来】的记录   |
| git log --pretty=oneline | 简略版，索引号完整                       | 只能看到过去的记录，看不到【未来】的记录   |
| git log --oneline        | 简略版，索引号只有开头                   | 只能看到过去的记录，看不到【未来】的记录   |
| git reflog               | 简略版，索引号只有开头，附带版本移动步数 | 既能看到过去的记录，又能看到【未来】的记录 |

- 【未来】的含义
  - 【未来】不是指实际物理时间的未来，而是指当前版本的未来
  - 比如现在有版本A->B->C，A最老，C最新，现在由版本C切换为B
  - 则命令1，2，3都只能看到记录A和B，看不到C
  - 而命令4能看到记录A，B，C



### 版本切换

- 三种版本切换方法

  - 索引号：git reset --hard|mixed|soft 索引号
  - HEAD^：git reset --hard|mixed|soft HEAD^
  - HEAD~：git reset --hard|mixed|soft HEAD~n

- 索引号使用

  - 首先通过提交历史命令查看快照对应的索引号
  - 再通过 git reset --hard|mixed|soft 索引号即可切换版本

- HEAD^使用

  - HEAD^只能回到以前的版本，一个^表示回退一个版本
  - git reset --hard|mixed|soft HEAD^：回退1个版本
  - git reset --hard|mixed|soft HEAD^^^：回退3个版本 

- HEAD~使用

  - HEAD~只能回到以前的版本，n表示回退n个版本
  - git reset --hard|mixed|soft HEAD~1：回退1个版本
  - git reset --hard|mixed|soft HEAD~3：回退3个版本

- hard|mixed|soft的说明

  - --soft：工作区移动，暂存区，本地库都不移动

    ![](https://github.com/gothicrush/learning/blob/master/Git/03.git%E6%A0%B8%E5%BF%83%E6%93%8D%E4%BD%9C/images/soft%E6%A8%A1%E5%BC%8F.PNG)

  - --mixed：工作区，暂存区都移动，本地库不移动

    ![](https://github.com/gothicrush/learning/blob/master/Git/03.git%E6%A0%B8%E5%BF%83%E6%93%8D%E4%BD%9C/images/mixed%E6%A8%A1%E5%BC%8F.PNG)

  - --hard：工作区，暂存区，本地库都移动

    ![](https://github.com/gothicrush/learning/blob/master/Git/03.git%E6%A0%B8%E5%BF%83%E6%93%8D%E4%BD%9C/images/hard%E6%A8%A1%E5%BC%8F.PNG)



### 文件删除与恢复

- 文件还没提交到暂存区和本地库，想删除
  - 直接删除，不涉及任何git操作
- 文件还没提交到暂存区和本地库，删除后想恢复
  - 没门，因为根本没用到git管理
- 文件已经提交到暂存区，想删除
  - git rm --cached [filename]
- 文件已经提交到暂存区，删除后想恢复
  - 工作区还存在文件：再git add一次就好
  - 工作区不存在文件：没门，因为相当于没用git管理
- 文件已经提交到本地库，想删除
  - git rm [filename]
  - git commit -m "信息" [filename]
- 文件已经提交到本地库，删除后想恢复
  - 使用 git reflog 查看最近的旧版本的索引号
  - 使用 git reset --hard 索引号回到最近的旧版本



### 文件对比

- git diff [filename]：将工作区与暂存区进行对比
- git diff \[版本索引号]  [filename]：将工作区文件与对应版本文件进行对比
- 不带 [filename] 时，比较全部文件



### 分支管理

* 分支概念

  * 在版本中，能够创建多个副本，在各个副本上可以进行的操作，最后副本可以与主体进行合并

  * 示意图

    ![](https://github.com/gothicrush/learning/blob/master/Git/03.git%E6%A0%B8%E5%BF%83%E6%93%8D%E4%BD%9C/images/%E5%88%86%E6%94%AF%E7%A4%BA%E6%84%8F%E5%9B%BE.PNG)

* 分支优点

  * 多线操作，多任务同时推进，提高开发效率
  * 开发过程中，如果副本开发失败，则可以直接删除副本，不会对主体造成影响

* 分支操作

  * 查看分支：git branch -v【带*号的是当前选中分支】
  * 创建分支：git brance [name]
  * 切换分支：git checkout [name]
  * 合并分支：git merge [被合并的分支]

* 分支冲突

  * 冲突概念：当分支1修改了一个地方为A，而分支2修改了一个地方为B，则在合并过程中就会发生冲突，git并不知道以哪个为准，所以git会进入分支冲突处理状态，由人工进行冲突的解决

  * 冲突位置：发生冲突的文件，git会标注冲突位置

    ```bash
    >>>>>>>>>> 分支1
    分支1的冲突内容
    ================
    分支2的冲突内容
    <<<<<<<<<< 分支2
    ```

  * 冲突解决

    * 人工修改\>>>>>>>与<<<<<<<< 之间的冲突内容
    * git add [filename] 添加修改文件名
    * git commit -m "信息"   不能带具体文件名



### 标签

* 标签的概念
  * 在项目发布新版本时，可以为版本起一个名字，即为版本库中打一个标签，一个标签就唯一标识一个版本 
* 标签的本质
  * 就是对快照索引号起一个别名，便于区分和查找
* 标签的用法
  * 为当前版本打标签：git tag [tagname] -m "信息"
  * 为指定版本打标签：git tag \[tagname] [索引号] -m "信息"
  * 查看所有标签：git tag
  * 查看标签信息：git show [tagname]
  * 删除标签：git tag -d [tagname]
* 标签与远程库的关系
  * 创建的标签都只存储在本地，不会自动推送到远程 
  * 推送特定标签到远程库：git push origin [tagname]
  * 推送全部标签到远程库：git push origin --tags
  * 删除远程库的标签
    * 首先删除本地标签：git tag -d [tagname]
    * 然后删除远程库标签：git push origin :refs/tags/[tagname]



