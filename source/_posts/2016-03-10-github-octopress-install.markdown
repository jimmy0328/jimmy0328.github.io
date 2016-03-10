---
layout: post
title: "Github Octopress 安裝介紹"
date: 2016-03-10 17:42:14 +0800
comments: true
categories:
---

有許多網站都寫的很詳細了，這邊就做個簡單的整理就好
這邊假設你己經有裝好ruby 及 bundler 環境，如果沒有安裝可以參考下方的參考連結
步驗如下

### 建立github repository
必須先建立一個 repository, 在github 上可以有blog的方式，他的命名規則必須是 [username].github.io

[New Repository](https://github.com/new)

[放圖片]

### 下載Octopress 及 安裝

```
 git clone git://github.com/imathis/octopress.git octopress
 cd octopress
 bundle install
 rake install
```

### 將 blog 與 repository 同步

```
rake setup_github_pages
```

這邊會問你的github的repository, 所以要輸入git@github.com:xxxxx/xxxx.github.io.git

```
rake generate
rake deploy
```

```
git add .
git commit -m "init blog"
git push origin source
```
大功告成，將blog 上傳到 github


### 其它指令

```
rake 'new_post["title"]'

rake generate   # Generates posts and pages into the public directory

rake watch      # Watches source/ and sass/ for changes and regenerates

rake preview    # Watches, and mounts a webserver at http://localhost:4000
```

# 參考連結
http://zerodie.github.io/blog/2012/01/19/octopress-github-pages/

http://wen00072-blog.logdown.com/posts/258497-octopress-installed-and-deployed-on-the-github-pages

http://octopress.org/docs/deploying/github/

# 其它問題

```
zsr:1: no matches found: [rejected]
r: failed to push some refs to 'git@github.com:jimmy0328/jimmy0328.github.io.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
先切到_deploy的目錄

```
git push origin master -f
```
http://blog.mohitkanwal.com/blog/2014/03/26/blogging-with-octopress-from-2-computers/

