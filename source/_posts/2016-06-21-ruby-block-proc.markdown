---
layout: post
title: "ruby 中的block 及 proc "
date: 2016-06-21 11:23:17 +0800
comments: true
categories: ruby
---


## Block
目前最常見的寫法都是用block，主要是使用 do end 來構成一個block，例如

```
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

arr.collect! do |n|
  n * 2
end

```

自己來寫一個看看

```
class Array
  def foreach
    self.each_with_index do |n, i|
      self[i] = yield(n)
    end
  end
end

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
arr.foreach do |n|
  n * 2
end


```

## Proc

這二種寫proc都可以
```
proc = Proc.new { puts "this is proc new"}
proc.call

proc = Proc.new do
  puts "this is proc block too"
end

proc.call

```

使用lambda 的方式
```
proc = lambda{ puts "lambda"}
proc.call

proc = -> { puts "lambda too"}
proc.call
```

## lambda to block

printer 是一個lambda ，因為在 pls.each 後只能放block，但可以透過 & 來將block 轉成 proc
```
pls = ["Ruby", "Python", "Java", "PHP", "Node"]
pls.each do |pl|
 puts "I like #{pl} "
end

printer = lambda{|pl| puts "I like #{pl}, too " }
pls.each(&printer)

```


另外有常見的二種寫法，結果是一樣的
```
 tweets.map { |tweet| tweet.user }
 tweets.map(&:user)
```
## option block

可以使用 block_given? 來判斷傳入的是不是block
```
class Timeline
  attr_accessor :tweets

  def print
    if block_given?
      tweets.each { |tweet| puts yield tweet }
    else
      puts tweets.join(", ")
    end
  end
end

timeline = Timeline.new
timeline.tweets = ["Ruby", "Python", "Java", "PHP", "Node"]

timeline.print
timeline.print { |tweet|
 "tweet: #{tweet}"
}
```

## 心得
block 、 Proc 、lambda  都一直都很容易混亂的，所以這個部份我也常常都要回來重新查一下觀念及寫法

## 參考資料

* CodeSchool
* [http://rubyer.me/blog/917/] (http://rubyer.me/blog/917/)
