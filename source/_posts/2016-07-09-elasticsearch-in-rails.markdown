---
layout: post
title: "在 Rails 中使用 Elasticsearch"
date: 2016-07-09 22:34:24 +0800
comments: true
categories: elastichsearch, rails, gem
---

承上一篇(http://jimmy0328.github.io/blog/2016/07/09/introduction-elasticsearch/)
怎麼在 Rails 中實現 ElastichSearch 的全文檢索呢

以下假設要search Boook
## 安裝

#### 安裝 elastichsearch-model gem
```
gem 'elasticsearch-model', git: 'git://github.com/elasticsearch/elasticsearch-rails.git'

```
bundle install

#### 設定初始化

新增 config/initializers/elastichsearch.rb

這邊假設index 名稱為 els ，這邊請自行修改成自己的 index name
```
Elasticsearch::Model.client = Elasticsearch::Client.new(Rails.application.secrets.elasticsearch)

if not Elasticsearch::Model.client.indices.exists(index: :els)
  Elasticsearch::Model.client.indices.create(index: :els)
end

```

secrets.yml

```
development:
  elasticsearch:
    :host: localhost:9200

```


## Model 的設定


```
class Book < ApplicationRecord

  include Elasticsearch::Model
  include Elasticsearch::Model::Callbacks

  index_name "els"

  def as_indexed_json(option={})
    self.as_json({
      only: [:name, :description],
    })
  end
end
```

## 測試

#### import data

如果已經存在data的，可以透過 import 把現有的資料寫到 elasticsearch中
```
Book.import
```

#### 查詢

```
Book.search "rails"
```

#### 查詢結果

```
res = Book.search "rails"
res.records.records
```

#### 更多
https://github.com/elastic/elasticsearch-rails/tree/master/elasticsearch-model

## 參考資料
https://github.com/elastic/elasticsearch-rails/tree/master/elasticsearch-model
http://www.codinginthecrease.com/news_article/show/409843?referrer_id=948927



