---
layout: post
title: "ActionCable入門 "
date: 2016-07-02 22:40:23 +0800
comments: true
categories: rails, actioncable
---

## 認識 websocket

我相信對於 Pub/Sub 這個名詞應該很多人都不陌生, 但在於web 的使用是近幾年比較多的，這邊不做深入的介紹，有興趣的自行google一下 polling 、long pulling、Streaming 。

在 HTML5 有支援 websocket 之後，開始有許多技術都出現了，server 主動推播訊息給 client 端的技術可以運用在太多地方了，有許多雲端的務服也開始陸訊出現，這些的一切都是為了實現 Real Time 的push 來誕生，例如： PubNub 、 Pusher 等。

HTML5 Websocket Protocol 連線後，透過 API 可以建立Real Time 即時送出訊息運用
![Alt text](https://www.websocket.org/img/websocket-architecture.jpg =800x)

有興趣的可以參考
https://www.websocket.org/aboutwebsocket.html

之後有空在來談論一下以上這些技術，今天要來介紹的是 Rails 5 中最大的亮點之一的 ` Action Cable `

## 認識 Action Cable

幾個專有名

- Connection
  在 Rails 5 中是指 `ApplicationCable::Connection`
  用來建立 Cable 的連線

- Channel
  在 Rails 5 中是指 `ApplicationCable::Channel`

  可以建立多條 Channel ，例如 聊天室可能就可以建立一個 room channel

- Streams
  用來把 client 送出的訊息傳送給 Subscribers

```
CommentsChannel.broadcast_to(@post, @comment)
```

- Broadcasting
  舉例說明，在聊天室中要傳送訊息出去，就是用 broadcasting 的機制，讓其它人可以同步收到訊息

```
WebNotificationsChannel.broadcast_to(
current_user,
  title: 'New things!',
  body: 'All the news fit to print'
)
```

- Subscriptions
 同Broadcasting , Subscriptions 就是為了接收有人 broadcasting 的資料。
 所以有接到資料的要實作內容。例如：在聊天室中收到訊息的就會把訊息更新到畫面中

```
App.cable.subscriptions.create { channel: "ChatChannel", room: "Best Room" },
  received: (data) ->
    @appendLine(data)

  appendLine: (data) ->
    html = @createLine(data)
    $("[data-chat-room='Best Room']").append(html)

```

## 架構圖

![Alt text](https://heroku-blog-files.s3.amazonaws.com/1462551406-rails-rack.png =800x)

資料來源
https://blog.heroku.com/real_time_rails_implementing_websockets_in_rails_5_with_action_cable



明天再來實作一個聊天室的文章

## 參考資料來源

http://guides.rubyonrails.org/action_cable_overview.html
https://blog.heroku.com/real_time_rails_implementing_websockets_in_rails_5_with_action_cable
