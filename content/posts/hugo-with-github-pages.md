---
title: "使用Hugo和Github Pages搭建自己的博客"
date: 2019-05-27T12:41:05+08:00
tags: ["hugo", "github pages"]
categories: ["技术"]
---

# 使用Hugo和Github Pages搭建自己的博客

撸起袖子直接干

## Hugo初始化
### 1 安装Hugo
Mac, 使用homebrew安装：
```shell
brew install hugo
```

### 2 初始化站点
```
# 替换<your-site-name>
hugo new site <your-site-name>
```

### 3 添加主题
通过git clone自己想要的主题，这里我们使用[maupassant](https://github.com/rujews/maupassant-hugo)主题
```shell
# 进入刚才的站点目录
cd <your-site-name>
# 克隆主题
git clone https://github.com/rujews/maupassant-hugo themes/maupassant
```
### 4 应用主题
更改config.toml文件，添加如下内容
```toml
theme = "maupassant"
```
### 5 本地启动hugo server，并访问相应[URL](http://localhost:1313)查看效果
```shell
hugo server
```
### 6 测试没问题后，停掉hugo server
```
ctrl + c
```

## 关联Github Pages
### 1 创建两个github仓库
第一个仓库用于保存hugo自身的文件，比如主题等。仓库名随意，在此，我们将仓库取名为blog。

第二个仓库就是github pages的仓库了，要求仓库名必须以如下形式：<github_user_name>.github.io 。在此，我们以realdeanzhao.github.io为例。
### 2 给站点目录添加git远程仓库，此仓库为刚才创建的blog仓库
```shell
git remote add origin <your-blog-repo-url>
```
### 3 添加站点目录下的public目录为git子仓库
```shell
# 删除刚才hugo启动时发布的内容
rm -rf public
# 添加public目录为git子模块
git submodule add -b master https://github.com/RealDeanZhao/realdeanzhao.github.io.git public
```
### 4 编写发布脚本
创建deploy.sh脚本，它能够将public目录的内容push到realdeanzhao.github.io仓库下。

脚本使用方法：
```shell
# 举例： ./deploy.sh "this is the first commit"
./deploy.sh <git提交的message>
```

脚本内容如下：
```shell
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..
```

### 5 访问github page
打开浏览器，即可查看效果了。

