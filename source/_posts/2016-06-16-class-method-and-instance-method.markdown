---
layout: post
title: " Ruby 裡的class method 與 instance method 使用方式"
date: 2016-06-16 10:14:25 +0800
comments: true
categories: ruby
---


## Instance Method

簡單的說就是要先建立一個實體才能使用裡面的方法

```
class User

  def initialize(name)
    @name = name
  end

  # Instance method
  def say_hello
    "Hello, #{@name}"
  end

end

```
#### 使用方式

```
user = User.new("Ruby")
user.say_hello # => Hello, ruby

```

#### 說明

必須建立實體之後才能調用物件內的方式
所以我們是不能直接用 User.say_hello


## Class Method


```
class Group

  def self.name
    "Kaohsiung Rails Meetup"
  end

  class << self
    def find(user_id)
      "find group user with the id of #{user_id}"
    end
  end
end

```

#### 使用方式


```
puts Group.name # => Kaohsiung Rails Meetup
puts Group.find(1) # => find group user with the is of 1

```

#### 說明
我最常使用的會有這二種宣告方式


## Rails 中的 Class Method

在 Rails 中使用 ActiveRecord modul 時，其實裡面有許多class method 的方式
最常見的就是 where, find, first...等

```
 class Group < ActiveRecord::Base

   scope :active -> { where state: 'active'}
   scope :inactive -> {where state: 'inactive'}

   def permalink
     xxxxxxxx
   end

 end

```

除了ruby 上面的二種宣告class method 的方式，在Rails中還可以使用scope 的方式來達到效果
如果你有在寫Rails的話，scope 的使用方式就應該很熟了

```
 1. Group.active
 2. Group.find
 3. Group.first
```

打完收工


