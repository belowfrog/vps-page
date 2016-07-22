# 设置git直接部署vps网站

> 参考来源
>
> [设置git自动部署][digital ocean]
>
> [git添加多个远程仓库][oschina]

### 一、创建远程git仓库

1. 创建github项目，拖项目

2. 在vps上创建git服务端，用于接收git push，自动部署文件

   1. 创建两个文件夹，仓库文件夹和部署文件夹`repo/`，`www/`

   2. 在`repo/`中创建git服务端`git init --bare`(—bare 只创建git版本管理，不需要添加源)

   3. 在hooks中添加push来时候的部署动作

      1. `vi post-receive`

      2. ```shell
         #!/bin/sh
         git --work-tree=/root/www --git-dir=/root/repo checkout -f`
         ```

      3. `chmod +x post-receive`

   4. 退出远程vps `exit`

### 二、本地git添加vps源

1. 本地机器添加vps的git地址
   1. git remote add vps ssh://user@domain/root/repo
   2. `git push vps `
2. 可以查看本地git配置
   1. 位置在`.git/config`
   2. 命令为`git config -e`

### 三、设置beta环境

1. 添加beta的环境，根据一二步骤，新建两个beta的对应文件夹`betaWww/`，`betaRepo`
2. 设置在beta中push到vps
   1. 在`betaRepo`仓库下添加vps仓库地址
   2. `git remote add vps ../repo`
   3. 提交代码就这样`git push vps xxx(分支名)`



[oschina]: http://my.oschina.net/shede333/blog/299032
[digital ocean]: https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps