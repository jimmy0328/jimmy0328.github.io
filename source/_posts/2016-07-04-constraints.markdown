---
layout: post
title: "在Rails 中使用constraints來限制條件"
date: 2016-07-04 23:52:21 +0800
comments: true
categories: rails
---


## 說明

有時侯我們在 Rails Routes 想要限定一些URL 的條件，此時就可以使用 constraints
例如:

限制:id必須是整數
```
match "/events/show/:id" => "events#show", :constraints => {:id => /\d/}
```

## 進階作法
有時侯限定的條件不會宣告在routes裡，也可以寫成class 來使用

例如: admin/:id 這個id 是有條件限制, 條件寫在 AppConstraint
```
namespace :admin
  get ":id" => "apps#index",constraints: AppConstraint.new
end
```

在AppConstraint 中定義一個matches? 來處理
以下constraint 是限定`admin/ms1983` 及 `admin/promo` 才是有效的URL ，找不到的就會對應不到URL
```
class AppConstraint
  def matches?(request)
    ActiveRecord::Base.connection_pool.with_connection do
      (["ms1983","promo"]+App.active.pluck(:title)).uniq.include? request.fullpath.gsub(/^\//, "")
    end
  end
end
```

## 加入autoload path
記得在 application.rb 加入, 這邊Rails 才可以知道 constraints 的存在
```
%w[constraints].each do |dir_name|
  config.autoload_paths << Rails.root.join('lib', dir_name)
end
```
## 參考資料來源

https://ihower.tw/rails4/routing.html
http://rails.ruby.tw/routing.html


