---
layout: post
title: "在 Rails 中 使用 delegate method"
date: 2016-07-11 23:08:23 +0800
comments: true
categories: rails
---

## 什麼是 delegate
delegate是 Rails ActiveSupport中的一個Module,
可以在class 中定義 delegate 來取得另一個關聯的attribute，容易簡單操作使用，相信沒有人看的懂這是什麼意思，所以直接使用範例說明

## 範例一

例如 User 是has_one profile, 在一般情況下，我們可能會使用 `user.profile.age`, 但如果要讓age 可以直接定義在user物件裡，就可以使用 `delegate :age, to: :profile` 的方式，把profile 的 age 直接在 user 中可以操作
```
class User < ActiveRecord::Base
  has_one :profile
  delegate :age, to: :profile, allow_nil: true
end

User.new.age # nil
```


## 範例二

除了定義has_one 的對象外，也可以直接定義 variable, class variable, constants

```
class Foo
  CONSTANT_ARRAY = [0,1,2,3]
  @@class_array  = [4,5,6,7]

  def initialize
    @instance_array = [8,9,10,11]
  end
  delegate :sum, to: :CONSTANT_ARRAY
  delegate :min, to: :@@class_array
  delegate :max, to: :@instance_array
end

Foo.new.sum # => 6
Foo.new.min # => 4
Foo.new.max # => 11

```

## 參考資料來源

http://apidock.com/rails/Module/delegate
https://simonecarletti.com/blog/2009/12/inside-ruby-on-rails-delegate/

