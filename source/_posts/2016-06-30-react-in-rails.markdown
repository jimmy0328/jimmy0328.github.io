---
layout: post
title: "在Rails中安裝React"
date: 2016-06-30 23:55:11 +0800
comments: true
categories: react
---

## 安裝 react gem

Gemfile 中加入
使用 sprockets-es6 來寫ES6
```
gem 'react-rails'
gem 'sprockets-es6', require: "sprockets/es6"
```
bundle install

## 安裝 react 檔案

會建立相關的檔案

執行 ` rails g react:install `
```
rails g react:install
```

會建立相關的檔案
```
Running via Spring preloader in process 15876
create  app/assets/javascripts/components
create  app/assets/javascripts/components/.gitkeep
insert  app/assets/javascripts/application.js
insert  app/assets/javascripts/application.js
insert  app/assets/javascripts/application.js
create  app/assets/javascripts/components.js
```


## 修改 application.js
app/assets/javascripts/application.js

把turbolinks 及 tree 移除，加入 react, react_ujs, components 目錄
```
 //= require jquery
 //= require jquery_ujs
 //= require react
 //= require react_ujs
 //= require components
```

## 修改環境設定檔

```
# config/environments/development.rb
MyApp::Application.configure do
  config.react.variant = :development
end

# config/environments/production.rb
MyApp::Application.configure do
  config.react.variant = :production
end
```

## 啟用 react-addons

在 application.rb 中啟用
```
MyApp::Application.configure do
  config.react.addons = true # defaults to false
end
```

## 撰寫第一支react

新增 ` app/assets/javascripts/components/hello.es6 `

```
window.Home = ( window.Home || {} );
window.Home = class Home extends React.Component{

  constructor(props){
    super(props);
  }

  render(){
    return(
      <div>
        Hello React and Rails
      </div>
    );
  }

}
```

在 view 中加上 component, 例如在 ' home/index.html.erb '

```
<%= react_component("Home.Index")%>
```
大功告成, 測試看看有沒有成功



