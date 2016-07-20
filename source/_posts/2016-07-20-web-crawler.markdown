---
layout: post
title: "使用 ruby 來爬資料"
date: 2016-07-20 23:40:09 +0800
comments: true
categories: ruby
---


## 前言
很單純只是要爬資料，所以使用 Rails 的 ActiveJob 來處理

為什麼要用 ActiveJob 呢，因為如果這個是有定期需要處理的，可以再加上cron 由系統自己去背景處理


## 說明

#### 產生 active job

```
rails g job trunk
```
在 app/jobs/下會多一支 trunk_job.rb
修改一下，內容如下

```
class TruckJob < ApplicationJob
  queue_as :default

  URL = "http://這是密秘/index.php"
  @regions, @data = [], []

  def perform(*args)
    find_all_regions
    start_fetch_regions
  end

  def find_all_regions
    parse_page = Nokogiri::HTML(HTTParty.get(URL))
    #先打所有的區先整理下來
    @regions = parse_page.css("#Contentcarry_lineSearchs_D").css("option").map{|c| c.text}
  end

  def start_fetch_regions
    @regions.each do |region|
      fetch_region(region)
    end
  end

  def fetch_region(region)
    50.times do |i|
      url = URI.encode("#{URL}?s_D=#{region}&carry_linePage=#{i}&carry_linePageSize=100")
      line_parse = Nokogiri::HTML(HTTParty.get(url))
      # 查出來沒有資料的就離開
      if line_parse.css(".Row").length == 0
        break;
      end

      line_parse.css(".Row").map do |row|
        one_row = row.css("td").map{|c| c.text.strip}
        save(one_row)
      end
    end
  end

  def save(data)
    Spot.transaction do
      spot = Spot.new
      spot.area             = data[0]
      spot.trunk_number     = data[1]
      spot.number           = data[2]
      spot.city             = data[3]
      spot.village          = data[4]
      spot.address          = data[5]
      spot.arrival_time     = data[6]
      spot.day_of_the_weeks = data[7]
      spot.save
    end
  end

end

```

## 測試

```
TrunkJob.perform_now
```

其實以上只是要備存作法，沒有太大意思 XD
