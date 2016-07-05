---
layout: post
title: "使用 rack mini profiler 來查看效能"
date: 2016-07-05 22:47:59 +0800
comments: true
categories: gem
---

Rails 開發速度很快，但也很容易誤用造成效能很低落，很多原因都是出來n+1 沒有處理好，
`rack-mini-profiler` 是一個很方便在畫面上就可以查看畫面執行速度及sql 效能問題

## 安裝

```
gem 'rack-mini-profiler'

```
bundle install

## 畫面

預設會在畫面的左上角，也可以透過設定把它改到右上
![Alt text](/images/profiler/view.png =400x)

很明顯這個畫面有執行多次sql 問題
![Alt text](/images/profiler/view_n_sql.png =400x)

可以點開sql 查看明細

![Alt text](/images/profiler/view_n_sql_detail.png =400x)

## 其它

如果想要在正式機上只有管理者才能看到

```
before_action do
  if current_user && current_user.is_admin?
    Rack::MiniProfiler.authorize_request
  end
end
```


