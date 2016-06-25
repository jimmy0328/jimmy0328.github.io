---
layout: post
title: " Rspec in Rails 安裝篇"
date: 2016-06-25 17:08:12 +0800
comments: true
categories: test, gem
---


## 說明

## 安裝

#### Gemfile 加入 rspec-rails
```
group :development do
(略)
gem 'rspec-rails', '~> 3.4'
(略)
end
```


#### bundle install

```
bundle install
```

#### install rspec

```
rails generate rspec:install

```

![Alt text](/images/rspec/install_rspec.png =400x)


#### add model rspec

app/models/post.rb

post model add validates

```
validates :title, :context, presence: true
```


spec/models/post_spec.rb
```
require 'rails_helper'

RSpec.describe Post, type: :model do

  it "valid attributes" do
    expect(Post.new).not_to be_valid
  end

  it "valid attributes" do
    post = Post.create!(title: 'test', context: 'test')
    expect(post).to be_valid
  end

end
```
#### 測試

```
spec spec/models/post_spec.rb
```

![Alt text](/images/rspec/test_pass.png =400x)


#### 參考資料
https://github.com/rspec/rspec-rails#model-specs
