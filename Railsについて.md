### Railsについて
#### MVCベース  
```
M-model - データ管理用  
V-view - 画面表示用  
   (html,css,js,...)
　C-controller - backend process実装用
　　(Ruby,...）  
・ViewとControllerの紐つけとして、route.rbでリンクの定義をする必要があること。  
・Model作成した後に作ったモデルデータがDBに反映するため、migrateする必要があること。   
　　rake db:migrate  
・なには特殊な処理をする時は必要なGemをインストールする必要があること。  
　例えば、画像アップロードならmini-magick gem  
・HttpRequestsのPutとPatchの使い分け  
　両方とも編集ようリクエストですが、PUTなら、一部編集したいのにあるリソース全部送信する必要があり、PATCHなら編集したい一部だけのリソースを送信する必要なので、easier&safer.
```
#### scaffold作成
```
rails generate scaffold モデル名 カラム名1:データ型1 カラム名2:データ型 2
例：rails generate scaffold user name:string age:integer
```
#### modelをDBに反映
```
rake db:migrate
```
