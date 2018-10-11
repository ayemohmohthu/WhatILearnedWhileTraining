### rubyインストール
　👉 https://qiita.com/takahiron/items/09bf1073b460c6fcd0fc
### ※ruby version切り替えが上手くできなかった。
　👉 https://qiita.com/opiyo_taku/items/3312a75d5916f6cd32b1  
    vi ~/.bash_profile  
    export PATH="~/.rbenv/shims:/usr/local/bin:$PATH"  
    eval "$(rbenv init -)"  
    source ~/.bash_profile  
### ※ mySqlインストール
 👉 brew install mySql
### ※ mySQLのパスワード設定。
 👉 mysql_secure_installation
### ※ Node.jsとnpm インストールとアップデート
 👉 https://qiita.com/jaxx2104/items/2277cec77850f2d83c7a
### ※ yarn install  
 👉 https://qiita.com/masterkey1009/items/50f95b1187646a7db385
### ※ node js exit  
 👉 process.exit();  
    process.exit(0);
### ※ webpacker とは  
 👉 Webpacker makes it easy to use the JavaScript pre-processor and bundler Webpack 2.x.x+ to manage application-like JavaScript in Rails.

### Project create(https://qiita.com/naoki85/items/51a8b0f2cbf949d08b11参照)
```
①ruby versions,rails versions,mysql installationを確認
②rails new アプリケーション名 -d mysql
③database.ymlにパスワード設定
④GemfileでDB確認
⑤bundle exec rake db:create
⑥bundle exec rake db:migrate
Gem:webpacker追加
⑦gem 'webpacker', github: 'rails/webpacker'
⑧yarn -v
⑨bin/rails webpacker:install
⑩rails webpacker:install:vue
11.rails g controller Home index
12.app/views/layouts/application.html.erbには<%= javascript_pack_tag 'hello_vue' %>を追加します。
13.routes.rbでは、home#indexをrootページに指定します。
  Rails.application.routes.draw do
    root to: 'home#index'
  end
14.JSのコンパイル
  bin/webpack
15.rails s
```
### TODOサンプルコード
##### POSTエラー
Can't verify CSRF token authenticity.Completed 422 Unprocessable Entity ~~  
👉 todo.jsに  
```
　　　import axios from 'axios'
     axios.defaults.headers.common = {
       'X-Requested-With': 'XMLHttpRequest',
       'X-CSRF-TOKEN' : document.querySelector('meta[name="csrf-token"]').getAttribute('content')
    };
```
### dockerに持っていく
```
yarnのバージョンチェックでエラー
 　config/webpacker.ymlの
 　　　check_yarn_integrity: false
 database.ymlに
 　host:db
 rails db:createとrails db:migrateを実行する必要がある。
 docker上のMysqlにDB作成が必要のため。
```
