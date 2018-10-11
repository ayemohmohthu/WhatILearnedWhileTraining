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
#### modlesで入力チェック
```
class User < ApplicationRecord
  validates :name, presence: true
  validates :phone, presence: true, format: {with: /\A[0-9-]{,14}\z/}
end
```
#### 入力チェック一覧
|入力チェック|説明|
|:--------------------------------|:--------------------------------------------------|
| ':presence => true | 未入力のチェックを行う |
| :length => { :minimum => 2, maximum=> 10 }	| 指定された値の長さをチェックする |
| :format => {:with => /^([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})$/i}	| 書式のチェックを行う |
| :acceptance => true	| 規約同意などのチェックボックスがチェックONにされているかを検証する |
| :confirmation => true	| パスワードなどの確認入力項目が、比較元項目と一致しているかチェックを行う |
| :exclusion => { :in => %w(www us ca jp) }	| 指定された値が含まれないことをチェックする |
| :inclusion => { :in => %w(small medium large) }	| 指定された値が含まれることをチェックする |
| :numericality => { :only_integer => true }	| 入力値が数値であるかチェックする |
| :uniqueness => true	| 入力値がユニークであるかチェックする |

#### Railsでhtml（テンプレート）中にif文を書きたい時
https://qiita.com/devdyaya/items/50d5527983d4c91ae6da

```sample.html.erb
<div class="sample">
  <% if foo == 'bar' %>
    hogehoge
  <% elsif foo == 'piyo' %>
    fugafuga
  <% else %>
    piyopiyo
  <% end %>
</div>
```
#### RubyOnRailsでdropdownlistを使用したい時
```
<%= f.select :desired_attribute, ['option1', 'option2']%>
<%= f.select カラム名, [["表示する文字”, "データベースに登録する値”]] %>
<%= form.select :status, [['未着手', '0'],['作業中', '1'],['完了', '2']] %>
```
