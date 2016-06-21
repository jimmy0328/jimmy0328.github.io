---
layout: post
title: "Rails5 Job with Sidekiq"
date: 2016-06-21 16:38:19 +0800
comments: true
categories: rails
---

以下都是以Mac環境為例，linux 及 window 就不好意思了

## 安裝 sidekiq

#### 1. 首先，你的電腦必須安裝 Redis 資料庫，可以使用 Homebrew 安裝

```
brew install redis

```

#### 2. 在 Gemfile 中加入
```
gem 'sidekiq'
```

#### 3. bundle install

```
bundle install
```

#### 4.加入 Job

```
rails g job inspector
```
會產生 jobs 目錄及 一支 inspector_job.rb

#### 5. 在inspector_job 中加入實作
在jobs/inspector_job.rb
```
class InspectorJob < ApplicationJob
  queue_as :default

  def perform(*args)
    puts "================== print inspector Job ============="
  end
end
```

#### 6. 宣告 job 使用sidekiq 來處理

```
# in /config/application.rb
class Application < Rails::Application
  # ...
  config.active_job.queue_adapter = :sidekiq
end
```

#### 7. 加入redis 的config

新增 initializers/sidekiq.rb
```
Sidekiq.configure_server do |config|
  config.redis = Rails.application.secrets.redis
end

Sidekiq.configure_client do |config|
  config.redis = Rails.application.secrets.redis
end
```
#### 8. redis 的連線我寫在secret.yml

```
略
development:
   secret_key_base:
   redis:
    :url: redis://localhost:6379
略
```

#### 9. 執行Job

```
InspectorJob.perform_later("xxxx")

```


#### 10.  Run the worker

```
bundle exec sidekiq -e development
```

```
worker: bundle exec sidekiq -c 10 -q priority -q default
```

## 安裝sidekiq web UI

#### 1. route.rb 加入

```
require 'sidekiq/web'

Rails.application.routes.draw do
...
  mount Sidekiq::Web, at: '/sidekiq'
end
```

#### 2. gemfile 加入
Rails5 要用 sinatra master 的版本

```
gem 'sinatra', github: 'sinatra'
```

#### 3. 查看 web UI

http://localhost:3000/sidekiq


## 測試

#### 1. 在專案下先執行sidekiq
```
sidekiq
```

### 2. rails console
```
InspectorJob.perform_later("xxx")
```

#### 3.sidekiq log 會印出

```
2016-06-21T08:16:45.734Z 28527 TID-owmgdm1ak InspectorJob JID-fe520b589d7cbdbaa1f3bd5c INFO: start
================== print inspector Job =============
2016-06-21T08:16:45.738Z 28527 TID-owmgdm1ak InspectorJob JID-fe520b589d7cbdbaa1f3bd5c INFO: done: 0.004 sec
```

## 參考資料來源

http://railscasts.com/episodes/366-sidekiq
http://kakas-blog.logdown.com/posts/738075-used-in-rails-sidekiq
https://www.rubyplus.com/articles/3931-Rails-5-ActiveJob-Basics-with-Sidekiq
http://epigene.github.io/Rails5_Redis_And_Sidekiq/
http://github.com/mperham/sidekiq/wiki/The-Basics
