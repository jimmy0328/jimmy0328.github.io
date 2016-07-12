---
layout: post
title: "在 model 中產生 generate token 的方式"
date: 2016-07-12 23:39:38 +0800
comments: true
categories: rails
---

我直接舉例說明，Order 的model 需要有token的欄位我們可以怎麼產生

##方式一: 在model中before_create 來處理

大多我們都是使用這種方式來處理 token

```
class Order < ActiveRecord::Base

  before_create :set_token

  private
  def set_token
    # SecureRandom.urlsafe_base64(nil, false)
    # SecureRandom.hex(10)
    # 這邊可以再加入檢查是否已經有 id 存在
    self.token = SecureRandom.urlsafe_base64(nil, false)
  end
end

```


##方式二: Rails 5 提供的新功能

直接在 model 中宣告 has_secure_token
如果以前有使用過 https://github.com/robertomiranda/has_secure_token ，就是這種用法沒錯

```
class Order < ActiveRecord::Base
  has_secure_token # 預設欄位是token
  has_secure_token :order_token #可以指定欄位
end

```

以下是 Rails5 source code 產生 token 的片段程式，在SecureToken module 裡的 class method 的has_secure_token,
使用 ` SecureRandom.base58(24) ` 來產生token

```
 def has_secure_token(attribute = :token)
    # Load securerandom only when has_secure_token is used.
    require 'active_support/core_ext/securerandom'
    define_method("regenerate_#{attribute}") { update! attribute => self.class.generate_unique_secure_token }
    before_create { self.send("#{attribute}=", self.class.generate_unique_secure_token) unless self.send("#{attribute}?")}
  end

  def generate_unique_secure_token
    SecureRandom.base58(24)
  end

```

## 參考來源

http://blog.bigbinary.com/2016/03/23/has-secure-token-to-generate-unique-random-token-in-rails-5.html

