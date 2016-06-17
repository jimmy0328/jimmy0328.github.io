---
layout: post
title: "Rspec 介紹"
date: 2016-06-17 10:34:23 +0800
comments: true
categories: test
---

## 什麼是Rspec

[Rspec 官網](http://rspec.info)

Behaviour Driven
Development for Ruby.
Making TDD Productive and Fun.

簡開單的說它就是一種測試工具，在系統開發過中，很多人會使用TDD的開方方法來做開發，所以撰寫測試就是一個其中一個很重要的部份了。

Rspec 是 Ruby 專用的測試，所以你可以在許多在Rails專案中導入使用來測試 Controller, Model, Request ..等
這邊要介紹的是單純的Ruby程式怎麼使用 Rspec 來測試


## 安裝Rspec

在你的開發目錄中，先安裝 Rspec

```
gem install Rspec
```

接著在目錄下初始化一下 Rspec

相關的Rspec套件也會一併安裝
![Alt text](/images/rspec/gem_install_rspec.png)

```
rspec --init
```

![Alt text](/images/rspec/rspec_init.png)

此時會自動產生一個spec 的目錄，在目前裡會有一支 spec_helper.rb

## How to use

在以下的範例僅是我為了簡單解說，所以不是一個很正規的架構方法

* 首先建立一個 ruby class, 我這邊是一支 tag.rb
* 建立一個 models 的目錄，之後有撰寫class的測試腳本都可以放在這邊
* 在 models 裡建立一支 tag_spec.rb

大致上需要先手動建立一下這些目錄及程式


![Alt text](/images/rspec/folder_tree.png)

#### ruby class
這是一個很簡單的ruby class ,好像沒什麼功能，所以才說單純是為了練習
在class裡有一個方法 add 可以用來加1

`` tag.rb ``
```
class Tag
  attr_reader :count

  def initialize
    @count = 0
  end

  def add
   @count += 1
  end
end

```

#### spec

這邊不解說Rspec的寫法，下一篇會詳細說明

`` tag_spec.rb ``
```
require 'spec_helper'
require 'tag'

describe Tag do
  it 'tag count for each roll' do
    tag = Tag.new
    10.times { tag.add }
    expect(tag.count).to eq(10)
  end
end

```


## 測試

可以直接測單一程式或是整個目錄

單一程式
```
rspec /spec/models/tag_spec.rb
```
![Alt text](/images/rspec/test_one_class.png)

測試整個目錄
```
rspec /spec/models
```
![Alt text](/images/rspec/test_models.png)


## 心得

有做測試會比較有品質保障
接下來還會有幾篇陸續介紹
今天打完收工
