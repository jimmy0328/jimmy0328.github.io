---
layout: post
title: "使用 Rspec 搭配 shoulda-matchers 測試 model 及 controller"
date: 2016-06-25 20:09:23 +0800
comments: true
categories: test, gem
---

## 說明
在上一章有介紹怎麼安裝, 在這章會介紹幾個重要的觀念及用法

#### 1. describe 及 context

這二個都是用來做分類使用的，例如

```
describe Post do

  context "when post is valid" do
   ...
  end

  context "when post is invalid" do
   ...
  end
end

```


#### 2. 使用 it 與 expect

可以用這二個來做小片斷的測試

```
describe Post do

  context "when post is valid" do

    it "should post if user is broker" do
     ....
     post = Post.create(title: 'test', context: 'test description')
     略
     expect(post.title).to eq("test")
    end

    it "should post if user is operator" do
     ...
    end

  end

  context "when post is invalid" do
   ...
  end
end

```

## 安裝 shoulda-matcher

#### 1. 修改 Gemfile
```
gem 'rails-rspec'
gem 'shoulda-matchers', require: "shoulda/matchers"
```

#### 2. bundle install

```
bundle install
```

https://github.com/thoughtbot/shoulda-matchers

#### 3. rails_helper.rb 加入設定

在最下面加入
```
(略)
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :active_record
    with.library :active_model
    with.library :action_controller
    with.library :rails
  end
end
```
使用 shoulda-matchers 來簡化測試

## 測試 model

首先修改一下model, shoulda-matcher 提供了方便的helper，常見的如下

- ActionModel matchers
- ActionRecord matchers
- ActionController matchers

測試model 當然是用 ActionModel matchers 的helper 來使用，例如: 必填欄位就可以使用 `validate_presence_of`

spec/models/post_spec.rb

```
require 'rails_helper'

RSpec.describe Post, type: :model do

  it { should validate_presence_of(:title) }
  it { should validate_presence_of(:context) }
  it { should belong_to(:user) }

  it "valid attributes" do
    expect(Post.new).not_to be_valid
  end

  it "valid attributes" do
    post = Post.create!(title: 'test', context: 'test')
    expect(post).to be_valid
  end

end

```

## 測試 controller

controller 可以寫到很細，這邊先用 index 及 create 做個簡單的sample

```
require 'rails_helper'

RSpec.describe PostsController, type: :controller do

  describe "GET #index" do

    it "renders the index template" do
      get :index
      expect(response).to render_template("index")
    end

    it "responds successfully with an HTTP 200 status code" do
      get :index
      expect(response).to be_success
      expect(response).to have_http_status(200)
    end

  end

  describe "POST #create" do

    it "can create" do
      expect{
       post :create, post: {title: 'test', context: 'test'} ,format: :json
      }.to change{
        Post.count
      }.by(1)
    end
  end

end

```

除了RSpec + shoulda_matcher 外，在測試上我們也會搭配factory_girl 及 faker 來做一些 fake data 的處理，之後有空來再補這個部份的介紹


