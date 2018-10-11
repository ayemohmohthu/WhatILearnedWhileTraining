## Dockerfile
```
※ Rubyは2.5.0以上で
FROM ruby:2.5.1
※ yarnインストール
RUN apt-get install -y curl apt-transport-https wget && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y yarn
※ Node.jsインストール
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
　　 apt-get install nodejs
※ Cannot find module node-sass対策
RUN yarn add node-sass
```
## Gemfile
```
source 'http://rubygems.org'
gem 'rails', '5.2.1'
gem 'mysql2'
```
##### Dockerfile、Gemfile、Gemfile.lock、docker-compose.ymlを準備した後、以下のURLの手順のdocker-compose buildから順番通り実行すれば、Docker環境のRailsPjにVue.jsが入れれる。  
https://qiita.com/asapontenhou/items/507558aad52dc7dd2b32
```
①docker-compose build
②docker-compose run --rm web /bin/bash
③rails new . --webpack=vue -f -d mysql
④config/database.yml編集
　　password: <%= ENV['MYSQL_ROOT_PASSWORD'] %>
　　host: db
⑤rails db:create
⑥rails g controller home index
⑦config/routes.rb編集
　Rails.application.routes.draw do
   root to: 'home#index'
　end
⑧app/views/hoge/index.html.erbの先頭に追加
　　<%= javascript_pack_tag 'hello_vue' %>
⑨プロジェクトのリポジトリにProcfile.devファイル作成
　　web: bundle exec rails s -b 0.0.0.0 -p 3000
　　# watcher: ./bin/webpack-watcher
　　webpacker: ./bin/webpack-dev-server
⑩binの配下にserverファイル作成
　　#!/bin/bash -i
　　bundle install
　　bundle exec foreman start -f Procfile.dev
11.Gemfile編集
　　group :development do
　　　# Access an interactive console on exception pages or by calling 'console' anywhere in the code.
　　　gem 'web-console', '>= 3.3.0'
　　　gem 'listen', '>= 3.0.5', '< 3.2'
　　　# Spring speeds up development by keeping your application running in the background.Read more: https://github.com/rails/spring
　　　gem 'spring'
　　　gem 'spring-watcher-listen', '~> 2.0.0'
　　　# ここを追加
　　　gem 'foreman'
　　end
12.bundle install
13.config/environments/development.rbに追加
　　# Cannot render console from Allowed networks 対策
  　config.web_console.whitelisted_ips = '0.0.0.0/0'
14.docker-compose build
15.docker-compose up
```

##### Local環境で動いていたPjが表示される場合、
```
~ ❯❯❯ lsof -wni tcp:3000
COMMAND  PID         USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
ruby    4671 ayemohmohthu   10u  IPv4 0x4d493c2ee305c013      0t0  TCP 127.0.0.1:hbci (LISTEN)
ruby    4671 ayemohmohthu   11u  IPv6 0x4d493c2ee0de18cb      0t0  TCP [::1]:hbci (LISTEN)
~ ❯❯❯ kill -9 4671
```
