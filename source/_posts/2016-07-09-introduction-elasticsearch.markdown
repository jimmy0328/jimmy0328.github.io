---
layout: post
title: "Elasticsearch 介紹"
date: 2016-07-09 17:45:41 +0800
comments: true
categories: elasticsearch
---

## 什麼是 ElastichSearch

![Alt text](https://www.elastic.co/assets/blt79de4c53eaf2f299/elastic_usergroup.png =600x)

ElastichSearch 是一套Java開發使用 Lucence 作為核心, ELＳ可以實現索引和搜索功能的功能，使用簡單 RESTful API 的方式來操作(因為如果直接使用 Lucence 是很複雜的)，所以在操作全文搜索就變的很簡單了。


## 安裝

下載位置 https://www.elastic.co/downloads/elasticsearch

可以直接下載 zip

在elasticsearch-[version] 下執行

```
./bin/elasticsearch

```
啟動成功的畫面

![Alt text](/images/elasticsearch/start_server.png =600x)


## 測試REATful API

#### 新增索引

```
curl -XPUT http://localhost:9200/rtaiwanr/spot/1 -d '{  "user": "jimmy","message": "Trying out elasticsearch, sfar so good?"}'

```

![Alt text](/images/elasticsearch/new.png =600x)

#### Search API

```
curl -XGET http://localhost:9200/rtaiwanr/spot/_search -d '{  "query" : {"term" : { "user": "jimmy" }}}'
```

![Alt text](/images/elasticsearch/search.png =600x)

## 其它
這篇沒有講太多的 ElasticSearch 相關，會提到elasticSearch 安裝而己，是因為下一篇我要寫 Rails 與 ElasticSearch 的整合，所以只是要基本的概念

如果要進階要了解ElasticSearch 的範疇還有很多
1. ElasticSearch
2. Logstash
3. Kibana
之後有空再找時間來研究

## 參考資料

https://www.elastic.co/

http://es.xiaoleilu.com/010_Intro/05_What_is_it.html

http://reality.hk/2011/10/19/using-elasticsearch/

http://zettadata.blogspot.tw/2014/09/elkelastic-searchlogstashkibanalog.html
