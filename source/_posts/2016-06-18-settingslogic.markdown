---
layout: post
title: "Settingslogic gem 的使用"
date: 2016-06-18 10:00:55 +0800
comments: true
categories: gem
---

## 什麼是 Settingslogic
Settingslogic is a simple configuration / settings solution that uses an ERB enabled YAML file. It has been great for our apps, maybe you will enjoy it too. Settingslogic works with Rails, Sinatra, or any Ruby project.

[Git source](https://github.com/binarylogic/settingslogic)


## 安裝

Rails Gemfile中使用

```
gem 'settingslogic'
```

記得 bundle install

## 使用

在Rails model中建立一支 models class, 例如 : Fee

```
ass Fee < Settingslogic

  source "#{Rails.root}/config/fee.yml"
  namespace Rails.env

end

```

在 config 中 增新一支 fee.yml
```
defaults: &default
  salary:
    sales: 0
    staff: 0
    staff_overtime: 0

```

大功告成


## 測試

在 Rails console 中就可以直接類似 Rails model 的方式操作它了

例如: `` Fee.salary``

![Alt text](/images/settingslogic/settingslogic_test.png)
