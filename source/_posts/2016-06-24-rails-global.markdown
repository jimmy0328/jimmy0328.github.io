---
layout: post
title: "Rails 多國語系－資料庫"
date: 2016-06-24 23:46:43 +0800
comments: true
categories: gem
---

## 說明

在上一篇 [Rails 多國語系-rails_i18n](http://jimmy0328.github.io/blog/2016/06/23/rails-i18n/) 是使用 rails-i18n 來達到多國語系，設定的方式大多都是使用yml 的設定檔來建立不同的語系，所以一旦你的平台是內容的多國語系，這個時侯就變成要時常在修改locale的yml檔，相當沒有彈性。

使用 globalize 可以把多國語系建立在資料庫中，所以可以直修改data就可以維護多國語系了

## 安裝

#### 1. gem install globalize
globalize 這個gem 似乎問題不小，在rails 4.2.3 跑 官方的 master branch 是有問題的
加上 Rails5 又有些不一樣了，改天再來在Rails5測試看看，總之以下方式是在 Rails5 是無法執行的

```
gem 'rails-i18n'
gem 'globalize', git: "git@github.com:globalize/globalize.git"
```

#### 2. bundle install
```
bundle install
```

#### 3. 建立 translations
```
rails g migration create_post_translation
```

#### 4. 修改 migration 檔案
```
class CreatePostTranslations < ActiveRecord::Migration
  def self.up
    Post.create_translation_table!({
      :title => :string,
      :context => :text
    }, {
      :migrate_data => true
    })
  end

  def self.down
    Post.drop_translation_table! :migrate_data => true
  end
end
```

#### 5. 修改 model

```
class Post < ActiveRecord::Base
  translates :title, :context
end
```

#### 6. 修改config/application.rb
加入
```
略
config.i18n.fallbacks = true
略
```

#### 7. rake db:migrate

```
rake db:migrate
```
如果到這邊是順利的，那應該就表示成功了，否則應該就是版本上有些問題，可能又要再google 一下了

## 測試

1. rails console

2. 測試建立資料

```
I18n.locale = :en

Post.create({ title: "test1", context: "test1"})

I18n.locale = :"zh-TW"

post = Post.last
post.title = "測試"
post.save

```
大功告成，此時在 locale = :en 的 title 是 `test1`, 而 locale = :"zh-TW" 的 title 是 `測試`

## 參考資料

- https://github.com/ncri/globalize

- http://railscasts.com/episodes/338-globalize3?autoplay=true


