---
layout:     post
title:      "PC端设置多个SSH秘钥"
subtitle:   "Set up multiple SSH secret keys on your computer"
date:       2018-11-04 22:00:00
author:     "Vinter"
header-img:   "img/in-post/post-web-forepart/multiple-ssh-key.jpg"
header-mask:  0.3
# catalog:      true
tags:
    - 前端开发
    - git
    - ssh
---

> 系统介绍单个SSH秘钥的设置方法。  
> 多个guthub账号又该如何绑定同台电脑。

网上看了很多版本的PC设置SSH-key的方法，感觉都不够具体，虽然不是什么高深的东西，但是分享这篇文章，让朋友们能更好认识，更快的设置好SSH。

**SSH简介** 
Secure Shell (SSH) 是一个允许两台电脑之间通过安全的连接进行数据交换的网络协议。通过加密保证了数据的保密性和完整性。SSH采用公钥加密技术来验证远程主机，以及(必要时)允许远程主机验证用户。
<br/>

**一、SSH设置** 
创建SSH（本地计算机与你的github账户关联，不用每次输入密码）
<br/>
1、查看本机是否已经有.ssh文件夹，如果提示“ No such file or directory”，表示没有该文件夹
<br/>
`cd ~/.ssh/`

2、若无，手动创建并进入文件夹中
```
    mkdir ~/.ssh
    cd ~/.ssh/
```

3、配置全局的name和email,如果想配置多个ssh-key最好不要设置全局name和email
```
    git config --global user.name "username" 
    git config --global user.email "email"
```

4、生成公钥和私钥
<br/>
`ssh-keygen -t rsa -C "email" `

5、连续三次回车，设置的密码为空，并创建了key
<br/>
6、得到id_rsa(私钥)和id_rsa.pub(公钥)两个文件夹
<br/>
7、将公钥添加进github或者bitbucket
<br/>
8、测试github是否已经安装成功
```
    ssh -T git@github.com
    提示：“Hi username! You've successfully authenticated, but GitHub does not provide shel l access.”
```

9、eg:使用方法
<br/>
`git clone [ssh]`

**二、PC端设置多个SSH-KEY**
<br/>
1、创建两对ssh-key
```
ssh-keygen -t rsa -C "1234324@afd.com" -f ~/.ssh/id_rsa_work
ssh-keygen -t rsa -C "2424545@qq.com" -f ~/.ssh/id_rsa_home

```

2、将两对公钥添加进不同的github账号
<br />
3、在公钥和私钥的同级目录下创建 “config”文件夹,配置如下
```
# 添加config配置文件

# 文件内容如下：
# home
Host home.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_home
    User user1

# work
Host work.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_work
    User user2    

# 配置文件参数
# Host : Host可以看作是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和ssh文件
# HostName : 要登录主机的主机名
# User : 登录名
# IdentityFile : 指明上面User对应的identityFile路径

```

4、测试
```
ssh -T git@home.github.com
//  Hi user1! You've successfully authenticated, but GitHub does not provide shell access.
ssh -T git@work.github.com
//  Hi user2! You've successfully authenticated, but GitHub does not provide shell access.

```

5、使用方法，eg:clone
```
git clone git@work.github.com:WSxzwen/vueBlog.git
git clone git@home.github.com:Xzwen/vueBlog.git

// 提示：git clone git@github.com:WSxzwen/vueBlog.git => git clone git@work.github.com:WSxzwen/vueBlog.git 或 git clone git@home.github.com:Xzwen/vueBlog.git
```

6、提交代码时会提示，会提示无权限，需要设置本项目的email或者name，注意别配置全局name 或者 email
```
git config user.email '目标github的email或者name'

```

7、如能正确提交，表示OK


> no pain,no gain!   
> Have a good night! 


