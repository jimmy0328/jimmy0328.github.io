---
layout: post
title: "使用 ActionCable 做聊天室"
date: 2016-07-03 12:11:35 +0800
comments: true
categories: actioncable, redis
---

## ActionCable Tutorial
實作一個簡易的聊天室

#### 1. 建立 Rails Project

```
rails new livechat
```

#### 2. 建立room controller

```
rails g controller rooms show
```

內容如下

```
class RoomsController < ApplicationController
  def show
    @messages = Message.all
  end
end
```

#### 3.建立Message model

```
rails g message context:text
```
rails db:migrate

#### 4. 修改View

rooms/show.html.erb


![Alt text](/images/actioncable/chat_view.png =400x)

 建立 messages/_message.html.erb

```html
<div class="message">
  <p> <%= message.context %> </p>
</div>
```

#### 5. 建立ActionCable

```
rails g channel room speak

```
會產生

app/channels/room_channel.rb

app/assets/javascripts/channels/room.coffee


##### 6. routes 設定
設定可以接受 cable 的 route

```
Rails.application.routes.draw do
  root to: 'rooms#show'
  mount ActionCable.server => '/cable'
end
```
#### 7. 修改 assets/javascripts/channels/room.coffee

@perform 'speak' 是指 speak chennel 要傳送的內容
註冊好 App.room 的 speak 與 received 之後，就在輸入框設定keypress 事情的listener

```
App.room = App.cable.subscriptions.create "RoomChannel",
  connected: ->
    # Called when the subscription is ready for use on the server

  disconnected: ->
    # Called when the subscription has been terminated by the server

  received: (data) ->
    $("#messages").append data['message']

  speak: (message) ->
    @perform 'speak', message: message

$(document).on "keypress", "[data-behavior~=room_speaker]", (event) ->
  if event.keyCode is 13
    App.room.speak event.target.value
    event.target.value = ''
    event.preventDefault()
```


#### 8. 修改 Server side 的Room Channel

修改 app/channels/room_channel.rb

```
class RoomChannel < ApplicationCable::Channel
  def subscribed
    stream_from "room_channel"
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end

  def speak(data)
    Message.create context: data['message']
  end
end
```

#### 9. 使用ActiveJob 推送訊息

Rails5 新增的 ApplicationControoler.renderer.render 功能
之後再來聊
總之這邊是broadcast messages/message 內容出去

```
class MessageBroadcastJob < ApplicationJob
  queue_as :default

  def perform(message)
    ActionCable.server.broadcast 'room_channel', message: render_message(message)
  end

  private

  def render_message(message)
    ApplicationController.renderer.render(partial: 'messages/message', locals: {message: message})
  end
end
```

#### 10. 修改 application.rb

設定active_job 使用 sidekiq

```
class Application < Rails::Application
  # Settings in config/environments/* take precedence over those specified here.
  # Application configuration should go into files in config/initializers
  # -- all .rb files in that directory are automatically loaded.
 config.active_job.queue_adapter = :sidekiq
end
```

#### 11. 修改 environment/development.rb 及 production.rb

```
config.action_cable.disable_request_forgery_protection = true
```

#### 12. 修改 cable.yml

在active job 我們是透過 redis 來處理server side 自己的 pub/sub

```
development:
  adapter: redis
  url: redis://localhost:6379/2

test:
  adapter: async

production:
  adapter: redis
  url: redis://localhost:6379/1
```

#### 13. 完成
大功告成，但一開始我沒有提到要先裝 redis 及 sidekiq
這邊完全是照著 https://blog.codeship.com/how-to-use-rails-active-job/ DHH教學文來做的，但我也不知道為什麼他沒有設定redis也可以work, 總之因為要跑 active job 來推播，所以這個部份我有加了一些設定，否則其它全部都是一樣的

## 參考資料來源

https://www.youtube.com/watch?v=n0WUjGkDFS0
http://edgeguides.rubyonrails.org/active_job_basics.html
https://blog.codeship.com/how-to-use-rails-active-job/

