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

[Git完整代码](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)



## 常识

- .git 目录
  - 存储版本信息 , 管理控制的



# 文件状态的演变

.git 就是一个观察者

1. 拷贝文件夹里面的内容到 .git 里面
2. 观察 文件夹的变化 ，同步到 .git的拷贝中去
3. 每次变化都有单独的拷贝



## 文件的状态生命周期

- ![](assets\git4.jpg)
  - Untracked 「 未追踪 」
  - Staged   「暂存」
    - `git  commit`
    - 拷贝到 .git 中
  - Unmodifiled 「未修改」
    - 拷贝的默认状态 就是未修改
    - 一旦监听到修改 , 就同步修改
  - modifiled 「已修改」
    - 记录修改的变化
    - 方便版本回退



- 核心理解
  - 真正的核心就是 上图中的 天蓝色 阴影部分, 这里面放着最终的版本 
    - `git commit` 这条命令就是让文件变成 Unmodifiled 状态
  - modifiled 和 Staged  状态的区别与联系
    - 两者都是和文件的改动有关
    - Staged  会暂存文件目录里面文件的变化
    - modifiled 会保存文件的最终变化 , 方便版本回退
    - `git add`  这条命令是记录变化的核心
- `git commit -am`

```powershell
git add .
git commit -m "提交文件的修改"  
```

- 上述是等价的



- **git add 与 git commit -a 有什么区别**
  - 对于新增文件 , 必须先 add  , 不可以一步到位





## 小结

以下两个命令能满足绝大多数使用场景：

```
git add .
git commit -am "bug fix"
```

 

