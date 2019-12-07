# ROS入门

#### 介绍
Ubuntu安装、ROS安装及ROS架构简介

#### Git基本操作
+ 初始化`git init`
+ 读取地址`git remote add origin`
+ 拉取码云上的所有文件到项目中来`git pull origin master`
+ 添加文件`git add .`(add空格后有个点别忘了)
+ 添加注释`git commit -m "注释原因"`来说明自己为什么要上传，方便以后自己查阅
+ 提交到码云上面`git push origin master`  
  第一次`git push -u origin master`第二次提交就按照上面的写法即可不在需要加`-u`
+ 待定

#### Git撤销
写完代码后，我们一般这样
```
git add . //添加所有文件

git commit -m "本功能全部完成"
```
执行完commit后，想撤回commit，怎么办？

+ 这样：
```
git reset --soft HEAD^
```

这样就成功的撤销了你的commit  
注意，仅仅是撤回commit操作，您写的代码仍然保留。

+ 说一下个人理解：
HEAD^的意思是上一个版本，也可以写成HEAD~1  
如果你进行了2次commit，想都撤回，可以使用HEAD~2
+ 至于这几个参数：
  + --mixed  
    意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作  
    这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。
  + --soft  
    不删除工作空间改动代码，撤销commit，不撤销git add . 
  + --hard  
    删除工作空间改动代码，撤销commit，撤销git add .  
注意完成这个操作后，就恢复到了上一次的commit状态。

顺便说一下，如果commit注释写错了，只是想改一下注释，只需要：
`git commit --amend`
此时会进入默认vim编辑器，修改注释完毕后保存就好了。

#### 软件架构
软件架构说明


#### 安装教程

1.  xxxx
2.  xxxx
3.  xxxx

#### 使用说明

1.  xxxx
2.  xxxx
3.  xxxx

#### 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request


#### 码云特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2.  码云官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解码云上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是码云最有价值开源项目，是码云综合评定出的优秀开源项目
5.  码云官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  码云封面人物是一档用来展示码云会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
