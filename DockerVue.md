## Dockerfile
```
# Rubyは2.5.0以上で
FROM ruby:2.5.1

ENV LANG C.UTF-8
# for MySQL
RUN apt-get update -qq && apt-get install -y build-essential mysql-client
RUN apt-get install -y curl apt-transport-https wget && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y yarn
# nodejs環境の構築
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get install nodejs
RUN yarn add node-sass
RUN gem install bundler

WORKDIR /tmp
ADD Gemfile Gemfile
ADD Gemfile.lock Gemfile.lock
RUN bundle install

ENV APP_HOME /myTaskApp
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME
ADD . $APP_HOME

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
追加：テストコードようRSpec+Guard導入
　　Gemfileに
　　group :development, :test do
  　　gem 'rspec-rails'
  　　gem 'guard-rspec', require: false
  　　gem "factory_bot_rails"
　　end
　　を追加して、bundle installを実行
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

#### dockerコンテナ上yarn updateしたいとき
```
docker-compose run web yarn install
```

docker-compose run --rm web bundle install
