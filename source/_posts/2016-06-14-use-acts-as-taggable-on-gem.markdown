---
layout: post
title: "acts_as_taggable_on gem 介紹"
date: 2016-06-14 22:08:51 +0800
comments: true
categories: rails
---

Rails中使用tag方便整合的gem

[GitHub 官網](https://github.com/mbleigh/acts-as-taggable-on)


## 安裝步驟
Step1: Gemfile中加上

```
gem 'acts-as-taggable-on'
```

回command line 執行 bundle install

Step2: 執行gem 的install

```
rake acts_as_taggable_on_engine:install:migrations
```

Step3: migrate

```
rake db:migrate
```

P.S 如果你使用的資料庫是MySQL 可以另外參加 https://github.com/mbleigh/acts-as-taggable-on#for-mysql-users

## 如何使用

在Model 中的宣告方式有二種

```
class User < ActiveRecord::Base
  acts_as_taggable
  acts_as_taggable_on :skill, :interest
end
```


所以在Controller 中的使用方式, 有依據以上二種設定方式有不同的參數值，分別如下

如果使用``acts_as_taggable`` 後面不接任何參數，那default使用 tags_list
```
class UsersController < ApplicationController
  def user_params
    params.require(:user).permit(:name, :tag_list)
  end
end
```

如果使用 `` acts_as_taggable_on :skills, :interests ``, 那就有區分 skills_list 與 interests_list

```
class UsersController < ApplicationController
  def user_params
    params.require(:user).permit(:name, :skill_list, interest_list)
  end
end
```

## 大功告成

以上是最簡單的設定方式，前端只要送對應的 tag_list 或是 skill_list 過來即可

```
<%= form_for :user , method: :post, url: users_path do |f| %>
略
<%= f.text_field :skill_list%>
<%= f.text_field :interest_list%>
略
```
P.S 輸入的值預設是用逗號區隔開來，例如 interest_list 的值為`` movie, game, swim ``


##提供了找出最多與最少使用的tag

```
ActsAsTaggableOn::Tag.most_used(10)
ActsAsTaggableOn::Tag.least_used(10)
```
預設不給值是抓前20筆

## 其它用法介紹

使用 add 及 remove 來新增及刪除tag

```
@user.tag_list.add("awesome")   # add a single tag. alias for <<
@user.tag_list.remove("awesome") # remove a single tag
```

```
@user.tag_list.add("awesome", "slick")
@user.tag_list.remove("awesome", "slick")
```
也可以很暴力的直接給他值

```
@user.tag_list = "awesome, slick, hefty"
@user.save
@user.reload
@user.tags

```

## 心得

此gem 提供很多不錯的helper，是很簡單就可以整合到專案上的GEM
如果有注意看migration的就會發現它全部都是在控制 tag 與 tagging 二個 table
最後如果想要更了解這個gem的使用可以參考 [GitHub 官網](https://github.com/mbleigh/acts-as-taggable-on)
在RailsCase 也有另一個教學可以參考[RailsCasts](http://railscasts.com/episodes/382-tagging)

