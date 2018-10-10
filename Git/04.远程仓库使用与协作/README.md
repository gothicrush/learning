### 远程仓库

* 远程仓库本质就是一台计算机，该计算机上存储了版本数据

* 远程仓库分类

  | 分类   | 例子                     |
  | ------ | ------------------------ |
  | 局域网 | 自建gitlab服务器         |
  | 互联网 | github \| gitlab \| 码云 |

* 远程仓库工作方式

  * 同团队工作
    * 背景：A开发一个项目，有本地库和远程库，B想以团队成员的方式参与项目的开发

    * 流程：
      * A邀请B进团队，B同意邀请并进入开发团队
      * B把项目clone到本地，然后进行开发，最后把新代码push到远程库
      * A发现有新的代码push，进而把B更新pull到A的本地库中

    * 示意图

      ![](https://github.com/gothicrush/learning/blob/master/Git/04.%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E4%BD%BF%E7%94%A8%E4%B8%8E%E5%8D%8F%E4%BD%9C/images/%E5%90%8C%E5%9B%A2%E9%98%9F%E5%8D%8F%E4%BD%9C.PNG)
  * 跨团队协作
    * 背景：A开发一个项目，有本地库和远程库，B想以非团队成员的方式参与项目的开发

    * 流程：
      * B将A的项目fork到自己的远程库中，再从自己的远程库将项目clone下来
      * B更新代码后push到远程库，在远程库中通过 pull request 请求合并代码
      * A看到pull request并审核通后，将B的新代码合并到自己的项目分支中

    * 示意图

      ![](https://github.com/gothicrush/learning/blob/master/Git/04.%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E4%BD%BF%E7%94%A8%E4%B8%8E%E5%8D%8F%E4%BD%9C/images/%E8%B7%A8%E5%9B%A2%E9%98%9F%E5%8D%8F%E4%BD%9C.PNG)

* 查看本地库对应有哪些远程库

  ```bash
  git remote -v
  ```




### github使用

* 注册登录账号

  网址：https://github.com

  ![](https://github.com/gothicrush/learning/blob/master/Git/04.%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E4%BD%BF%E7%94%A8%E4%B8%8E%E5%8D%8F%E4%BD%9C/images/github%E7%99%BB%E5%BD%95%E4%B8%8E%E6%B3%A8%E5%86%8C.PNG)

* 新建远程库

  ![](https://github.com/gothicrush/learning/blob/master/Git/04.%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E4%BD%BF%E7%94%A8%E4%B8%8E%E5%8D%8F%E4%BD%9C/images/github%E7%99%BB%E5%BD%95%E4%B8%8E%E6%B3%A8%E5%86%8C.PNG)

  ![](https://github.com/gothicrush/learning/blob/master/Git/04.%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E4%BD%BF%E7%94%A8%E4%B8%8E%E5%8D%8F%E4%BD%9C/images/%E6%96%B0%E5%BB%BA%E8%BF%9C%E7%A8%8B%E5%BA%932.PNG)

* 本地库 --> 远程库

  * 将本地库连接远程库

  ```bash
  git remote add 远程库名[默认origin] http协议地址|ssh协议地址
  ```

  * 将本地库推送到远程库

  ```bash
  git push 远程库名[默认origin] 分支名
  ```

  * 注意事项：在推送前，本地库必须拥有远程库的最新的修改，否则不能推送

  ```bash
  git pull 远程库名[默认origin] 分支名
  ```

* 远程库 --> 本地库

  ```bash
  git clone https://github.com/username/repositoryname.git
  git clone git@github.com:username/repositoryname.git
  ```

  

### SSH协议连接

* SSH介绍

  * 本地库与远程库进行数据传输时可以通过两种协议：**HTPP协议**和**SSH协议**
  * HTTP协议：每次连接都输入密码，麻烦但相对安全
  * SSH协议：一旦SSH配置成功，就不用再输入密码，方便但不太安全

* 配置SSH

  * SSH协议基于非对称加密，创建本机的公钥和私钥

    ```bash
    ssh-keygen -t rsa -C 邮箱 # 这里的邮箱要与git config user.email的邮箱相同，否则可能无法提交
    ```

  * 生成的公钥和密钥在**~/.ssh**目录中，私钥：**id_rsa**，公钥：**id_rsa.pub**

  * 在github中加入公钥：**setting -> SSH and GPG keys --> New SSH key**