# Git基础

## Git是什么?Github是什么?

- Git是一个 代码版本管理工具
  - 版本管理是核心概念
    - 代码最近完成上线前 , 肯定会有很多的修改
    - 在写代码的过程中 , 想要从第29天回到第10天的代码 是很麻烦了
    - 你必须手动的给每天写的代码备份 ,  才有可能实现
    - 光是想起来就很麻烦
  - 版本管理 
    - 任意版本都可以通过git 找到
    - 版本回退
    - 多人协作
- Github是一个网站 , 用来存储和分享代码



## 命令行clone项目到本地

- 进入github , 调整clone , 把https 换成 ssh
- 复制地址
  - git@github.com:xxx/yyy.git 
  - 地址类似于这样
- 执行命令
  - `git clone git@github.com xxxx`
- 注意
  - ![](assets\git.jpg)



## 配置密钥

- 命令
  - `ssh -keygen -t rsa -b 4096 -C "your_email@example.com"`
  - 换成githun 注册的邮箱
- 之后无脑下一步就好了
  - 这个过程 , 可以去尝试一下 读一下
  - 最后生成`~/ssh/id_rsa.pub`公钥文件
- 查看公钥
  - 公钥被放置在一个文件
    - vim ~/ssh/id_rsa.pub    或者
    - cat ~/ssh/id_rsa.pub   都可以得到公钥
- 配置github
  - settings 下的目录  ssh and  GPG keys
  - 点击 new SSH key
  - 如图![](assets\git3.jpg)
  - 起个标题  ,  把公钥 复制粘贴到 key 里面
  - 点击add 就好了
- [官方教程](https://help.github.com/articles/generating-an-ssh-key/)



## 为什么要配置公钥?

- 为了保证你的电脑 和  github上的项目 ==完全的支配权==
- 给gitbub上的项目配一把锁   这把锁的名字 就是 公钥
- 这把锁的钥匙 , 就是根据你的电脑生成的一把第一无二的钥匙







## 提交到本地仓库

## 推送到远程仓库



# Git进阶

[git完整文档](https://git-scm.com/)

## 

